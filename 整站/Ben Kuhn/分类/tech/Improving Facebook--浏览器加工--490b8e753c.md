Today I realized that Facebook’s newsfeed isn’t useful. I still want to use Facebook, because I chat with people and get invited to events and things, but every time I visit the site I get distracted by the newsfeed and it never contains anything that I’m glad I read.

So I replaced it with a kitten picture.[1](https://www.benkuhn.net/fbcat/#fn:1)

To make this happen I used the Stylish extension ([Firefox](https://addons.mozilla.org/en-US/firefox/addon/stylish/), [Chrome](https://chrome.google.com/webstore/detail/stylish/fjnbnpbmkenffdnngjfgmeleoegfcffe?hl=en)) and added the following user style:

EDIT 2: I accidentally killed the Gist when changing Github usernames (don’t ask), and I no longer have this code. Sorry!

The reason I no longer have this code is that I now use [µBlock](https://chrome.google.com/webstore/detail/%C2%B5block/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en) for this. While it doesn’t display kittens, it does have the very nice feature that you can copy and paste/export your entire config at once. Since I have multiple browser profiles, this is huge—it makes it a lot easier to sync blockers between them.

Here’s the relevant section of my µBlock config:

```
www.facebook.com###leftCol
www.facebook.com###rightCol
www.facebook.com###topnews_main_stream_408239535924329
```

The first line blocks the left sidebar, the second line blocks the right sidebar, and the third line blocks the newsfeed.

As before, please comment if this code doesn’t work for you, and I’ll try to cook up a replacement.

* * *

OBSOLETE EDIT: sometimes Facebook changes their HTML and breaks things. I’ll try to keep this Gist up to date with the version that works for me, but if it doesn’t work for you, try messing around with the CSS—it’s hopefully pretty self-explanatory.

* * *

1.  Unfortunately, my screenshot was deleted in an accident involving an unversioned S3 bucket and a badly-written shell script. I no longer use the kitten script, so you’ll have to imagine it. [↩︎](https://www.benkuhn.net/fbcat/#fnref:1)