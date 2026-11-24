A lot of people have recently started doing most of their work via video calls instead of in-person meetings. Unfortunately, because humans are terrible at making technology that just works, there are many ways to ruin your video calls unnecessarily. The most frequent culprit is a bad internet connection, so in this post I’ll go over how to troubleshoot and fix that (in the ways that matter most for video calls).

There are many network troubleshooting guides out there, but most of them regurgitate a random sequence of “try this” without explaining why, or even showing you how to see if it worked. In this guide I’ll aim higher: I’d like to not just give you troubleshooting steps, but explain what you’re doing and why you’re doing it.

_Epistemic status: [the best way to get the right answer on the internet is to post the wrong answer.](https://meta.wikimedia.org/wiki/Cunningham%27s_Law) I’m not a professional network technician, just someone who’s used a lot of bad networks and occasionally fixed them. [Suggestions or corrections welcome](https://www.benkuhn.net/contact/)!_

Contents

-   [Concepts](https://www.benkuhn.net/vcnet/#concepts)
-   [Ping your router](https://www.benkuhn.net/vcnet/#ping-your-router)
-   [Optimize the connection to your router](https://www.benkuhn.net/vcnet/#optimize-the-connection-to-your-router)
    -   [Dead zones](https://www.benkuhn.net/vcnet/#dead-zones)
    -   [Interference](https://www.benkuhn.net/vcnet/#interference)
    -   [Other problems](https://www.benkuhn.net/vcnet/#other-problems)
-   [Ping the Internet](https://www.benkuhn.net/vcnet/#ping-the-internet)
-   [Test bandwidth](https://www.benkuhn.net/vcnet/#test-bandwidth)
-   [Test end-to-end](https://www.benkuhn.net/vcnet/#test-end-to-end)
-   [Enjoy!](https://www.benkuhn.net/vcnet/#enjoy)
-   [Appendix: further reading](https://www.benkuhn.net/vcnet/#appendix-further-reading)

* * *

## Concepts

When you video call a friend, the data (audio and video) are sent from you to your friend over many different network links between different devices:

![](https://www.benkuhn.net/vcnet/links.png)

Any one of these links can have a problem, although you will mainly care about the ones on your side (the top half of the diagram) because they’re the ones under your control. If you’ve solved your own network problems but the person you’re talking to hasn’t, just send them a link to this post :)

When analyzing the quality of a network link, you care about three things:

-   **Bandwidth**, the rate at which data can be sent to the person you’re talking to. Bandwidth is divided into _upload_ (sending data from your computer to the Internet) and _download_ (the other direction).
    
    Despite marketing and much of the Internet’s advice, bandwidth is probably _not_ the thing making your video calls suck, unless you’re on a bad phone connection or someone else on your network is streaming video or something. Most internet plans that aren’t dialup or low-end DSL will support this.
    
    For instance, Zoom [needs only 600 kbps](https://support.zoom.us/hc/en-us/articles/201362023-System-Requirements-for-PC-Mac-and-Linux) of upload and download bandwidth for “high quality” 1:1 video, and tops out at 1.8 Mbps (megabits per second). Streaming a standard definition video already requires 3 Mbps.
    
    It’s important that your bandwidth is enough to transmit video and audio at the quality level you want (and to accommodate any other simultaneous users of your network), but beyond that, extra bandwidth won’t help.
    
-   **Latency**, the time it takes to send one “packet” of data across the link. High latency manifests as lag, where it takes your friend a while to start talking after you stop, because the audio took a while to come out of their speaker. With high enough latency, it can be really hard to have a fluent conversation, because every single silence feels awkwardly long and triggers your conversational instinct to fill it with chatter.
    
    Even if it’s not noticeable, though, latency is one of the things that contributes to video calls feeling “off,” because everyone’s reactions to what you’re saying are delayed by a few beats.
    
-   **Jitter**, or fluctuations in the latency. For calls, it’s important for latency to be low not just on average, but consistently, because audio/video lag is determined by the latency of the slowest packet in the stream. If your video call software observes that every tenth packet takes an extra 500 milliseconds to arrive, it will leave a 500 millisecond “buffer” between when it receives a normal packet and when it displays the data, so that when the 10th packet is delayed, it won’t need to stop the stream. That 500 millisecond buffer will show up as lag.
    
    Sometimes packets of data can be not just delayed, but lost entirely. This is called **packet loss**, but from the point of view of video calls, it usually has a similar effect to extreme jitter, in that it will cause the video/audio to cut out for a little while, and possibly to skip over some audio when it resumes. This is what causes the dreaded “sorry, you cut out for a minute—can you repeat that?”
    
    Extreme jitter or packet loss are most often caused by a network link failing. More moderate jitter can be caused by fluctuations in _queueing time_, when an intermediate node in the network waits for a while before forwarding your packet because it’s received a temporary surge of traffic from elsewhere.
    

At a high level, your goal is to make sure the latency and jitter between you and your friend are as low as possible, and that the bandwidth between you is large enough to carry your video and audio.

The first step is to understand which part(s) of your network are the problem.

## Ping your router

To test local network performance, you’ll send a small bit of network traffic from your computer to your router and see how it performs.

First, find your router’s **IP address** by following [these steps](https://www.howtogeek.com/233952/how-to-find-your-routers-ip-address-on-any-computer-smartphone-or-tablet/). The IP address will be a series of numbers separated by periods, like `123.45.67.89`. Once you have it, plug it into [this “blip” tool](https://www.benkuhn.net/blipper/) which repeatedly makes small requests to your router and plots how long it takes to get a response. My wired connection shows pretty consistent sub-5ms blips, and my wireless connection is pretty consistently under 10ms but with a bit more jitter.

## Optimize the connection to your router

If your blips don’t look good, it’s time to try improving things!

The easiest and most likely-to-succeed fix, if practical, is to connect to your router via an Ethernet cable. You can buy really long cables, and stick them to your wall using wire clips like [these](https://amzn.to/33VJW9h) to keep them out of the way.

If you’d rather not run a cable, you now get to go down the “wifi performance debugging” rabbit hole. Here are a few potential causes of high latency:

### Dead zones

Your computer and router might not be able to reliably hear each others’ wireless transmissions, because they’re too far away or have too many walls between them. In that case, you may see lots of jitter as your computer needs to retransmit its packets frequently. If your latency and jitter improve when you move your computer next to your router, this is probably your problem. You can walk around while running the blip tool to map out your dead zones.

You can work around this by buying various types of wireless “repeaters” or “extenders,” although all of them will add latency compared to running a cable to your device. The minimum-latency wireless option probably involves running a cable from your router to a convenient nearby point and installing a wired range extender there.

### Interference

If there are other nearby wireless networks using a nearby part of the wireless spectrum, they may interfere with your network, again causing jitter and retransmissions.

The wifi spectrum is divided into several numbered _bands_ of frequencies that your router and computer use to talk to each other. If there’s another router nearby using the same band, they will interfere with each other and become slow. Unfortunately, the 2.4GHz part of the spectrum, which old devices and routers use, is very narrow: there are only three non-overlapping bands.

Interference is more likely if you’re on a 2.4GHz network than on 5GHz—where there is more spectrum space to spread different networks out in—but it can happen on 5GHz too if there are a lot of networks nearby.

To check whether interference might be a problem, use a _network channel scanner_ app ([Mac](https://www.imore.com/how-use-macos-wireless-diagnostics-get-best-possible-wifi-settings-your-area), [Windows](https://www.howtogeek.com/197268/how-to-find-the-best-wifi-channel-for-your-router-on-any-operating-system/)) to show you how much interference there is on each channel. You should compare your network’s _RSSI_ (signal strength) to the strength of the next-strongest network on that channel; if they’re too close (or the other network’s is higher) then they might be interfering. If so, switch the channel to a clearer one; your router’s manual will tell you how.

### Other problems

Dead zones and interference are the two most common wifi issues, but there are huge numbers of less frequent problems you might run into.

-   **Using the wrong band**—most routers today broadcast on two different bands of the wireless spectrum: 5GHz and 2.4GHz. 5GHz is faster and less prone to interference, but doesn’t penetrate walls as well. You’ll usually get much better performance on 5GHz as long as you’re in-range.
    
    Some routers will broadcast 2.4GHz and 5GHz as different networks (named e.g. “Foo Network” and “Foo Network 5GHz” or something similar), in which case you can ensure you’re on the right band by connecting to the 5GHz network. But others will have a single network and use something called [band steering](https://www.draytek.com/support/knowledge-base/5694) to automatically switch clients between 2.4GHz and 5GHz bands. If your router does this, then walking into a 5GHz “dead zone” might end up with your computer switching to less-reliable 2.4GHz connection, and then staying on the 2.4GHz band even once you move out of the dead zone.
    
    If you notice this happening, consider splitting the 2.4GHz band off into its own network to prevent this type of surprise performance degradation (and adding more 5GHz access points to ensure high-performance network coverage in the current dead zones).
    
-   **Computer software issues**—misbehaving networking software can sometimes interfere with ping times. Notably, many apps built with the Qt graphical interface toolkit contain [a horrible bug](https://bugreports.qt.io/browse/QTBUG-46015) that will cause periodic ping latency spikes every 10 seconds as Qt asks your computer to scan for new nearby wifi networks over and over. On OS X, clicking the wifi icon will also spike your wifi latency for a similar reason.
    
    To (mostly) rule out misbehaving software, you can do the ping test after a fresh reboot without starting any other applications.
    
-   **Router hardware or software problem**—the router may be degraded in some way that makes it less effective at processing your packets. You can fix this by getting a new router, although router quality matters a lot for this, so make sure you get a good one.
    
    I haven’t needed to replace my own router yet so only have second-hand recommendations, but here are some:
    
    -   The Wirecutter [recommends](https://amzn.to/2UKdq72) the TP-Link [Archer A7 ($70)](https://amzn.to/3bO1cja) for small spaces without too many devices, and the [Archer A20 ($200)](https://amzn.to/2UKdq72) for larger spaces or more devices.
    
    -   If you want to put more effort into optimizing your network, a coworker recommended a Ubiquiti [EdgeRouter X](https://amzn.to/3bTjL5C) wired-only router + [AP AC Lite](https://amzn.to/2RaZ514) access point (or multiple access points for better coverage). Ubiquiti’s software apparently provides much more powerful administration and troubleshooting tools, like live traffic monitoring, flexible quality-of-service / traffic shaping, and various other metrics. If you don’t mind tinkering, this will probably make it easier to troubleshoot bandwidth issues etc. (Note: if you have a very high-bandwidth uplink, like 500+ Mbps, you may need faster equipment like the [EdgeRouter 4](https://amzn.to/3dUXR3V) + [AP AC NanoHD](https://amzn.to/2V4ChkU) to saturate it.)

## Ping the Internet

Once the connection between your computer and router is solid, the next step is to test your router’s “uplink” to your ISP and rest of the Internet. Your uplink is more likely to be reliable than your wifi, since it’s a more controlled environment with fewer moving parts. But it’s still worth testing.

To test, head back to [the blip tool](https://www.benkuhn.net/blipper/) and watch the blue blips. This is your browser making small requests to a page on Google’s “content delivery network,” whose closest server is probably really close to your ISP. That means the latency you observe here is mostly the latency between your house and your ISP (plus your wifi latency).

The table below typical round-trip latency ranges for various types of connections in the US, as [measured by the FCC](https://www.fcc.gov/reports-research/reports/measuring-broadband-america/measuring-fixed-broadband-eighth-report#_Toc427484593) (fixed) and [High Performance Browser Networking](https://hpbn.co/mobile-networks/) (mobile).[1](https://www.benkuhn.net/vcnet/#fn:1) The distance between blue and green blips on the blip tool should fall within the range listed in the “latency” column.

| type | latency | jitter |
| --- | --- | --- |
| Fiber | 12-20 ms | low |
| Cable | 15-34 ms | medium |
| DSL | 25-80 ms | medium |
| 4G | “<100 ms” | low |
| 3G | 100-500 ms | high |
| 2G | 300-1000 ms | very high |

Note that while this table gives general numbers, specific latency/jitter can vary a lot. For instance, cable wiring is shared between many neighbors and used for TV as well, so you may notice worse latency or bandwidth during peak times when your neighbors are using it too. On the other hand, DSL signal apparently gets worse when you’re far away from your ISP, so you may see better or worse numbers depending on your house’s location.

If your uplink is wired (DSL, cable or fiber) and showing unusually high latency or inconsistent blips, there are two potential culprits:

-   If you have a modem, it could be a problem with your modem—the device responsible for converting the packets from your router into something that can be sent along the DSL or cable wire. Modem problems seem relatively common; for instance, a few years ago Intel sold millions of units of a modem chipset (“Puma”) with awful latency spikes. (To check if your modem was affected by that particular problem, check [badmodems.com](https://badmodems.com/Forum/app.php/badmodems).)
    
    Unfortunately, if you’re not looking for a specific problem like that one, it’s kind of hard to test modem latency in isolation. I don’t have a modem (I have a fiber connection) so I haven’t played around with this much. If you have better ideas, [let me know](https://www.benkuhn.net/contact/) and I’ll update the post!
    
-   If it’s not your modem, it’s probably your ISP’s fault—complain to them or switch to a better one.
    

On a wireless uplink (e.g. mobile hotspot), signal strength also plays a role, but I’ve never really figured out if there’s a consistent way to improve a bad cellular signal, other than buying a dedicated cellular modem with higher transmit power. (Let me know if you know of good resources on this!)

## Test bandwidth

Bandwidth is usually limited by how much data your ISP will allow you to move. To test bandwidth, you can [run a speed test](https://www.google.com/search?q=speed+test). Note that some speed test sites only test download speed, but for video calls, upload speed is equally important.

If your bandwidth is substantially below what your ISP told you you’d be getting, it could be one of a few problems:

-   Your ISP may not be allowing you to use the full bandwidth that they promised you, perhaps because their network is congested by other subscribers or perhaps because they are jerks or liars. This is depressingly common and your only real options are to complain to your ISP or switch.
    
-   Your ISP may be allowing you the full bandwidth, but some of it is being used by something other than the speed test. In this case, it’s useful to know what else is using the bandwidth, but unfortunately, most routers don’t provide good visibility into how much traffic is being caused by different devices; you’ll probably need to buy a more advanced router, like the Ubiquiti recommendation above (or to install [custom firmware](https://dd-wrt.com/) on your current one) to access this.
    
    If the problem is other devices on your own network, the best way to fix it is to use “quality of service” settings to configure your router to prioritize your video call traffic at the expense of less latency-sensitive traffic. It’s a bit hard to test whether that worked, though, because you need to use an actual live video call to test, and most video call software doesn’t expose good enough diagnostics to take objective measurements of quality.
    
-   Something might be limiting the bandwidth between your computer and your router. You can determine whether this is the problem by re-running the speed test on a wired connection to your router. But if you’ve already solved your wifi _latency_ problems with the above steps, then your wifi link is not likely to cause real bandwidth problems for video calls: even the slowest wifi standards will top out at 11 Mbps when working properly, which is much more than is needed for video calls.
    

## Test end-to-end

Finally, you should test the results of your optimizations by actually making some video calls! I’ve saved this until last because I don’t know of an easy way to test this without requiring another person to be on the line. Even if you try to do a test call between two devices on your local network, at least some services (like Zoom) will route the traffic directly between the two machines without using the public Internet, giving you artificially high quality and low latency. So I’d suggest calling a friend who’s on a different network.

The easiest service to test calls with is Zoom, because it offers a statistics panel that displays actual measurements of your upload and download latency/jitter. You can find it in the “Statistics” section of the settings (they’re real-time so you need to be on a call to see anything useful):

[![](https://www.benkuhn.net/vcnet/stats_hu943200cb84b35aa1c0db6705fa1644b4_317575_1128x940_resize_box_3.png)](https://www.benkuhn.net/vcnet/stats.png "full size")

This is a call on my local network, so the video latency is lower than normal.

Other video call software won’t give you such nice diagnostics, but will probably work similarly to Zoom.

(It won’t be exactly the same, since it might use different protocols which can do a better or worse job compensating for bandwidth, latency or jitter issues, or might involve connecting to different servers with notably different uplink latency—for instance, while in Senegal I’ve sometimes observed multi-second latencies to Zoom servers while Skype worked fine, or vice-versa.)

## Enjoy!

Hopefully you’ve now gotten rid of your most annoying video call problem! In a later post I’ll go over camera/microphone/speaker quality, which can also make or break your calls. Stay tuned!

## Appendix: further reading

[High Performance Browser Networking](https://hpbn.co/) goes into more detail on both physical networking, and the protocols used by video calls (and the rest of the internet). Chapters 1 and 5-7 are the most relevant.

[Homenet Howto](https://www.homenethowto.com/) goes into way more detail about how home networking works. If you’re curious about the reasoning for any of the advice here, check it out.

[An Introduction to Computer Networks](http://intronetworks.cs.luc.edu/current/html/) is the textbook I learned networking from. Not as relevant to video calls specifically, but read it and you’ll probably feel comfortable reading most things written about networking.

The [Home Networking subreddit](https://www.reddit.com/r/HomeNetworking/) has various useful guides and advice, and people there will answer questions.

_Thanks to Eve Bigaj and [Lincoln Quirk](https://www.lincolnquirk.com/) for commenting on a draft of this post and to Howie Lempel for catching a typo after publication. Thanks to [Avery Pennarun](https://apenwarr.ca/log/) for making the [original blip tool](https://github.com/apenwarr/blip) which I’ve modified slightly._

* * *

1.  FCC numbers are roundtrip times to their “nearest measurement server;” HPBN didn’t explain what latency, exactly, it was measuring, but I expect it’s somewhat similar. [↩︎](https://www.benkuhn.net/vcnet/#fnref:1)