Many computer systems offer _autocomplete_: the ability to guess what you’re about to type based on the first few letters you’ve typed. For instance, if you open up IPython and create a string variable, you can do this:

```
In [7]: s = 'foo'

In [8]: s.s<TAB>
s.split       s.splitlines  s.startswith  s.strip       s.swapcase
```

I’m used to thinking of autocomplete as a convenience tool that saves you a few keystrokes, but it’s much more than that. Good autocompletion has become a driving factor in which tools I choose. If I were writing a sophisticated user interface today—say, a programming language or a complex application—autocompletion is one of the primary constraints I would design it around. It’s that important.

* * *

Tons of folks (including me) swear by zsh as a huge improvement over bash. But most of us barely use the actual features that are exclusive to zsh (RPROMPT? advanced globbing? “improved array handling”?) Instead, for most people the killer feature of zsh is that _it doesn’t re-output the prompt when you autocomplete_. Compare…

Zsh:

```
 test % cd fo<TAB>
foo1/  foo2/  foo3/  fop/
```

Bash:

```
test$ cd fo<TAB><TAB>
foo1/ foo2/ foo3/ fop/
test$ cd fo
```

In zsh, the cursor stays on the first line, whereas in bash, the cursor moves to the third line. (bash also requires you to hit `<TAB>` twice—the first time you press it, you get a bell sound but no completions. Gee, thanks, bash!)

That one extra line is the single biggest reason why bash users leave. Because it means that bash barfs out a different set of completions every time you tab (twice):

```
test$ cd fo<TAB><TAB>
foo1/  foo11/ foo12/ foo13/ foo2/  foo3/  fop/
test$ cd foo<TAB><TAB>
foo1/  foo11/ foo12/ foo13/ foo2/  foo3/
test$ cd foo1<TAB><TAB>
foo1/  foo11/ foo12/ foo13/
test$ cd foo1
```

Once your directories start having more than 10 files in them, you’ll end up with an unreadable mess in your terminal history. Zsh, by contrast, will delete your previous completion lists when it makes a new one, avoiding the mess entirely. It’s a dramatically more pleasant experience when you autocomplete shell commands a hundred times a day.

* * *

I can point to exactly one tool that has approximately doubled my programming speed, and that’s the IPython notebook (as an alternative to coding into a file and running it).

Part of this is because I can immediately see the results of my computations. But a larger part is because IPython autocompletion is hands-down the best way of learning about Python.[1](https://www.benkuhn.net/autocomplete/#fn:1)

Say I want to manipulate a string in Python. I could open up a new tab, google “Python string api”, remember whether I need Python 2 or Python 3, scroll through their [badly organized page on strings](https://docs.python.org/3/library/string.html), realize that the first Python 3 result for “python string api” _doesn’t actually describe the methods on_ `str`, search “python string methods” instead, realize I wanted the [“built-in types”](https://docs.python.org/3/library/stdtypes.html) docs, scroll wildly through their [long, spaced-out list of methods](https://docs.python.org/3/library/string.html) until I found one I wanted, context-switch back to my code, and repeat.

(I swear I wasn’t trying to pick a particularly convoluted example and this is _actually what happened_ when I looked for the string documentation while I was making this post)

Or I could take my string object that already exists in my notebook, type `s.<TAB>` and see whether any useful-looking methods appeared. Nice!

And that’s just with strings, which have relatively well-written documentation. Good luck navigating something less well-supported like [PyQT](http://pyqt.sourceforge.net/Docs/PyQt5/) with the written documentation! I hope you enjoy constantly translating from C++ to Python.

(As a side note, this is a big advantage of the Python-style `foo.bar()` instead of Julia-style `bar(foo)` for method calls. In Python you can type `foo.<TAB>` and get a list of `foo` methods. I’m not sure an equivalent operation exists in Julia, and it would be tough to design one that’s as easy to use.)

* * *

I spend most of my waking hours in a state of mild annoyance at Xcode. But it’s a great case study of autocompletion enabling fundamental interaction shifts.

Objective-C really _needs_ autocomplete in a way that many languages don’t, because in Objective-C all arguments are keyword arguments. (Mostly.) Your method calls will look something like[2](https://www.benkuhn.net/autocomplete/#fn:2)

```
[NSLayoutConstraint constraintsWithVisualFormat:@"foo" options:0 metrics:nil views:viewDict]
```

(I’ve left it to hang off the side of the page for effect.)

On the one hand, the method names can get pretty heinous. I’m glad that somebody else is invoking

```
[MTLBlitCommandEncoder copyFromTexture: sourceSlice: sourceLevel:
  sourceOrigin: sourceSize: toBuffer: destinationOffset:
  destinationBytesPerRow: destinationBytesPerImage:]
```

so I don’t have to.[3](https://www.benkuhn.net/autocomplete/#fn:3) The API writers could certainly use a lesson in concision. And the fact that Apple’s methods were designed for autocomplete means that the language is practically unusable in environments without it, which in practice means substantial lock-in to Apple’s proprietary dev tools since no one else invests as much.

On another few hands:

-   Blitting is a complex operation that needs to go really fast! If you want to blit something quickly you actually do need an eight-parameter method.
    
-   The reason we don’t usually need named parameters is that (a) there’s usually an obvious convention for the order that the parameters go in, so the programmer can guess the order; (b) if you mess up the order, the type system will usually catch your mistake. If you have eight integer parameters, both of these break down.
    
-   Even for methods with fewer arguments, transposing the order of two non-keyword arguments of is one of the easiest bugs to make and the hardest to catch in code review. I’ve worked at multiple companies where a bad enough bug could theoretically wipe out millions of dollars before anyone noticed. Keyword args are _great_.[4](https://www.benkuhn.net/autocomplete/#fn:4)
    

-   This style means that method names are largely self-documenting. Fewer documentation lookups means fewer context switches, which means faster programming. It also means that it’s easier to tell if a method is the one you want _from inside the autocomplete window_ instead of searching for it in the documentation, which reduces context-switching even more.
    
-   Trying to shorten names “to save typing” is one of the great scourges of interface design, because you end up switching between e.g. `source` and `src` depending on how much they want to save space. Good autocomplete means you can implement an obvious rule: just don’t do that!
    

Ruthie often tells me that “keystrokes are cheap.” But Objective-C shows that this isn’t quite true, at least in big enough aggregate. Saving one or two keystrokes isn’t usually worth the trouble, but Xcode can save programmers tens of keystrokes per method call. By the tens or hundreds, keystroke costs do add up—both in terms of time spent typing, and of chance to make a typo that costs further time to correct and recompile. Removing that kind of cost is a big enough deal that it’s transformed how Objective-C interfaces are designed.

* * *

Before I discovered keyboard shortcuts, I used to spend a ton of time looking for things in menus. That’s why one of my favorite features in OS X is the search bar in the Help menu. In newer versions of OS X, `⌘-⇧-/` (a.k.a. `⌘-?`) opens up the Help menu and plops your cursor in a search bar that can search through every menu item in the application:

[![](https://www.benkuhn.net/autocomplete/help-shot_hu8c2b33ef8f090eaecef7414dc57287be_136469_702x506_resize_box_3.png)](https://www.benkuhn.net/autocomplete/help-shot.png "full size")

The only reason I ever browse menus anymore is out of habit. (And because the search isn’t quite as great as it could be—it can still be more efficient to use menus if you know exactly where your target is.) `⌘-<space>` for Spotlight serves a similar function for apps, files, and other miscellaneous computer objects, and it’s replaced my need for the dock,

Gmail’s contact autocompletion means you barely even need to remember what your contact’s name is, let alone their email. Google search autocompletion means you don’t even need to know what you’re looking for—type in some vaguely relevant words and Google will guess what you mean.

And of course Emacs autocompletion deserves a special mention, especially as augmented by [helm](https://github.com/emacs-helm/helm). I have 90 files open in emacs right now and I can find any particular one in five keystrokes. Imagine having 90 tabs open in Chrome—you could never find anything! With Emacs I don’t even need a tab bar because it’s so simple to autocomplete the buffer I need. In fact, I’ve set it up not to distinguish between files on disk and files that are already open—when I ask emacs “switch to this buffer” I’m really asking it “open this file (or show the buffer if it’s already open).” And it’s still fast and seamless.[5](https://www.benkuhn.net/autocomplete/#fn:5)

* * *

So why is auto-completion so great?

First of all, the keyboard is a much higher-bandwidth interface than pointing and clicking. If you type 60 words per minute you’re typing 4 characters per second, for 24 bits of entropy per second. To match that precision with a mouse would require you to be able to identify and navigate to any thumbnail-sized part of your screen in a third of a second.[6](https://www.benkuhn.net/autocomplete/#fn:6) Maybe barely doable if you’re very dextrous.

The problem with most keyboard-based interfaces is that, even if the input is high-bandwidth, the output isn’t. For instance, my text editor appears on a screen with 3.5 million pixels (100 megabits of entropy if we’re being generous), but it’s barely using any of that to display novel information—every time I press a key, I can predict almost exactly what my file will look like afterwards. I’m giving the computer a lot of information, but I’m not getting very much back.

* * *

Here’s why the high-bandwidth visual interface is important: The common thread in between the autocompletion in zsh, IPython, Xcode and OS X help is that it _makes it easy to find hidden things_.

As computers get more complex, the number of objects they have to deal with—files, functions, menu items, or anything else—explodes. The easiest way to tame the complexity is to hide these objects away until you, the user, call them out by name. But the computer is dumb, so you have to get the name _exactly right_. You have to pick exactly the right menu sequence, or type out the method signature precisely, without any feedback process—it’s like shooting a bullseye in the dark. Autocomplete turns on the lights.

* * *

1.  Which is not saying much, considering the huge number of obvious ways that IPython could work way better. But that’s a topic for another post! [↩︎](https://www.benkuhn.net/autocomplete/#fnref:1)
    
2.  This is an actual method call in the Wave codebase (with the parameters scrubbed). Found by tabbing to Xcode and skimming the file I was currently editing until I found a wrapped line. [↩︎](https://www.benkuhn.net/autocomplete/#fnref:2)
    
3.  This, on the other hand, is the longest public method signature in the iOS libraries. Source: [https://github.com/Quotation/LongestCocoa](https://github.com/Quotation/LongestCocoa) [↩︎](https://www.benkuhn.net/autocomplete/#fnref:3)
    
4.  So are unit tests, static analysis, assertions, and so on—but when a lot is at stake, it’s nice to have defense in depth. [↩︎](https://www.benkuhn.net/autocomplete/#fnref:4)
    
5.  Of course Helm is useful for way more than just buffers—it does everything from files to elisp commands to function names to copy/paste history—but you can read about that elsewhere if you’re interested. [↩︎](https://www.benkuhn.net/autocomplete/#fnref:5)
    
6.  A thumbnail is about 0.5 in^2. The 15-inch Macbook Pro is ~100 square inches of display, so there are 200 thumbnails to a screen, or ~8 bits of entropy. [↩︎](https://www.benkuhn.net/autocomplete/#fnref:6)