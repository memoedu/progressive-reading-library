…by spending 50% more time on the important parts instead of Slack/email/busywork/things I do when I’m unfocused. I actually didn’t even notice until I checked my monthly [RescueTime](https://www.rescuetime.com/rp/ben) report—at which point I realized I had gotten an extra 40 hours of engineering work in September compared to August.

I had no intuitive sense of that difference—if you’d asked, I’d have guessed my engineering time in September was roughly the same as August. It’s disturbing to me that I didn’t notice such a huge difference. On the other hand, a consistent difference of 80 minutes daily can easily be swamped by the 3-4 hour day-to-day swings depending on my schedule, level of focus and how much I feel like working. So I can sort of understand.

Here’s the comparison of my RescueTime reports:

| Category | August | September |
| --- | --- | --- |
| Very productive (engineering) | 93 h | 131 h |
| Productive (necessary busywork) | 40 h | 36 h |
| Neutral (communication) | 77 h | 60 h |
| Distracting (random stuff) | 18 h | 16 h |
| Very Distracting (news/reading) | 52 h | 45 h |
| TOTAL | 280 h | 286 h[1](https://www.benkuhn.net/50pct/#fn:1) |

As the table shows, it’s not just that I had more time in September. Instead, I got my programming gains by taking time away from other activities—roughly 50% from communication and 50% from other activities.

The decrease in communication time partly explains why I didn’t notice; the bulk of my communication happens in small chunks with frequent context switches to other things, so it’s hard for me to notice when it takes a lot of time. On the other hand, when I’m doing programming work I frequently get so absorbed that I lose track of time, so I don’t realize when I spend more time on that either. I suppose the point is that my impression of how I spend my time is incredibly unreliable!

It remains to be explained what actually caused this shift, though. I changed a lot of things about how I worked between August and September, and it’s not clear which ones were important. But here they are, in order of how sexy of a productivity tip they are.

**Meetings.** At the beginning of August I had an ad-hoc meeting schedule—I had one-on-ones on two different days of the week, a company all-hands meeting on Tuesday, hiring interviews scheduled throughout the week, and smaller one-off meetings whenever we felt like it. But I realized that every meeting destroyed some time beforehand (because I had to spend attention worrying about making it to the meeting on time) and afterwards (because I had to get back into a focused state). So I started batching my meetings on Tuesdays and alternate Fridays. I pretty immediately saw a huge increase in the amount that I got done on non-meeting days.

I get the sense that this doesn’t really work at larger companies, given e.g. how excited [this Facebook guy is](https://medium.com/facebook-design/your-calendar-is-a-mess-f9a0a3457fb1) that they have _one whole day_ where he doesn’t have any meetings. But I’ll enjoy it at Wave while it lasts.

**Closing Slack.** I realized that I spent a lot of time reading Slack in short bursts simply because it was sitting there in the middle of my Cmd-Tab switcher. I wrote [a piece of AppleScript](https://gist.github.com/benkuhn/6e975d297a432d1747a9) that used [Notifications Scripting](http://www.cooperative-fruitiere.com/notifications/index_en.html) to pop up a notification every 10 minutes reminding me to close Slack if I wasn’t waiting on a response from someone:

[![](https://www.benkuhn.net/50pct/slack-notification_hu1e468e7e9e00caf0dc4d3cd7ef806d8c_31616_716x194_resize_box_3.png)](https://www.benkuhn.net/50pct/slack-notification.png "full size")

The only sad part is that the Slack desktop app takes about 30 seconds to start up (yes, in 2015), which makes me close it less often than I otherwise would, and waste more time waiting for it to load than I otherwise would. (Slack people, are you listening? Stop re-coloring your plaid and fix your startup time![2](https://www.benkuhn.net/50pct/#fn:2))

**BS timeboxes.** I realized that I was spending a long time on things that needed to get done but didn’t feel important (a random recent selection: buying a mouse on Amazon, doing accounting, planning a trip, applying for Global Entry). My default pattern was basically doing things whenever I remembered about them and felt like I had time, or like they were sufficiently urgent. Instead of that, I added three weekly one-hour “BS timebox” events to my calendar and committed to working on urgent-but-unimportant tasks only during those windows. This both forced me to do them efficiently, and meant that I didn’t have to think about them for the other 163 hours in the week.

**Delegation and automation.** At Wave I ended up being assigned one particular task that interrupted me a few times a day—I had to look through our systems for transactions that encountered unexpected errors or got stuck and deal with them. I took two steps to fix this. First, I whined about it enough to our CTO that it got delegated to a less-technical employee (who would take care of the common cases herself and escalate confusing/rare ones back to me). Second, I refactored the transaction handling system so that it had a much easier and more uniform error handling interface. This made it much easier for us to automate the handling of new errors, which meant that we were gradually able to reduce the load of manual error-handling even further.

**Office.** Midway through September I moved into a private office about 10 minutes’ walk from my house. I’m very easily distracted by any sort of visual or audio interruption happening nearby me, so working from home, or from [an open office](http://www.benkuhn.net/oo), is distracting and unpleasant. Having a place that’s just me and my computer has done a lot for my focus.

**Moving.** In mid-August, Ruthie and I moved into our new group house. The move didn’t seem to affect the amount of time that I spent on the computer overall (since that didn’t change between the two months), but I definitely benefited in September from not having to worry about random house-related things as much.

_Edited Nov. 8 to clarify a few things for the easily-confused Hacker News readers._

* * *

1.  RescueTime tracks all time spent on the computer, not just time when I’m “at work.” I am not trying to work for 280 hours a month; that would be completely unreasonable. My actual work week is probably about 50 hours, and it’s only that much because (a) I have no commute and (b) I’m incredibly excited about my job.
    
    Also, I’m putting stuff I do for relaxation in the “Very Distracting” category not because I want to spend less time relaxing, but because I’d rather be spending that time on higher-quality relaxation that’s away from the computer. [↩︎](https://www.benkuhn.net/50pct/#fnref:1)
    
2.  Sorry, Slack people, I love your product and I [know how much work you have](http://scraps.benkuhn.net/2015/09/02/darkmatter.html). But I hope you get to this one eventually! [↩︎](https://www.benkuhn.net/50pct/#fnref:2)