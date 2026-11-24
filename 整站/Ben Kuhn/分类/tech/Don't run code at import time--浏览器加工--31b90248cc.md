In Python and a lot of other dynamic languages, there’s no distinction between loading a module and executing a script. When you write a file named `main.py` reading:

```python
class Example:
    def sayhi(self):
        print("Hello world!")
```

you’re actually instructing the Python interpreter to construct a new instance of `function`, construct a new instance of `type`, assign the `function` object to a field on the `type` object, and assign the `type` object to a field on the `main` module. That code is almost exactly the same as writing

```python
Example = type(name='Example', bases=(object,), dict={
    'sayhi': lambda self: print("Hello world!")
})
```

This might seem natural if you’ve used dynamic languages for a while, but it’s not how most early languages worked at all. In C or Java, the top level statements in a source code file don’t get executed at all—they _declare_ functions or classes that can be invoked later.

Blurring the distinction between declaration and execution is simpler in some ways. For instance, it makes it a lot easier to write quick scripts: compare Python’s `print("Hello, world!")` to Java’s

```java
class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!")
    }
}
```

It also makes it easier to do fun dynamic things, like add new methods to classes defined in other files. And it makes it a lot easier to initialize complicated global data structures.

But executable modules also make some things a lot harder—or worse, prone to subtle errors. Since there are other, better ways to get most of the benefits above, I think executable modules are a misfeature in most languages that use them. Here are some of the ways they can go wrong.

_Note: most of my examples below are drawn from Python, because that’s the language I know best. I suspect most of them carry over to other languages with executable modules as well._

### Import existence/order dependency

In Java, `import foo` means “I depend on the `foo` module; please have the virtual machine link me to it when I run.” In Python, `import foo` means “look for `foo.py` in the path, execute it, store local variables in a module object, and assign that module to the `foo` variable in my scope.” The difference is that the Python version can run essentially arbitrary code, including importing other modules which then import other modules in turn.

This wouldn’t be so bad except that lots of modules decide to call setup code when they’re imported, and sometimes the setup code has hidden gotchas in how it’s invoked. For instance, Wave once had a bug where our logs randomly switched from our custom formatting back to the default Python formatting. There was no smoking gun—nobody had touched the logging code in forever—so we were confused for a long time about what could have happened. We eventually traced the problem to a commit which imported a module that called our logging configuration code at top level (because it was originally written as a script). This resulted in two calls to the logging configuration code, which for some reason caused most of the formatting to go away instead of being idempotent as you might expect.

### Import cycles

Running code at import time also runs the risk of introducing _circular imports_, as in:

```python
## async_tasks.py
import email
# ...
send_email = define_task('send_email', email.send_email)

## email.py
import async_tasks
# ...
def handle_incoming_email(email):
    async_tasks.send_email(FORWARDING_ADDRESS, email)
```

Note that if you import `async_tasks` first, this code will work fine, but if you import `email` first, it will throw an exception. That means that if your code ever gets imported in a different order from usual, it might suddenly go up in flames.

That might seem like an arcane issue, but it’s relatively likely if your application has multiple entry point modules—say, one to handle HTTP requests and one for a task runner like [Celery](http://www.celeryproject.org/). In that case, the entry points are likely to import other modules in different orders. This could also happen if you’re writing a library where your clients could import submodules in different orders—one client might run perfectly fine while the other goes up in flames as soon as it’s started.

Here it’s obvious that there’s an import loop, but it’s very easy to create one by accident—or convert a non-crashing import loop into a crashy one by turning an `import` into a `from ... import`. And few projects are disciplined enough to avoid (non-crashing) import cycles. For instance, here’s what Pylint reports for some notable Python projects I happened to have installed:[1](https://www.benkuhn.net/importtime/#fn:1)

Project | Number of cyclic imports

-   | - requests 2.11.1 | 0 flask 0.11 | 4 werkzeug 0.11.10 | 20 sqlalchemy 1.1.3 | 24 matplotlib 1.5.3 | 50

As the data shows, it’s difficult to avoid laying import-loop traps for yourself, unless your name is Kenneth Reitz.

### Risky

Not only does running code at import time increase the risk of bugs, but those bugs are also disproportionately risky. In many projects, a lot of instrumentation and monitoring (e.g. logging configuration) is set up in the application code entry point, _after_ modules are imported. As a result, this instrumentation and monitoring won’t properly report errors that occur at import time. Instead, you might end up, say, deploying a web application that just crashes endlessly in a loop, without warning anyone that it’s stopped serving requests. (Wave has never done this, but we’ve come reasonably close.)

### Slow

Executable modules are also much slower to import than declarative ones. For instance, importing Wave’s database models takes half a second (with a warm filesystem cache) on my machine,[2](https://www.benkuhn.net/importtime/#fn:2) because of all the SQLAlchemy metaprogramming. Importing the full Wave codebase takes almost 2 seconds. Presumably most of this code does need to get run at some point,[3](https://www.benkuhn.net/importtime/#fn:3) but not on startup, and the 2-second delay (on a relatively small app!) slows down iteration noticeably.

### Makes tools worse

The above points are reasons to use import-time code judiciously. But there are also reasons it’s bad even if you don’t do anything weird with it. For instance, the _possibility_ that a module might run import-time code makes it much harder to write tools that work with modules.

I really like writing code interactively in `ipython`: I’ll fire up a notebook and write my code in a notebook cell so that I can quickly run it and see where it fails. Unfortunately, often when I work this way I need to edit some code that lives on my filesystem, which means reloading the code for that module.

There’s a function in Python that’s supposed to handle this, which is `importlib.reload`. Unfortunately, it has tons of gotchas caused by the fact that Python modules are executable.

First, `importlib.reload` re-executes the entire module to be reloaded. This works fine in a lot of cases, but fails quite badly in some. For instance:

```python
# registry.py
REGISTRY = {}
def register(decorated):
    REGISTRY[decorated.__name__] = decorated
def handle(handler_name, arg):
    REGISTRY[handler_name](arg)
```

If you reload this module, it’ll blow away your registry.[4](https://www.benkuhn.net/importtime/#fn:4)

Second, reloading modules does not play nicely with `from ... import`. If the `server` module does `from app import handle_request`, and later `app` is reloaded, `server.handle_request` will still point to the old function. If modules weren’t executable, then `handle_request` could be late-bound in `server`, so that it automatically resolved to the new funtion once `app` was reloaded.

Finally, for similar reasons, reloading a module that defines a class with existing instances can break all of those instances:

```python
## breaks.py
class Foo:
    def hi(self):
        super(Foo, self)
        return 1

## ipython
In [1]: import breaks

In [2]: f = breaks.Foo()

In [3]: f.hi()
Out[3]: 1

In [4]: import importlib; importlib.reload(breaks); f.hi()
...
TypeError: super(type, obj): obj must be an instance or subtype of type
```

The problem is that `f` is an instance of the old `Foo`, which is different from the new `Foo`; but the `super` call in the code of the old `Foo` figures out what `Foo` is by looking it up in the `breaks` module, which means it’s being called with the new `Foo`.

If the module weren’t executable, the reloading infrastructure could transparently replace the old class definition with the new class definition. But since defining a class (and class methods) is just another Python statement, you can’t do anything smart with it; you need to execute it exactly the way the statement semantics tell you to. In fact, even if the `super` call issue were fixed (as it is in Python 3), you’d still be left with a bunch of instances of old classes with old methods—which makes reloading a lot less useful, since it doesn’t actually change the code of many of the objects you care about!

When I was doing machine learning, I’d often develop in an interactive environment so that I could keep my datasets loaded and test code on them quickly. Unfortunately, I would occasionally have to edit my codebase on disk and reload it, at which point I’d usually get stuck with so many broken instances that it was faster to reload everything from the beginning than to surgically replace all my instances’ `__class__` variables with the newly-loaded classes.

### Conceptually tricky

The root cause of all of these issues is that programmers _want_ to treat top-level module code as declarative. Most programming languages encourage this, for instance by referring to function and class “declarations” (rather than “assignments”). But under the hood there’s no declarativity; the values are pushed around by assignment and mutation and the semantics are completely imperative.

The bad thing about this is that it means there are _more things that matter_ in your code: in particular, executable modules make the order of definitions/imports relevant where it was irrelevant before. That breaks most programmers’ default mental models and prevents the programming language tooling from making a lot of optimizations it could otherwise make.

What do we get in exchange for this complexity? It’s a little bit easier to do metaprogramming and monkey patching, but this can be solved almost as well with a good macro system. It’s a little bit more concise to write scripts, but this can be solved just as well with a clean separation between “script” and “module” files. It’s a little bit easier to define complicated global constants, but this can be solved just as well with a good way to make those constants lazy (evaluated on first use). Given the alternatives, it doesn’t seem like executable modules are a great tradeoff.

_Thanks to Keller Scholl for reporting a typo._

* * *

1.  I calculated these numbers by running `pylint --disable=all --enable=cyclic-import <module>` on the source trees for the respective versions. [↩︎](https://www.benkuhn.net/importtime/#fnref:1)
    
2.  A mid-2015 15" retina MacBook pro with a 2.5 GHz Intel Core i7 processor, 16 GB RAM and 500 GB flash storage. Import takes 550-570 ms for our models and ~1.8 s for the whole app, as measured by repeatedly running `echo '%time import <module>' | ipython` and discarding the first few. [↩︎](https://www.benkuhn.net/importtime/#fnref:2)
    
3.  Actually, that’s not quite true: if our object-relational mapping layer used code generation instead of metaprogramming, it would eliminate a huge amount of import-time code. [↩︎](https://www.benkuhn.net/importtime/#fnref:3)
    
4.  You can make the module reloadable by not setting REGISTRY if it’s already set, e.g.: `try: REGISTRY; except NameError: REGISTRY = {}`. I’ve never seen anyone actually do this, but maybe that’s just because they care less about reloadable modules than I do? [↩︎](https://www.benkuhn.net/importtime/#fnref:4)