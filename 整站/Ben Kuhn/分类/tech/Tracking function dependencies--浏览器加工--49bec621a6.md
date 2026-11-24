I recently started looking into dependency tracking in Python—determining which pieces of code and data are required to compute a particular function. This led to a sequence of journeys into the weeds of cPython and various crazy interpreter hacks. Probably don’t try anything in this post at home, but hopefully they’re fun to learn about, at least.

## Problem statement

I’ve been doing a lot of data science work recently, and I’ve been noticing more and more how current tools suck.

There’s one problem that I’ve been facing particularly much as our models get larger and more complex: _lack of incremental evaluation._ Consider the following scenario:

-   You want to build a model of, say, loan repayment behavior.
    
-   You have about a million rows, each with about a thousand different features.
    
-   Each feature has its own (expensive) preprocessing algorithms.
    
-   You extract out-of-sample predictions for each data point via [repeated cross-validation](http://en.wikipedia.org/wiki/Cross-validation_%28statistics%29).
    
-   You then feed these predictions into one of several decisioning algorithms which build a loan portfolio based on various heuristics.
    
-   You then evaluate this portfolio on a number of metrics (e.g. predictive accuracy, rate of default, overall return), and plot the results.
    

This doesn’t seem like an uncommonly complex setup for a real-world machine learning problem. And yet by default, if you want to change the very last step—the metrics evaluating the decisioning algorithm—you need to re-run the entire script, which will pointlessly recompute the transformed dataset, cross-validated predictions, etc.

One thing that might help with this problem is a sort of “super-[memoizer](http://en.wikipedia.org/wiki/Memoization)” that caches results to function calls, not only during a single run of the program, but persistently across multiple runs. If all layers of the above toolchain were cached appropriately, then only the metrics that you change (and the resulting plots) would have to be recomputed.

In fact, there are already some tools that do this, like [Make](http://www.gnu.org/software/make/). The big problem with getting them to work with a data science pipeline is that they’re not particularly expressive for things that are more complex than a preprocessor/compiler/linker chain, like the stuff I described above. So it would be useful to have such a thing built into the actual libraries you were using.

Unfortunately, this is a lot harder of a problem than Make has. If all of your steps are different scripts, then you only need to keep track of their input and output files, and whether the script itself changed. But if your steps are different functions in the same file, then your super-memoizer potentially need to keep track of _all of the global state that each function accesses_. How could you do that?

It turns out that there actually are fairly reasonable ways to do this, although none of them is 100% effective (that would require storing a prohibitively large amount of state). I’ll give some examples in Python, all of which involve deep interpreter voodoo but are pretty interesting.

## `sys.settrace`

If you want to track dependencies on function, you can use some tools meant for debuggers. Python’s [`sys.settrace`](https://docs.python.org/2/library/sys.html#sys.settrace) allows you to specify a function that will be called every time:

1.  A function is called;
    
2.  A line of code is executed;
    
3.  A function returns;
    
4.  An exception is raised;
    
5.  A C function is called;
    
6.  A C function returns;
    
7.  A C function raises an exception.
    

To track function calls, we just need to write an appropriate tracing function:

```
def tracer(frame, event, arg):
    if event == 'call':
         print frame.f_code.co_name
sys.settrace(tracer)
```

## Overriding the function globals

Of course, tracking the function graph is only part of the process. If the functions to be super-memoized depend on global constants that change frequently, then the programmer will be stuck remembering to dump the cache manually, which is pretty error-prone.

There’s a solution to this that seems pretty nice to me, but unfortunately it only works on Python 3. It turns out that you can construct function objects with a specified [`globals` dict](https://docs.python.org/2/library/types.html#types.FunctionType), like so:

```
import types

a = 1
def foo(): return a

# the constructor is types.FunctionType(code, globals)
foo2 = types.FunctionType(foo2.func_code, {'a':2})
foo2() # 'a'
```

The trick is to replace the globals dict with an object that inherits from `dict` and logs all of its item accesses by overriding `__getitem__` and `__setitem__`. The reason this works only in Python 3 is that in Python 2, global variable accesses call `dict.__getitem__(globals, varname)` directly instead of doing virtual dispatch, which means you can’t override them. Fortunately, Python 3 is more flexible here.

## Disassembling the function

What if we want to replicate this behavior in Python 2? As far as I know, there’s no way to do it when the function runs—you just can’t get the globals dict to log all its accesses. But you can cheat by reading the function code ahead of time!

Every global dict read is triggered by a specific Python bytecode operation, `LOAD_GLOBAL` (and similarly, reads are triggered by the `SET_GLOBAL` opcode). You can read the function’s bytecode using the [`dis`](https://docs.python.org/2/library/dis.html) module and mark down every time these instructions are used to get a list of all attribute accesses that occur.

This approach works on Python 2, but it’s worse than the previous one, for a couple reasons.

1.  It records too many global accesses: if you have a global access in a branch that doesn’t execute, this approach can’t tell, but the previous one can.
    
2.  It misses some global accesses—namely, those done with `globals()['foo']` instead of `global foo`.
    

But at least it works on Python 2.

## Caveats

None of the solutions I’ve described is complete as stated. For instance, they won’t wrap function closures correctly (they’ll leave out dependencies accessed from the enclosing scope). They also only look at _immediate_ global dependencies, so they’ll track a reference to (for instance) `django.core.management` as a dependency on the toplevel `django` module. You might be able to fix this with even more clever hacking, but it would be more fragile as well.

## Other solutions

There are other ways of solving this problem, too, most of which I ruled out because they were clunky. For instance, you could make clients of the super-memoizer explicitly declare all global dependencies, but that would involve a lot of annoying boilerplate.

## Conclusion

Frankly, it’s probably hopeless to retrofit a complete dependency checker onto Python, or most languages that weren’t designed for the purpose. The syntax simply isn’t going to be elegant enough to make it hassle-free. Plus, the libraries weren’t designed to be friendly to dependency trackers, so a perfect tracker might end up with ridiculously large and complex dependency graphs.

But that doesn’t mean that even a limited solution can’t be somewhat helpful. Even if the abstraction [leaks too much](http://www.joelonsoftware.com/articles/LeakyAbstractions.html) to remove the mental load of manually marking when dependencies change, they can, for instance, act as a backup to make sure the programmer doesn’t forget to mark this in the common case. I’m still not sure what the right solution is to the problem I posed in the first paragraphs of this post, but I think some kind of automated dependency tracking is at least part of the puzzle.