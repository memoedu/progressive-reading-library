I recently started using the [µBlock](https://chrismatic.io/ublock/) ad blocker. One advantage it has, in addition to blocking ads, is a nice interface for blocking arbitrary CSS elements from pages. This turns it from a single-purpose ad blocker into an incredibly versatile tool that can displace any number of site-specific addons.

For instance, I decided I didn’t like the sidebars or newsfeed on Facebook, because I never used them and they cluttered up the interface. Ordinarily I could install an add-on like [Social Fixer](http://socialfixer.com/) to get rid of them, but that requires another addon (and more bloat) for each site. With µBlock, I could just right-click the offending parts of the page and block them.[1](https://www.benkuhn.net/ublock/#fn:1)

Another great thing about uBlock is that the configuration is text-based, so you can share snippets easily. You can borrow my Facebook blocked by pasting the following lines in the “My filters” section of your config:

```
www.facebook.com###leftCol
www.facebook.com###rightCol
www.facebook.com###topnews_main_stream_408239535924329
www.facebook.com###pagelet_sidebar
```

If you also hate Facebook messages, like I do, you can block those too (don’t do this without warning your friends first):

```
www.facebook.com##.jewelButton[name="mercurymessages"]
www.facebook.com###pagelet_dock
```

The Stack Overflow “network questions” sidebar is distracting too, so it gets nuked:

```
stackexchange.com###sidebar
stackoverflow.com###sidebar
```

If you use an app like [MightyText](https://mightytext.net/) that has a bunch of buttons for “pro version only” features cluttering it, you can tear them out:

```
mightytext.net###share-button-main[href="#"]
mightytext.net###goProWrapper
mightytext.net###events
```

And of course it makes a decent procrastination-blocker:

```
news.ycombinator.com###hnmain > tbody > tr:nth-of-type(3) > td
www.reddit.com##.content
```

Now if only it would [replace the blocked elements with cat pictures](https://www.benkuhn.net/fbcat).

* * *

1.  The “block element” menu item brings up a dialog with a list of progressively more general selectors that you might want to block. You can click on each item to highlight the area of the page that it would block. I usually just click down the list and find the lowest-down item that doesn’t block anything I want to keep. [↩︎](https://www.benkuhn.net/ublock/#fnref:1)