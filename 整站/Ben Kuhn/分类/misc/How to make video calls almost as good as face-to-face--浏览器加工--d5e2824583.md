I spend a lot of my day on video calls. Wave is a distributed company, so they’re the main way we communicate. But compared to talking in person, they feel unnatural:

-   Most people have low-quality microphones and webcams that make them look and sound bad.
-   There’s a lag between when you say something and when the other person hears it, making it hard to navigate conversational [turn-taking](https://en.wikipedia.org/wiki/Turn-taking).
-   If you’re using headphones, you can’t hear your own voice very well.
-   Because of [echo cancellation](https://en.wikipedia.org/wiki/Echo_suppression_and_cancellation), you often can’t talk when someone else is also talking, which makes the conversation flow less well.

I started wondering how much nicer video calls would feel if I fixed these problems. So I spent way too much time fiddling with gear and software. This post summarizes what I’ve learned.

Collectively, these recommendations have had a pretty big impact: when talking one-on-one to friends with equally good setups, I’ve been able to go 4+ hours without feeling fatigued.

_Epistemic status: best guess; not a professional; almost certainly contains wrong bits. Tell me which ones by comment or [email](https://www.benkuhn.net/hi/)!_

Contents

-   [omg ben don’t make me read your 4500 word doorstopper, just tell me what to do](https://www.benkuhn.net/vc/#omg-ben-dont-make-me-read-your-4500-word-doorstopper-just-tell-me-what-to-do)
-   [Network](https://www.benkuhn.net/vc/#network)
-   [Audio](https://www.benkuhn.net/vc/#audio)
    -   [Get away from other people](https://www.benkuhn.net/vc/#get-away-from-other-people)
    -   [Get full-duplex audio with no echo](https://www.benkuhn.net/vc/#get-full-duplex-audio-with-no-echo)
    -   [Throw your wireless headset in the trash](https://www.benkuhn.net/vc/#throw-your-wireless-headset-in-the-trash)
    -   [Hear yourself clearly with open-back headphones](https://www.benkuhn.net/vc/#hear-yourself-clearly-with-open-back-headphones)
    -   [Don’t mute](https://www.benkuhn.net/vc/#dont-mute)
    -   [Get a better microphone](https://www.benkuhn.net/vc/#get-a-better-microphone)
    -   [Listen to yourself](https://www.benkuhn.net/vc/#listen-to-yourself)
-   [Video](https://www.benkuhn.net/vc/#video)
    -   [Use a dedicated monitor](https://www.benkuhn.net/vc/#use-a-dedicated-monitor)
    -   [Improve your lighting](https://www.benkuhn.net/vc/#improve-your-lighting)
    -   [Use your real background](https://www.benkuhn.net/vc/#use-your-real-background)
    -   [Don’t bother with webcams](https://www.benkuhn.net/vc/#dont-bother-with-webcams)
    -   [Use your smartphone…](https://www.benkuhn.net/vc/#use-your-smartphone)
    -   […or a real camera](https://www.benkuhn.net/vc/#or-a-real-camera)
    -   [A note on camera buying](https://www.benkuhn.net/vc/#a-note-on-camera-buying)
-   [Conclusion](https://www.benkuhn.net/vc/#conclusion)

* * *

## omg ben don’t make me read your 4500 word doorstopper, just tell me what to do

Here’s how I would stack-rank my advice for my past self. (Of course, your personal ranking might be different depending on your situation.)

1.  ($depends) Don’t work in a space where your noise can bother other people, or vice versa.
    
2.  ($10-30) If you ever have network issues, run [a cable](https://amzn.to/3luTVdV) between your computer and router. You’ll probably need an [adapter](https://amzn.to/2YOFis9). (Contrary to popular belief that a bad connection is your ISP’s fault, it’s [more likely to be flaky wifi](https://www.benkuhn.net/wireless/).)
    
3.  (~$100) Buy [open-back headphones](https://amzn.to/3bULynH), which let you hear your own voice normally and are extremely comfortable.
    
4.  (~$30) Switch from your built-in computer mic to a [headset mic](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) (and [pop filter](https://amzn.to/3hOU5JU)), which will sound much better and pick up less noise. Note this requires a headset with detachable cable, like the one I linked above.
    
    You can now leave yourself unmuted! If the other person also has headphones, you can also talk at the same time. Both of these will make your conversations flow better.
    
5.  ($0) Prefer Zoom to most alternatives; it has higher sound quality, better echo cancellation, and fewer silly behaviors. If you have headphones and a good mic, enable “original sound” to turn off some unnecessary audio filtering.
    
6.  (~$200) Get a second monitor for notes so that you can keep Zoom full-screen on your main monitor. It’s easier to stay present if you can always glance at people’s faces. (I use an iPad with [Sidecar](https://support.apple.com/en-us/HT210380) for this; for a dedicated device, the right search term is [“portable monitor”](https://amzn.to/3iZRUVz). Also, if your meetings frequently involve presentations or screensharing, consider getting a third monitor too.)
    
7.  ($0?) Arrange your lighting to cast lots of diffuse light on your face, and move away any lights that shine directly into your camera. Lighting makes a bigger difference to image quality than what hardware you use!
    
8.  (~$20-80 if you have a nice camera) Use your camera as a webcam. There’s software for [Canon](https://www.usa.canon.com/internet/portal/us/home/support/self-help-center/eos-webcam-utility), [Fujifilm](https://fujifilm-x.com/en-us/support/download/software/x-webcam/), [Nikon](https://downloadcenter.nikonimglib.com/en/products/548/Webcam_Utility.html), and [Sony](https://support.d-imaging.sony.co.jp/app/webcam/en/) cameras. (You will want to be able to plug your camera into a power source, which means you’ll probably need a “dummy battery;” that’s what the cost is.)
    
9.  (~$40 if you have a smartphone with a good camera) Use that as a webcam via [Camo](https://reincubate.com/camo/).
    
10.  (~$350) If you don’t own a nice camera but want one, you can get a used entry-level mirrorless camera + lens + dummy battery + boom arm. See [buying tips below](https://www.benkuhn.net/vc/#a-note-on-camera-buying).
     

More detailed recommendations and justifications follow.

## Network

Connection problems are the thing that makes video calls suck the most. They do this in three different ways:

1.  If your connection ever gets really bad, your audio will break up, which is exhausting to listen to and ruins the flow.
    
2.  Even if it doesn’t get that bad, a poor connection will increase _latency_, or the time between when you speak and when the other person hears you. High latency is what causes the dreaded “you first, no _you_ first” dance.
    
3.  Finally (and least importantly), a bad connection limits the amount of data you can exchange, forcing you to use lower-quality video. This doesn’t really matter if you’re using a webcam, but by the end of this post, you might have a good enough camera that it matters.
    

I wrote a whole post of its own on how to troubleshoot your home network for video calls, but realistically, most connection problems are because [wifi sucks](https://www.benkuhn.net/wireless/) and you can avoid them by not using wifi. So, first try running an [Ethernet cable](https://amzn.to/3luTVdV) between your computer and your router. If you still notice high latency or your connection dropping, or if you really can’t run a cable for some reason, [check the guide](https://www.benkuhn.net/vcnet/) for more troubleshooting advice.

## Audio

Video improvements are flashy and noticeable, but audio is the reason you’re having the call, thus ultimately more important. So audio comes first.

### Get away from other people

This is a basic prerequisite for everything below. Coworking spaces and cafés are nice if you plan to be silent all day, but will make natural-feeling meetings impossible due to your crippling self-consciousness about noise levels. If you’re going to be on a call for more than 5 minutes, get your own space.

(If you are committed to taking your meetings in a crowded and noisy space, ignore the rest of the audio section. You’re mostly just doomed to crappy calls in this case, though you might be able to limit the damage by getting [a nice headset mic](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) and installing [krisp.ai](https://krisp.ai/get-krisp-now/?ref=benkuhn).)

If you’re talking to someone else who’s in a noisy environment, you can apparently also use krisp.ai to filter their audio yourself, though I haven’t tried this.

### Get full-duplex audio with no echo

One key ingredient to making voice conversations feel “natural” is that both participants need be able to talk and hear the other person talking at the same time (“full-duplex audio”). Full-duplex audio is important because it allows you to talk simultaneously (“overlap”) with the other person.

You might think that overlap should be rare, because interrupting someone else is rude. While that’s true of large-scale overlaps, we often use small-scale overlaps to [negotiate conversational turn-taking](https://en.wikipedia.org/wiki/Turn-taking#Overlap) (e.g. starting talking when the speaker is trailing off but hasn’t finished), or to signify that we’re paying attention (“uh-huh,” “yeah”).

The hard problem of full-duplex audio is that if someone else is talking, their voice is going to come out of your computer’s speakers and go back into your microphone. If your computer leaves the microphone on, in that case, it’ll end up playing back an “echo” of their own voice to them, which is extremely annoying. So video call tries to filter out feedback from your speakers into your microphone, which is called [echo cancellation](https://en.wikipedia.org/wiki/Echo_suppression_and_cancellation).

Unfortunately, removing _only_ the speaker echo from your microphone stream is really hard to do. So instead, the software often ends up completely muting your mic if someone else is talking. If you’ve ever tried to micro-overlap with someone and noticed that their audio cut out briefly, that’s what’s going on.

If your listeners can’t overlap with you, it’s harder to tell whether they’re following along, and it’s harder to negotiate whose turn it is to speak. This makes the conversation feel less natural, especially in larger groups.

To get full-duplex audio, you need to (a) have an audio setup that doesn’t produce echoes, then (b) convince your video call app not to try to suppress echoes.

(a), “an audio setup that doesn’t produce echoes,” means that your microphone should not pick up any sound from your speakers. In practice this means that your “speakers” must be headphones.

(b), “convince your video call app not to try to suppress echoes,” seemed surprisingly tricky when I tried to research it, because each video call app has its own heuristics for when to engage echo-cancellation.

So I did my own tests of echo cancellation Zoom, Skype, and Hangouts in Chrome and Firefox. I started a chat between two computers, both with headphones attached—a setup that should have required no echo cancellation. I then played music into the microphone of one computer. On the other, I spoke into the microphone and listened for whether the music got quieter.

Zoom, Skype and Hangouts in Firefox all seemed to slightly decrease the audio volume when I spoke, indicating light echo cancellation. For Hangouts in Chrome, the audio cut out _completely_ every time I said anything. In Zoom, I was able to eliminate all echo cancellation by selecting the “use original audio” option, which you can also permanently enable for particular audio devices—I’d recommend doing this.

### Throw your wireless headset in the trash

The gear I recommend in this guide is all wired, not Bluetooth. While Bluetooth seems like it should be great, in practice it has [horrible problems with audio latency, quality and reliability](https://www.benkuhn.net/wireless/). Also, I don’t think wireless open-back headphones (see below) exist.

If you finished the previous paragraph and still think you can get away with using wireless audio gear, read the post at the link :)

### Hear yourself clearly with open-back headphones

Most headphones are _closed-back_, which means they form an acoustic seal over your ear that attenuates outside sound. This is good for “noise isolation” when you’re listening to music. But it’s bad in calls because it also isolates you from your own voice, making you sound muffled and unnatural to yourself. (The same thing also happens with any earbuds that form a seal, i.e. pretty much everything except EarPods or non-Pro AirPods.)

Personally, without the feedback from hearing myself, I also tend to start speaking louder or shouting on calls. This tires out my voice, and can get stressful for whoever I’m ~shouting at~ talking to.

To avoid this, you can buy _open-back headphones_, which have mesh instead of a closed covering over your ears. I bought the [Philips SHP9500](https://amzn.to/3bULynH), which I like a lot; I haven’t tested any other pairs. (I chose a low-end pair because for video calls, sound quality will mostly be limited by people’s microphones; if you want to use the same ones to listen to music, you might want a higher-end pair.)

As an extra bonus, open-back headphones are _way_ more comfortable because they get less hot. I didn’t realize beforehand how much difference this would make, but it’s amazing to be able to wear headphones all day without my ears overheating!

Note that open-back headphones “leak” sound, so anyone near you will hear the other side of the conversation as well. This isn’t a problem if you have your own space, but they’re not suitable for shared spaces. You might think the sound could leak into your own microphone and cause an echo, but I tested and it’s too quiet for that unless you set the volume uncomfortably high.

### Don’t mute

This isn’t about equipment per se, but it has implications for your equipment choices. Quoting [Matt Mullenweg](https://ma.tt/2020/03/dont-mute-get-a-better-headset/), founder of one of the earliest and largest fully-distributed companies:

> One heterodox recommendation I have for audio and video calls when you’re working in a distributed fashion is not to mute, if you can help it. When you’re speaking to a muted room, it’s eerie and unnatural — you feel alone even if you can see other people’s faces. You lose all of those spontaneous reactions that keep a conversation flowing. If you ask someone a question, or they want to jump in, they have to wait to unmute. I also don’t love the “unmute to raise your hand” behavior, as it lends itself to meetings where people are just waiting their turn to speak instead of truly listening.

I strongly agree with this and prefer for the people I’m talking with to stay unmuted unless they have a crappy mic that picks up a lot of noise. Which won’t be your problem as long as you…

### Get a better microphone

Most non-standalone computer microphones, including ones on fancy headsets, sound ear-bleedingly bad. (The 2020 MacBook Pro microphone is okay.) You can sound way more pleasant to your colleagues by getting a nicer one. For instance, compare me reading [Edward Lear](https://www.nonsenselit.org/Lear/BoN/bon010.html) on the following mics:

| Mic | Recording | Commentary |
| --- | --- | --- |
| 2014 iMac | 
 | Sounds like a tin can, because it is |
| Jabra Evolve 70 | 

 | Wirecutter rec; sounds like a bad head cold |
| 2020 MacBook Pro 13" | 

 | “Studio quality” my foot; try “moderate head cold” |
| [V-Moda BoomPro](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) | 

 | Stupid name, $30, actually sounds ok |

The best “can’t mess up” microphone option is the last one in the table, the [V-Moda BoomPro](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) (with a [foam windscreen](https://amzn.to/3hOU5JU)), which attaches to your headphones in place of a standard 3.5mm audio cable.✻✻ This means the BoomPro requires headphones with a detachable cable. If you’re wedded to a different pair, the [Antlion ModMic](https://amzn.to/2Zypzhq) is a more expensive option that works with any headphones, at the cost of a second cable. There’s also a wireless version, although you shouldn’t use wireless audio equipment for video calls due to latency and quality concerns. It sounds much clearer and less muffled than any headset’s built-in mic. It’ll also pick up less background noise (e.g. typing) than _any_ mic, by virtue of being closer to your mouth, which makes it easier to follow “don’t mute” above. However, it won’t sound quite as natural as a non-headset mic.

If you want something that sounds even more realistic, your best bet is something like an [AT2005](https://amzn.to/3kh95SM) positioned less than six inches from your face using a [boom arm](https://amzn.to/3bVaDP5) that’s [clamped](https://amzn.to/3kb57uP) to your desk. If you don’t want your mic to be visible, it may require some zooming/cropping of your camera setup to get it that close. Compare it to the BoomPro (you’ll need headphones to hear the difference well):

| BoomPro | AT2005 |
| --- | --- |
| 
 | 

 |

I tested a few different microphones, and listened to recordings of many more. I haven’t trained myself to detect small quality differences, but my tentative conclusions are:

1.  Distance to your mouth dominates nearly everything. For the mics I tested, 6 inches vs 12 inches made as big of a difference as switching mics.
    
    (That’s because from the microphone’s perspective, doubling the distance makes your voice 4x quieter, or equivalently, makes room noise 4x louder. It also makes louder e.g. the echoes of your voice—leading the microphone to produce a “boomy” sound.)
    
    Here’s two fairly different-sounding microphones at 6 inches vs 12:
    

| Model | 6 inches | 12 inches |
| --- | --- | --- |
| Blue Yeti | 
 | 

 |
| AT2005USB | 

 | 

 |

2.  Low-end condenser microphones sound somewhat “fuller” or more natural than dynamics, which can sound a little “muffled.” For example, the Blue Yeti above is a condenser while the AT2005USB is a dynamic.
    
    (Dynamic mics are sometimes said to “reject noise” better than condensers, but I couldn’t find anyone making this claim who explained why.†† It is easier to put a dynamic microphone right next to your mouth, which is sort of like rejecting noise, but not relevant if you want to place your mic outside the frame of a video. Dynamic mics also have less high-frequency and “transient” response, which can make some types of room noise less obtrusive.)
    
3.  Mics under $50, and headset mics, sound noticeably bad even when close to your mouth. Outside of that, it seemed like sound quality depended as much or more on things other than microphone quality, like how much your space echoes. (There’s a surprisingly large genre of YouTubers demoing expensive mics with bad-sounding setups.) So it seemed like higher-end microphones would probably be wasted in most video call setups.
    

Based on this, I suggest the AT2005 as a widely-recommended mic in the lowest “doesn’t sound noticeably bad” price tier, that can be stuck on a boom arm with minimal ceremony.

Other microphone comparisons for further reading:

-   [Wirecutter USB mic comparisons](https://soundcloud.com/wirecutter-audio/sets/2019-usb-microphone-testing)
    
-   [Marco Arment on podcasting microphones](https://marco.org/podcasting-microphones)
    
-   [Matt Mullenweg “Don’t mute, get a better headset”](https://ma.tt/2020/03/dont-mute-get-a-better-headset/)
    
-   [rtings.com headset recording quality tests](https://www.rtings.com/headphones/tests/microphone/recording-quality)
    

### Listen to yourself

In video calls, unlike real life, what you hear is not the same as what’s heard by the person you’re talking to. And mics are fiddly enough that your particular setup and mic technique matters a lot. So it’s really useful to listen to how your audio sounds. You can do this with a web app like [miccheck.me](https://www.miccheck.me/). The most common mic problems are [plosive pops](https://en.wikipedia.org/wiki/Pop_filter) and [harsh sibilants](https://en.wikipedia.org/wiki/De-essing)—pick sentences that will cover those. The [Harvard Sentences](https://www.cs.columbia.edu/~hgs/audio/harvard.html) are a good starting point.

If those consonants sound bad, you might need a better windscreen, or to change how your mic is positioned. For instance, if you have a headset mic, you should position it just beside the corner of your mouth—_not_ directly in front—so that you’re not breathing/spitting into it.

## Video

### Use a dedicated monitor

I recently started putting my active call in full-screen mode on my primary (27") monitor, and using a second monitor for notes or other activity. This turned out to make a surprisingly big difference to how immersive the call felt. (Maybe I should have been clued in by the fact that this feature is called “immersive mode”?) It’s amazing for keeping me focused and present. Possible reasons why:

-   In windowed mode, Zoom keeps your preview at the top of the screen in a bar of its own, but in fullscreen mode there’s no bar, just a floating preview window (which you can also hide). That means there’s a lot more screen available for other people’s video.
    
-   Hiding the window bezel, task bar, etc. make it much less salient that you’re talking through a computer, and the user interface is less likely to distract you.
    
-   In windowed mode, I’d sometimes end up tabbing away to look at another window, and lazily forget to tab back. Then I’d spend a lot of the call not looking at people and feeling less connected.
    

For the second monitor for notes, I use an iPad with [Sidecar](https://support.apple.com/en-us/HT210380), but you can also buy dedicated “portable monitors” for under $200 that would serve this purpose well.

### Improve your lighting

The best way to get a sharper image on any camera is to put more light into the sensor. Laptop webcams have terrible image quality, but a laptop webcam with good lighting will look better than a fancy camera with bad lighting:

[![](https://www.benkuhn.net/vc/imac%20cam%20bad_hu1afdd5c2d162e08ace9eebe225e08ba7_133549_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20bad.jpg "full size")[![](https://www.benkuhn.net/vc/sony%20bad_hu1afdd5c2d162e08ace9eebe225e08ba7_252223_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/sony%20bad.jpg "full size")[![](https://www.benkuhn.net/vc/imac%20cam%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_156166_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20good.jpg "full size")

Left: 2014 iMac webcam destroyed by MAXIMUM BACKLIGHT. Middle: Fancy Sony A6000 struggles too. Right: iMac webcam gloriously lit by a [lumenator](https://www.benkuhn.net/lux/) bounced off the wall in front of me. Click for fullsize.

The two basic rules of lighting are:

1.  Cast lots of diffuse light on your face to make sure it’s brighter than the background. (Also, the more light that’s hitting the scene, the less grainy your image will appear.)
    
    The easiest way to do this is to put your desk in front of a window; the second-easiest is to bounce artificial lights off of a light-colored surface behind the camera. If neither of those is enough, you can also use a “ring light” or “softbox” (no particular recommendations as I’ve never tried one).
    
2.  Eliminate light sources in the camera’s field of view. Compared to the human eye, cameras have lower _dynamic range_—they can’t capture large differences in brightness. That’s why, for instance, your phone can’t take good pictures of trees against the sky on a bright day. Similarly, if there’s a bright window behind you, or a light fixture in your camera’s field of view, that part will look “blown out” and everything else will look dark by comparison.
    

### Use your real background

Probably controversial. I’m speaking strictly from the point of view of immersiveness here—not e.g. expressing your individuality, making your coworkers laugh, or hiding the pile of laundry behind you. Those are all valid reasons to want to use backgrounds! Just be aware that you’re sacrificing immersiveness when you do.

Why? Zoom’s background detection software is not very accurate, and it’ll periodically delete parts of your hair/body, make the background show through your eyeballs, etc. Plus, it’s really bad at detecting boundaries so some of the real background will show through your hair.

If you have a decent camera (as below), and a space of your own (see [get away from other people](https://www.benkuhn.net/vc/#get-away-from-other-people) above), you’ll look less distracting and more real if you don’t use a fake background.

### Don’t bother with webcams

It’s probably obvious that laptop webcams suck. Even if they’re not [inexplicably positioned so that they look up your nose](https://www.reddit.com/r/Dell/comments/5pkpa7/who_thought_this_is_the_perfect_place_to_put_a/), they will be grainy, blurry and have a tiny dynamic range.

Maybe less obviously, external webcams aren’t that much better. This surprised me, since a high-end webcam like the [Logitech Brio](https://www.logitech.com/en-us/product/brio) costs 2/3 as much as a used interchangeable-lens camera and has _one job_.‡‡ To be clear, the Brio [looks a lot better](https://randsinrepose.com/archives/good-meetings-are-jazz/) than most other webcams—just still a lot worse than a real camera. For instance, I bought a Logitech C920, Wirecutter’s pick for “best webcam,” but it wasn’t obviously better than my six-year-old iMac webcam, mostly due to _really_ questionable exposure / color balance settings.§§ The C920 does allow you to tune these settings, but it took me about 2 hours to figure out how, and I ended up having to buy a crappy third-party app. Manually tuning the settings would also require you to change them whenever your lighting changes throughout the day, which is pretty annoying.

[![](https://www.benkuhn.net/vc/imac%20cam%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_156166_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20good.jpg "full size")[![](https://www.benkuhn.net/vc/c920_hu1afdd5c2d162e08ace9eebe225e08ba7_348796_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/c920.jpg "full size")

Left: 2014 iMac camera; right: Logitech C920. I am pretty white, but I'm not THAT white.

### Use your smartphone…

(I haven’t used a smartphone as a webcam extensively because I jumped straight to using a full camera, so these are weakly held. I’ll update as I learn more.)

I expected webcams to be better than smartphones, because they wouldn’t have much reason to exist otherwise. But it turns out I was wrong: webcams do not, in fact, have much reason to exist. Smartphones beat them on sensor size and quality of materials. For example, the iPhone 11 camera [costs $73.50 in materials](https://www.cultofmac.com/658650/iphone-11-pro-max-production-cost-bill-materials-bom/), and has a [1/2.55" sensor](https://photographylife.com/reviews/iphone-11-pro-camera), while the Logitech C920 _retails_ for $80 and has a [1/3" sensor](https://www.techtravels.org/modifying-logitech-c920/).‖‖ Of course, if you have a lower-end smartphone, the C920 might have nicer materials than your smartphone. But it seems like there’s a very small region of tradeoff-space where “buy a webcam” is a good idea relative to “buy a nicer smartphone” or “buy an entry-level camera." So, if you want an external webcam, use a smartphone.

[![](https://www.benkuhn.net/vc/c920_hu1afdd5c2d162e08ace9eebe225e08ba7_348796_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/c920.jpg "full size")[![](https://www.benkuhn.net/vc/camo%20phone_hu7fba9da1679ce4a56c592454604cb9c1_865840_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/camo%20phone.jpg "full size")

Left: Logitech C920; right: iPhone XR via Reincubate Camo (crop).

I briefly tried two apps for using your smartphone as a webcam, [Camo](https://reincubate.com/camo/) and [EpocCam](https://www.kinoni.com/). EpocCam is cheaper ($8 vs $40) but seemed somewhat buggier than Camo (both had occasional issues). Both of them have free trials that watermark your video, so suitable for testing but not actual calls.

The easiest way to mount a smartphone seems to be via a [gooseneck holder like this](https://amzn.to/3hvwyOf). It seemed like it should be possible to find a much smaller device that attached the phone to my monitor, but the best I could find was [this clip thing](https://amzn.to/2RkLO5E), which obscures part of the screen on laptops, doesn’t adjust in small enough increments to get the right field of view, and doesn’t work on external monitors with curved backs like my iMac. Let me know if you have a better suggestion.

### …or a real camera

Even the lowest-end “real” cameras will trounce most smartphones on image quality. You’ll get a much sharper image, with a pleasingly blurred background to subtly draw attention toward your face and away from your piles of dirty laundry.

[![](https://www.benkuhn.net/vc/camo%20phone_hu7fba9da1679ce4a56c592454604cb9c1_865840_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/camo%20phone.jpg "full size")[![](https://www.benkuhn.net/vc/sony%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_289081_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/sony%20good.jpg "full size")

Left: iPhone XR via Camo; right: Sony A6000. This one makes more sense at full resolution; the Sony has a bit worse dynamic range, but the image is **much** sharper.

This is probably the most noticeable improvement in the list: I started using a camera when I gave a virtual conference talk at [!!con](http://bangbangcon.com/speakers.html#ben-kuhn) and got compliments from ~20 different coworkers and conference speakers. (Being this noticeable is not necessarily an advantage, but I figured my coworkers already mostly knew that I was extremely vain, and so there was no point in trying to hide it.)

Unfortunately, these cameras are also… pretty user-unfriendly and can take a bit of work to set up. Some non-obvious tips if you decide to replicate my setup (Sony A6000 + Elgato CamLink 4k):

-   Get a non-power-zoom lens, otherwise your zoom setting will get reset every time you turn the camera off and on again.
    
-   If you have a Sony A6000, make sure you turn the top dial to “video” mode. Otherwise the HDMI output will be lower quality and the continuous focus won’t work right.
    
-   Use the widest aperture possible to minimize graininess and get a nice blurry background effect.
    

### A note on camera buying

I’m not sure which camera model is best now that so many different manufacturers have webcam drivers. Before the webcam drivers came out, the Sony A6000 was the default recommendation for a camera for streaming, and it’s also generally well-recommended as an entry-level mirrorless camera; but it doesn’t look like it works with Sony’s webcam drivers at all (it’s not in the list of supported models, although the A6100 and later are). Basically, do your own research.

It’s best to buy from a reputable used camera dealer (e.g. [Keh](https://www.keh.com/), [B&H](https://www.bhphotovideo.com/), [Lens Authority](https://www.lensauthority.com/)) since random resellers on Amazon / etc. might not be good about checking that the camera works well.

For the record, here’s what I bought though I don’t particularly endorse it:

-   Sony A6000
-   16-50mm ƒ/3.5–5.6 PZ OSS lens (as mentioned above, I’d recommend the non-PZ 18–55mm ƒ/3.5-5.6 OSS instead)
-   Elgato Camlink 4k (may be out of stock; cheaper options available from no-name vendors)
-   HDMI to Micro HDMI cable (took me a surprisingly long time to figure out what type of HDMI port the A6000 has!)
-   Random off-brand USB dummy battery (sometimes gives “battery depleted” errors so maybe not the best idea)

## Conclusion

The main thing I learned from this adventure is that, much like [wireless](https://www.benkuhn.net/wireless/), video calls still basically don’t work. Every piece of equipment has tricky subtleties that make it super hard to select the right gear and use it correctly. Software does incredibly stupid things that you’ll never notice unless you know what to look for. You never hear your own audio, so if it sucks, you’ll never know. Many of the problems you do notice will be near-impossible to debug because there are no good diagnostics.

If there was a $2000 device that eliminated the need for this post to exist, it would be an automatic purchase for my employer and probably every other distributed company. But there isn’t, so here we are.

_Thanks to [Dan Luu](https://danluu.com/), [Sasha Illarionov](https://sdll.io/), David Coletta, [Alexey Guzey](https://guzey.com/), [Lincoln Quirk](https://www.lincolnquirk.com/), [Eve Bigaj](https://evebigaj.com/) and Jessica Gambirasi for commenting on a draft of this post._