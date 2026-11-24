As of the last couple years, [every](http://mypy-lang.org/) [popular](https://flow.org/) [dynamic](https://www.typescriptlang.org/) [language](https://medium.com/byteconf/stripe-is-building-a-ruby-typechecker-d6cd7cee6abf) has [optional](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration) [static](https://docs.hhvm.com/hack/overview/typing) [types](https://support.microsoft.com/en-us/help/311329/option-explicit-and-option-strict-in-visual-basic-net-and-in-visual-ba). Why is this such a convergent feature of language designs?

The obvious answer:

> Static types speed up large projects, but slow down small projects. The most popular dynamic languages have many “small projects” that became very large, and so their “enterprise users” want to retrofit static types onto them.

This may have been the impetus for many gradual typing projects, but I don’t think it can explain the explosive growth in interest. Writing Python today, I want to add types to anything larger than a tiny script—the biggest point of friction is `from typing import` and that I have to run `mypy` manually.

Instead, my theory of the success of gradual static types is that they’ve actually ended up as the _optimal point_ in language design space, for a few underappreciated reasons.

### Low syntactic noise

All the gradual type systems I cited were designed to easily allow adding annotations to existing code. That _forces_ the system to have great type inference, because users won’t accept the extra labor of adding redundant annotations.

Most gradual type systems seem to have converged on a happy medium for inference: require annotations for function signatures, but (almost) nothing else. The type-signature requirement was probably originally chosen to make inference computationally tractable. But it’s also the only place where type annotations are ergonomically important, because it’s the place where it’s hardest for the _reader_ to figure out the types from context.

### High expressiveness

Another thing constraining these type systems is that existing code wasn’t designed with static types in mind—for instance, using heterogeneous dictionaries where a statically typed language might use classes. That forces the type systems to add expressive features like [typed dicts](https://mypy.readthedocs.io/en/latest/more_types.html#typeddict), [structural](https://mypy.readthedocs.io/en/latest/protocols.html) [typing](https://www.typescriptlang.org/docs/handbook/interfaces.html), [unions](https://mypy.readthedocs.io/en/latest/kinds_of_types.html#union-types), [intersections](https://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types), and [literal](https://www.typescriptlang.org/docs/handbook/advanced-types.html#string-literal-types) [types](https://mypy.readthedocs.io/en/latest/literal_types.html). They’ll even [analyze](https://mypy.readthedocs.io/en/latest/casts.html) your [control flow](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-guards-and-differentiating-types) to constrain your types further.

As language features, the expressiveness and clean syntax of gradual types are great, but not exactly an innovation—they’re shared with many static-only languages (except for maybe the control flow analysis). But gradual type systems also have great features that no static-only type system I know of has achieved:

### Seamless dynamic code

If you have some code that you don’t want to type, a gradual type system makes it easy for you to do that. This doesn’t sound so great (we should type everything, shouldn’t we?) but it’s really useful in a couple situations.

**Metaprogramming:** Code that dynamically creates classes or functions (for instance, an object-relational mapper) is often very hard to type soundly. In a statically typed language, that means you have to resort to code generation, which is much more laborious to write.[1](https://www.benkuhn.net/gradual/#fn:1) It’s probably not an accident that Java’s [Hibernate](https://github.com/hibernate/hibernate-orm) is 10x the code of Python’s [SQLAlchemy](https://github.com/sqlalchemy/sqlalchemy/issues).[2](https://www.benkuhn.net/gradual/#fn:2)

**Testing:** Test doubles like [mocks](https://en.wikipedia.org/wiki/Mock_object) and [spies](http://xunitpatterns.com/Test%20Spy.html) are _way_ more verbose without the [escape hatch](https://www.benkuhn.net/hatch) of dynamic code. Some languages have frameworks like [Mockito](https://site.mockito.org/) to make them less painful, but the contortions they have to go through to make the types work are often horrifying. By contrast, a fully general mock is easy to implement with Python’s `getattr`, Ruby’s `method_missing`, etc.

### Unsoundness

A related benefit that you can’t get from static-only type systems is the ability to _lie about types_.

Why is this useful? Take the example of Python’s [`unittests.MagicMock`](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.MagicMock). It doesn’t inherit from any object, but it’s a drop-in replacement for nearly anything. That would be forbidden in a type system like Java’s, but most gradual type systems will let you lie that the `MagicMock` is actually an `AnnoyingToInstantiateDependency` or whatever.[3](https://www.benkuhn.net/gradual/#fn:3)

This comes up in practice more than you might think. For instance, the mypy codebase annotates types that are lies with [`Bogus`](https://github.com/python/mypy/pull/5467/files/5e21f6a54ad1a9ea9872571f84998c25ac863e17#diff-af82c5d1f153051e70cd73fa5b5e6845) so that the `mypyc` static compiler doesn’t make wrong assumptions about their type; I count about 35 Boguses in mypy v0.660.

* * *

Five years ago, it seemed to me like static languages were obviously better for large codebases. They gave you two huge productivity boosts:

-   You could catch many trivial bugs without actually running your code.
    
-   You could have reliably good [code completion](https://www.benkuhn.net/autocomplete).
    

I was confused why anyone would choose a dynamic language for something they expected to be a big project. But since then, dynamic languages have leapfrogged static ones. They captured the same productivity boosts—and they extended the idea of a type system in ways that let them stay cleaner and easier to write.

* * *

1.  If your statically typed language supports macros, you can also use macros. But macros are notorious for being hard to manage both for language designers and for users. I think macro systems have made progress recently, but I don’t have enough experience using them to compare them to metaprogramming in a dynamic language. [↩︎](https://www.benkuhn.net/gradual/#fnref:1)
    
2.  As counted by `git clone sqlalchemy/sqlalchemy;` `git clone hibernate/hibernate-orm;` `sloccount sqlalchemy hibernate-orm`. This comparison is somewhat unfair, since Hibernate seems to be [more popular](https://stackshare.io/stackups/hibernate-vs-sequelize-vs-sqlalchemy), and Java is exceptionally verbose and has a culture of crappy architecture. It’s also possible Hibernate has more features or something. Still, it’s hard for me to imagine those making a 10x difference on their own. [↩︎](https://www.benkuhn.net/gradual/#fnref:2)
    
3.  Of course, in this case you could extract an interface and make the client take an `IAnnoyingToInstantiateDependency`, or use Mockito or something. But extracting interfaces everywhere can lead to huge boilerplate problems (as anyone who’s tried to read a complicated Java library can tell you), and Mockito is a whole complicated library built to solve the single use case of lying about mocks. [↩︎](https://www.benkuhn.net/gradual/#fnref:3)