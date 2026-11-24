I used to be an anti-wire crusader. I hated the clutter of cables, and my tendency to unconsciously chew on them if they got anywhere near my face. But running into bug after tricky wireless bug—mostly while trying to make my video calls work better—I’ve apostasized. The more I’ve learned about wifi, Bluetooth and related protocols, the more I’m convinced that they’re often worse, on net, than wires.

For instance: most people, when their video call stutters, blame their Internet service provider. That’s understandable, since most ISPs are overpriced oligopolists with barely-usable software and horrible customer service. However, every time I can remember helping someone track down the source of their connection problems, the culprit has turned out to be their wifi. And often, the easiest fix has been to run a cable.

Wifi (and bluetooth, etc.) sucker you in by making it seem like they “just work.” But if you investigate, you’ll often find that the wireless link is operating in a degraded state that performs much worse than a wired equivalent. Since this degradation is silent, it’s often not obvious that the problem is the wireless—instead, you’ll probably conclude that it’s your device/software/self.

Over and over again, I’ve seen people fix some wireless-related problem and go [“wow, I had no idea how much better this could be!”](https://www.jefftk.com/p/ethernet-is-worth-it-for-video-calls)

[![](https://www.benkuhn.net/wireless/desk_hu18de0b9e12a1a70caedb5f95b671d2c6_5832704_600x400_resize_q75_box.jpg)](https://www.benkuhn.net/wireless/desk.jpg "full size")

Fig. 1a: managed cables on my desk.

[![](https://www.benkuhn.net/wireless/under_hu63e9800c185aac6c3ebf266a25d00a9b_5406720_600x400_resize_q75_box.jpg)](https://www.benkuhn.net/wireless/under.jpg "full size")

Fig. 1b: less-managed cables underneath.

Recently, I finally ragequit and replaced all my desk’s wireless devices with wired ones. While I had to invest a bit in figuring out cable management (and break my habit of chewing on headphone cables), I was able to achieve nearly the same level of tidiness, with _much_ better reliability, quality and speed. I no longer have to worry about my equipment failing to pair, running out of battery, or spontaneously giving me garbled robot voice during a livestreamed talk. It’s dramatically reduced my level of device-related agony.

To illustrate the degree of agony I’m talking about, below I’ll cover some of the subtle, hard-to-notice but severe problems I’ve run into with wireless protocols. If you’re convinced, try out some wires—you, too, can figure out whether you’ve been a victim of the wireless trap.

## Wifi

**Interference.** If multiple wireless networks are operating on the same “channel” (radio frequency band), their transmissions can interfere with each other. When that happens your device needs to re-send the same information, which makes your wifi slow.

You might think this could be solved by having routers automatically figure out the least interference-prone channel to use, but many of them seem to be quite bad at this. (Also, the old 2.4GHz wifi protocol was only allowed to use three non-overlapping channels.)

That means in dense areas (e.g. apartment buildings), routers will often pick a bad channel and end up interfering with each other. There’s no way for your router or device to notify you if it’s experiencing interference, so you’ll only learn about it if you know how fast your router “should” be and notice that it’s slower.

**Dead zones.** If you’re too far from your router, your computer may not be able to reliably receive the signal that the router is sending, or vice-versa. How far is “too far” can also be affected, sometimes in weird ways, by whatever walls or ceilings are in the way. Unless you know a lot about how radio waves interact with building materials, it’s hard to predict where your dead zones are.

The worst part is that many dead zones aren’t fully dead: your computer and the router will try to retransmit each data packet multiple times before giving up, and often it will eventually go through. If that’s what mostly happens, instead of a dead zone you’ll end up with a “slow zone” where your internet works, but is subtly crappy.

Of course, unless you’re keeping a close eye on your network performance statistics and how they relate to your spatial location, you’d never notice a slow zone. If you noticed anything, it would be that sometimes your internet is randomly worse than other times.

**Polling.** Any program on your computer can ask your wireless card to enumerate the nearby networks. This causes it to go into “polling mode,” where it spends less time transmitting data and more time listening for routers advertising their network info (it can’t transmit and receive at the same time). Thus, it will cause a sudden burst of network delays that can e.g. cause your video call to stutter or freeze for a few seconds.

Most programmers don’t realize that wireless polling interferes with network performance, so they ask the OS to poll with wild abandon. I’ve been burned by this many times.

The most egregious instance was when I noticed that my video calls sometimes stuttered with an oddly regular frequency. Here’s the tortuous process by which I tracked down the culprit:

-   I [pinged](https://en.wikipedia.org/wiki/Ping_%28networking_utility%29) my router every second for about 10 minutes, then plotted the output in Excel and confirmed that the slow pings were exactly 30 seconds apart. That made me guess it was probably a software problem.

-   I asked for help debugging it on Facebook and someone recommended [enabling macOS wireless debug logging](https://twitter.com/benskuhn/status/952245967931215872).
    
-   I enabled the debug logging and noticed that several apps, when I had them open, would requesting network scans, at times correlated to the increased ping latencies.
    

-   After narrowing it down to those few apps, I [asked how to stop them from doing that on AskDifferent](https://apple.stackexchange.com/questions/310218/stop-destroying-network-performance-every-time-an-app-scans-for-wifi-networks).
    
-   Someone on AskDifferent tracked the problem down to [Qt](https://www.qt.io/), a software framework for making user interfaces, used by apps with millions of users.
    

Qt included a component which would poll for networks every 30 seconds whenever a “network access manager” was instantiated, causing pretty much _any Qt app_ using the network to degrade your wifi for ~5 out of every 30 seconds.

There were already multiple bug reports for this issue, [one of which](https://bugreports.qt.io/browse/QTBUG-46015?focusedCommentId=290240&page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel#comment-290240) was declared “closed” by an engineer because they allowed users to use an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) to disable the polling.

Of course, this is an unbelievably useless “solution” because most users won’t realize that their wifi is degraded; those who do won’t realize that it’s Qt’s fault; and those who realize will still have a hard time Googling for the appropriate fix (let alone implementing it, unless they can code).

This behavior is so user-unfriendly, and the “fix” so laughable, that it seems likely that the Qt developers somehow failed to realize the severity of the problem—I’d guess it ruined video calls for on the order of a million people, since, for instance, it affected qBittorrent which has been downloaded [75m times](https://en.wikipedia.org/wiki/QBittorrent). Most of those million people were probably not technical enough to figure out how to “set the `QT_BEARER_POLL_TIMEOUT` environment variable to `-1`.”

(Fortunately, it does look like in 2017—three years after the original bug report—they finally realized they should just _stop polling_ and [fixed the bug the right way](https://codereview.qt-project.org/c/qt/qtbase/+/214029).)

Qt was the worst offender, but it’s far from the only one. Even macOS had a bug for a while where the same thing would happen when you opened Spotlight (which I do frequently during video calls, if someone asked me to look at a particular file, or if I want to ~zone out and read the internet~ multitask). I had to fix it by disabling individual Spotlight result types until I found out which one was causing the problem. So it seems even Apple’s own developers don’t realize that wifi polling is a hazard.

## Bluetooth audio

**High latency.** Most Bluetooth headsets introduce [around 150-300ms of latency](https://www.rtings.com/headphones/tests/connectivity/bluetooth#comparison_4115) (the time between my computer receiving the audio from the Internet, and the sound coming out of the headphones). That means that if I’m chatting with a friend in New York, the audio data will take about 50ms to get from them to my computer, and, say, 200ms—_4x as long_—to get from my computer to my ears. Since high latency ruins the natural flow of conversations, I’d like to eliminate as much of it as I can.

It’s possible to find lower-latency Bluetooth headsets if they support the right “codec,” like “AptX Low Latency.” Of course, in addition to supporting the codec in theory, they have to agree with your computer to use it, which can sometimes fail. (The option to inspect which codec is being used is, of course, buried in various hidden menus and settings depending on your OS.)

**Low quality.** Related to the codec issue, many bluetooth devices will play high-quality audio when the microphone is turned off, but degrade to much lower-quality audio when it’s turned on. You can test this for yourself if you have a bluetooth headset: play music on it, then open your microphone settings to the page where it shows the mic input volume. You’ll probably hear the audio cut out for a second, then return at lower quality. (This happens even with devices you might expect to be high-end, like my Airpods Pro + 2018 Macbook Air.)

## Bluetooth general

**Bluetooth bugs.** Even though all computers now have built-in Bluetooth, many Bluetooth accessories also ship with proprietary [dongles](https://en.wikipedia.org/wiki/Dongle) that use a different protocol. (For instance, Jabra headsets, Logitech keyboards/mice, [this headset mic](https://antlionaudio.com/products/modmic-wireless?variant=12529444126775).) These dongles apparently use different forms of wireless transmission because Bluetooth drains lots of power, adds latency, and is less reliable.

For instance, on my Mac’s built-in bluetooth, my mouse (a Logitech MX Master) displayed noticeable jank—stopping, then jumping, instead of moving smoothly. I’ve seen this happen on three different Macs, so it seems likely to be a software problem. When I switched to using Logitech’s dongle it stopped.

Similarly, when connected to Mac bluetooth, my Jabra Evolve 75 headset would frequently have the mic or sound drop. It (mostly) worked fine on its own dongle.

I’m not sure whether to blame Jabra/Logitech or Apple (or the Bluetooth standards body) for these problems. But either way, when device manufacturers are abandoning it for other protocols that require a dongle-per-device, we know Bluetooth has failed.

The original goal of Bluetooth was to unify different wireless devices in a single wireless radio and protocol, [much like Harald Bluetooth unified Denmark](https://en.wikipedia.org/wiki/Bluetooth#Name_and_logo). Meanwhile, here in reality, I recently had to buy a USB hub just for the dongles for my keyboard, mouse, headphones and microphone.

**Reliability.** Even with proprietary dongles, wireless devices are much less reliable than wired. For instance, I wrote above that my Logitech MX Master worked fine once I switched to the proprietary dongle, but that’s not quite true: it worked fine for a while, then one day it started janking again for no discernible reason. (That was the day I finally apostasized and threw out my desk’s wireless gear.)

I encounter other Bluetooth bugs that require me to un-pair and re-pair a few times a week. For instance, my AirPods sometimes “desynchronize” so that one is playing back audio a few milliseconds ahead of the other, causing a strange and really unpleasant echo effect.

**Interference.** One possible reason for poor reliability is that Bluetooth and 2.4GHz wifi interfere with each other. Much like wifi interference, your devices will never warn you if they’re experiencing interference; you’ll notice only because they suddenly become kind of crappy.

**Charging.** Not the fault of Bluetooth per se, but a downside of using too many wireless devices is that it’s really annoying to remember to keep them all charged. Mine tended to die at the least opportune times, e.g. during video calls.

## Conclusion

Most of these problems shared a few things in common:

-   Things didn’t break completely, they just degraded. That’s probably the right call, but it meant that I didn’t immediately notice there was a problem.
    
-   Compounding this, I had no idea how well the device “should” work, so I took a long time to notice that it was in a degraded state.
    
-   Even once I was aware of the problem, it was hard or impossible to understand the root cause and fix it because I didn’t know the right diagnostics (or they didn’t exist).
    

I want my tools to be _predictable_—to have consistent performance and fail in ways that I understand. Wireless protocols are inherently more complex (because many devices share the same airspace) and have more different ways to fail, so they’re much less predictable than wires. For me, the convenience often isn’t worth that cost.

I still use wireless gear when it’s clearly worth it—for instance, I use wifi for my laptop since it moves around a lot, and I use a wireless charger for my phone since I don’t really care how fast it charges. But for serious work, I’ll spend the time to fiddle around with cable routing and wire everything.

This makes me wonder what the world would look like if we took 10% of the effort we currently spend on removing wires from everything, and put it into ingenious cable routing solutions instead. I’d bet that a lot of wireless-dependent activities like video calls would be way more pleasant.

## Appendix: cable management

For me, one of the biggest obstacles to [running more wires](https://www.benkuhn.net/wireless/) was aesthetic: they caused a lot more clutter. But I eventually worked out some good strategies to hide my wires. I’m at the point where only a few are visible now!

Here’s how I got there:

-   The biggest collection of cables is the one feeding into my desktop computer. To manage those, I got a USB hub that I attached to my monitor’s stand with rubber bands. I used zip ties to keep all the cables in a single bundle, and coiled excess length in a loop behind my monitor.
    
-   To run an Ethernet cable to my desktop I ran it around the wall of my room, using [cable hooks](https://amzn.to/3cME3Ow) to keep it right at the edge. (The fancy way to do it is to run the cable through the wall/floor/ceiling but that’s more work than I was up for.)
    
-   My desk top has “cable management grommets” (round holes drilled in the back corners that you can run cables through). I put a wireless charger for my phone in one, and some charging cables (microUSB and Lightning) in the other.
    
-   I mounted a [power strip](https://amzn.to/3cHCEZs) on the underside of my desk. The normal thing would be to leave the strip on the floor, but then the cables running to it are much more visible, and you’d need to leave slack to accomodate raising/lowering the desk. This strip powers my computer, my computer’s USB hub, the wireless charger, the charging cables, and my desk’s motor.
    
-   To route the power strip’s cable cleanly, I attached a [cable raceway](https://amzn.to/2Mz9YqX) to the back of one leg. This doesn’t totally solve the routing problem because it’s a sit/stand desk, so I could only attach the raceway to the part of the leg that doesn’t expand. When it’s in the “sit” configuration there’s an annoying loop of cable dangling from the leg. It’s much better than letting it dangle though.
    

I’m sure there are other better things to do that I’m missing—please comment if you have ideas!

_Correction: a previous version of this post mistakenly described Bluetooth devices’ proprietary dongles as Bluetooth dongles. Thanks to helpful [Hacker News commenters](https://news.ycombinator.com/item?id=23592695) for the correction, which even more strongly supports my point that Bluetooth sucks :)_