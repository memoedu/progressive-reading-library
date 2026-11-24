Most code editors color different pieces of your program in different ways. For instance, they’ll make keywords like **`if`** bold and bright so that you notice when you’ve misspelled them. They’ll make non-executable parts like comments and documentation fainter so that you know that the computer isn’t seeing that part of the program. Take this example in Pycharm colors:

```
def frobnicate(swizzle):
    """Frobnicates the given swizzle."""
    pass
```

But isn’t this exactly backwards?

-   I want the syntax of my language to get out of my way, not jump into the foreground. I can easily recognize keywords, and I’d rather focus on the parts of the code with higher information density—method names, arguments, and so on.
    
-   If your code is locally readable, then there are only two times you need comments: (a) high level overviews and signposts; or (b) to call out some tricky non-local effect of the code. These are the two most important and least straightforward parts of programming: I want them _emphasized_, not faded out!
    

Take as an example the following function definition from the Zulip codebase ([source](https://github.com/zulip/zulip/blob/83d422d5bc7008725d38fb2a29d8da925e6234dd/zerver/lib/avatar_hash.py#L30)):

```
def user_avatar_path(user_profile: UserProfile) -> str:
    # WARNING: If this method is changed, you may need to do
    # a migration similar to
    # zerver/migrations/0060_move_avatars_to_be_uid_based.py .
    return user_avatar_path_from_ids(
        user_profile.id, user_profile.realm_id)
```

`def` is the least important part of this snippet—I know it’s a function. The comment is way more important: if I update the code without reading it, I’ll probably ship a bug.

So what if we flipped these two styles?

```
def user_avatar_path(user_profile: UserProfile) -> str:
    # WARNING: If this method is changed, you may need to do
    # a migration similar to
    # zerver/migrations/0060_move_avatars_to_be_uid_based.py .
    return user_avatar_path_from_ids(
        user_profile.id, user_profile.realm_id)
```

Seems better! The keywords fade into the background and I definitely won’t forget to write a migration. I’m not convinced about bolding every comment—it’s a little obtrusive—but I don’t have any better ideas and I’d rather read too many comments than not enough.

I’ve been using this color scheme for a few weeks now and it’s been fine so far! The most noticeable effect is that my source code is way less noisy and distracting. I think I’m paying attention to comments more (and leaving them to be stale less often), but that’s a lot harder to tell so I can’t say for sure.

Random notes:

-   I really do want to find something better than bold for comments.
    
-   I was curious why many languages today display keywords in bold so I looked up the history of syntax highlighting on Wikipedia. It sounds like the first IDE to bold keywords may have been [MacPascal](https://en.wikipedia.org/wiki/Syntax_highlighting#cite_ref-macpascal_9-0), which was written for a monochrome display. In that case it makes sense: with a monochrome display and one font, they could either make keywords bold or make everything else bold, and keywords was the less-intrusive option. But now that we have 16-million-color displays, maybe it’s time to revisit that.
    

-   I started thinking about this after discovering Robert Melton’s [No Frils](https://www.robertmelton.com/posts/syntax-highlighting-off/) Vim theme, which I noticed also has an option “to turn on high contrast comments rather than the default faded style, for projects where the comments are not deceit and lies.” Maybe the deceit and lies problem is why this isn’t more popular? On the other hand, it seems like fading out comments and docstrings probably makes the staleness problem a lot worse.

-   It’s also probably a lot easier to do this in codebases with familiar and obvious naming conventions (e.g. classes are `CamelCase`, functions are verbs, decorators are adjectives, timestamps start with `when_`…).
    
-   I tried to search around for any systematic studies of what kinds of syntax highlighting help, and I couldn’t find a single one. I’d love any pointers.