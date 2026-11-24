A little while ago, inspired by [Paul Christiano’s post](http://lesswrong.com/lw/kuw/changes_to_my_workflow/) about various habits he found useful, I decided to put a bit more work into my email processing. Paul had one email trick that sounded particularly useful:

> I set gmail to archive all emails on arrival and assign them the special label “In.” This lets me to search for emails and compose emails, using the normal gmail interface, without being notified of new arrivals. I process the items with label “in” (typically turning emails into todo items to be processed by the same system that deals with other todo items) at the beginning of each half day. Each night I scan my email quickly for items that require urgent attention.

I use email [asynchronously](http://www.benkuhn.net/comm), but was still checking it multiple times a day, and even when I didn’t immediately respond to most of my emails this took up a lot of attention. Enforcing processing only once a day sounded attractive. (I had previously tried doing this purely by force of will, and this was helpful but easy to slip from, e.g. when I was composing new emails or searching my history.)

I tried Paul’s exact solution briefly, but most of the Gmail user interface is designed with inbox processing, rather than label processing, in mind—this setup messes with, among other things, some keyboard shortcuts, [Boomerang](http://www.boomeranggmail.com/referral_download.html?ref=4sbhr),[1](https://www.benkuhn.net/inbox/#fn:1) and the “send and archive” button. I looked around for things that would let me just hide the inbox from the Gmail UI, but they didn’t seem to exist. So finally I wrote [a little piece of Google Apps Script](https://script.google.com/d/18vE3VXKW_1022176bzzkdtGwfU6Yv2JVeD-TzXxaGzbIu9FS4-KGUQFQ/edit?usp=sharing) to do the job.

While I was scripting my inbox, I realized that I could add a couple more enhancements:

-   I set the script to run once a day, between 8pm and 9pm (Google only lets you specify down to a one-hour window). I go to bed at around 10pm, so this naturally enforces that I don’t spend too much time answering emails.
    
-   I set the script to send emails marked “important” to my inbox, and “unimportant” to a separate label that I process once a week. This turned out to be unexpectedly very helpful: I can segment my email processing into parts where I need to make a conscious decision for every email, and parts where I’m basically just looking for the few surprising/interesting emails that look like unimportant ones to Google’s (pretty excellent) filters.
    
-   At some point, I might add a secondary function that takes emails labeled “Urgent” and moves them to my inbox more quickly, for the few threads that I need to monitor more than once per day (e.g. next-day logistics). I’m still adjusting to the fact that it’s now harder for me to do last-minute logistics by email, but I think overall this is positive since those are a big attention drain.
    

So far, I’ve found it very helpful. I typically only deal with 5-10 emails per day now (whereas before I was dealing with 25-50). On the weekend I go over another 100-200 emails but usually only for a few seconds each. So far the Gmail “important” filter has about a 3% false negative rate (emails that I wish it had flagged as important but it didn’t), and none of them have been actually necessary to respond to.

To use the script, make a copy of it in your own drive via the link above. Then follow the instructions written at the top of the script.

I hope this is helpful. Enjoy!

* * *

1.  This is a referral link which will give both of us a month of free paid service if you sign up for the paid plan. [↩︎](https://www.benkuhn.net/inbox/#fnref:1)