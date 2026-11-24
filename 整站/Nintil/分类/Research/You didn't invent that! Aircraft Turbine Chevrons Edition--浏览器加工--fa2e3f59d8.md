I recently saw a tweet

https://twitter.com/NASAAero/status/681540119615135745

[NASA-developed chevrons](http://www.nasa.gov/topics/aeronautics/features/bridges_chevron_events_prt.htm) to reduce noise! Cool! But where did those really come from?

This post involves an intro about what aircraft engines are and how they work, and then an exploration of the patents that lead up to this invention, and the story of the organisations that were involved in developing them.

## Aircraft Engines

The thing you see in the picture is something similar to what generates power in many large scale electrical powerplants: A gas turbine. Gas turbines follow what is know as the Brayton Cycle, basically compressing air, injecting fuel into the combustion chamber and igniting it, and then expelling it at the rear. Gas turbines are used in different ways to generate propulsion in aircraft: you have turboprops, propfans, turbofans, and turbojets. A typical turbojet looks like this:

![](https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/31-2-1.svg/500px-Jet_engine.svg.png) Turbojet

Yes: What drives the compressor is the turbine at the rear. But this works only if the turbine is already spinning. This is why [auxiliary power units](https://en.wikipedia.org/wiki/Air-start_system)(usually placed at the aircraft tails) are used to start one turbine, and with that one, start the other. The APUs themselves are also gas turbines, so they have the same problem. To avoid an infinite recursion of gas turbines and postulating a God to solve it, batteries are usually used.

Externally, these look like

![](https://upload.wikimedia.org/wikipedia/commons/6/61/Olympus593.JPG) Rolls-Royce/Snecma Olympus 593 (Concorde Engines)

To finish, turbojets were developed independently by Whittle in the UK and von Ohain in Germany, as I already mentioned [here](https://artir.wordpress.com/2015/08/28/is-war-necessary-for-economic-growth-a-review-and-some-aviation-history-love/).

You have probably noticed that thoe don't look like the usual engines you see in aircraft. That's because almost no one uses turbojets today: now we have turbofans.

## Turbofans

Turbofans are jet engines with a fan at the front. Hence the name. Heh.

![](https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/31-2-2.svg/500px-Turbofan_operation_lbp.svg.png) Turbofan

In turbofans, part of the air goes into the compressor-combustion chamber-turbine section, and other part just goes through the fan.

You now probably wonder why would you want to do that. Isn't it better, for the same size, to have a larger combustion chamber and avoid bypassing air around? If you want maximum speed, yes. These are called low-bypass turbofans, and have only a tiny bit of bypassed air (and look very similar to turbojets)

![](https://nintil.com/old_assets/2015/12/31-2-3.jpg) Eurojet EJ2000 (Eurofighter engine, low-bypass turbofan)

![](https://nintil.com/old_assets/2015/12/31-2-4.jpg) Rolls-Royce Trent 900 (A380 engines, high-bypass turbofan)

Basically, turbojets convert fuel into thrust directly, while turbofans convert fuel into mechanical power (via the turbine at the rear) and that is then converted into thrust by the fan at the front. And this is much more efficient. Turbojets are basicallt throwing out really hot gasses to propel themselves, and that is precious energy. So what you do is to extract as much energy as possible, cooling the exhaust, and then sending it to the fan.

These things are noisy. One of the reasons is due to turbulence. Turbulences wiggle around at certain frequencies, and this generates sound. Since noisiness is a bad thing, people have tought of  some ways to get rid of it. [Chevronning](https://en.wikipedia.org/wiki/Chevron_\(aeronautics\)) (does that word exist?) the rear of the engines is one such a way.

![](https://nintil.com/old_assets/2015/12/31-2-5.jpg) Trailing edge chevrons in a 787 engine (GEnx)

With that explained, let's see where these came from. You could think of two stories:

1.  A NASA committee sat together to survey the field of aircraft engine noisiness, decided to work together on how to reduce it, and after extensive research, theorising, and testing, figured out that chevrons were the way to go. An enormous reduction of noise ensued.
2.  People had been working on this problem for decades, until a NASA committee decides to involve itself and do the things described in the previous story to help advance the field. A marginal reduction of noise happens.

Imagine what's the closest to truth. The first chevrons were installed around 2003 in a GE CF34-8C5 engine mounted in a Bombardier CRJ900 aircraft, but they really gained traction in 2006. Noise trends were as follow. Note how cherrypicking makes the first curve look very nice (It is from a corporate presentation) compared to a chart from a paper.

[![noise1](https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/31-2-6.png?w=630)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/noise1/#main) http://www.comblab.ae.gatech.edu/meyer.pdf

[![noise2](https://nintil.com/old_assets/2015/12/31-2-7.jpg)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/noise2/#main) Dutton, 2002, ch. 2

## The History of the Chevrons

Reducing noisiness in turbine exhaust is not something new. Before gas turbines, reducing the noise of steam turbines was also sought, and thus we see patents like _Device for absorbing the noise of escaping high-presure steam or gas_ (US181288A,1876), where an early apparatus for noise reduction is described, altough the mechanism was quite different from the chevrons that we are discussing here (He used a spring). _Exhaust Nozzles_ (US157548A,1874) is a design that is closer to the chevron philosophy: to try to divide the flow into smaller subflows.[![Shaw74](https://nintil.com/old_assets/2015/12/31-2-8.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/shaw74/#main)

But the field of aircraft engine noise reduction seems to have started with a report from the UK College of Aeronautics (What is today called Cranfield University) in 1952, by two men who later would be involved in many of the projects that came afterwards: [Geoffrey Lilley](http://www.telegraph.co.uk/news/obituaries/11912481/Professor-Geoffrey-Lilley-aeronautical-scientist-obituary.html) and Robert Westley. In that report, different nozzles were tested to see its effects in noise. They look like this:

[![Cranfield](https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/31-2-9.png?w=630)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/cranfield/#main)

You can already see a design similar to the chevrons there in figure c4. These guys moved fast and got a patent, \_Jet noise supression means \_(US3153319, 1952) for some of their research, and the designs there are even closer to the designs that ended up in production half a century later:

[![Cranpat](https://nintil.com/old_assets/2015/12/31-2-10.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/cranpat/#main)

[![Cp2](https://nintil.com/old_assets/2015/12/31-2-11.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/cp2/#main)

They also made designs that follow more closely the idea of splitting the flow into severeal streams. These form the basis of the so called hush-kits, that can be retrofitted to older aircraft to reduce their noise levels:

[![cp3](https://nintil.com/old_assets/2015/12/31-2-12.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/cp3/#main)

This is a [hush-kit](http://www.airspacemag.com/how-things-work/hush-kits-8747402/?no-ist)

![](https://nintil.com/old_assets/2015/12/31-2-13.jpg)

They also patented really crazy stuff:

[![cp4](https://nintil.com/old_assets/2015/12/31-2-14.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/cp4/#main)

The idea there is to use mechanical actuators to insert the vanes into the flow in a controlled fashion, to induce flow mixing, and reduce noise. But imagine actually implementing this into flying aircraft!

According to Morris (2010), Lilley's designs are the ancestors of all noise reduction equipment mounted in contemporary aircraft.

Not much later than that, in 1957, you see similar designs appearing in Rolls-Royce patents like _Jet noise supressor nozzle_ (US30922055A,1963)

[![rollsold](https://nintil.com/old_assets/2015/12/31-2-15.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/rollsold/#main)

In 1961, Vickers Armstrongs Aircraft patents another noise reduction mechanism, based on a design similar to the weird Cranfield designs, but this time, making more sense:

[![vikers](https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/31-2-16.png?w=630)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/vikers/#main)

Those triangles are much less clunkier than the Cranfield design!

## Enter NASA

Some years later, in 1965 NASA starts the US Supersonic Transport Program. As the program advances, subcontractors GE and Boeing note that sound is an issue. This was the source of a newer research program,

> -   NASA initiated the U.S. Supersonic Transport (SST) program in 1965 to develop enabling technologies for future supersonic civil transport concepts and provided support to the Federal Aviation Administration (FAA) to run the program. The FAA subcontracted the Boeing Company to provide the various airframes and General Electric (GE) to provide the engines. As the program progressed, jet noise became an increased concern, and noise goals were set forth for both takeoff and approach flight conditions.
> -   Due to heightened interest in noise reduction and concerns with pollution, the SST program was canceled in 1971, and a new SST Noise Reduction Technology program was initiated. The program primarily focused on testing new ideas for jet noise suppression devices such as high-flow injectors, secondary injectors, and fluid injectors. (Bargsten & Gibson, 2011)

Why did a SuperSonic Transport began there?

[Boeing was working on that since 1952](https://en.wikipedia.org/wiki/Boeing_2707), and was already doing designs and prototypes of supersonic transports. Kennedy saw the aircraft industry wasn't in a very good shape, and to give it something nice to do, he asked the Federal Aviation Authority (FAA) to prepare a report on national aviation goals. FAA suggested focusing on, among other things, SST. Also, the Europeans were working on their Concorde, and so were the Soviets in the Tupolev Tu-144 "Charger" (which actually flew before Concorde). See more in [Flying Magazine](https://books.google.es/books?id=1hZcCuEbSIsC&pg=PA60&dq=Flying+Magazine+1961+SST&hl=es&sa=X&ved=0ahUKEwiLlbK4zIbKAhWEtxoKHWD8Dt0Q6AEIHzAA#v=onepage&q=Flying%20Magazine%201961%20SST&f=false) from 1961.

Interestingly, when the program's funding was cut off, President Nixon was in favour of it, as were the aircraft industry (citation needed :), and labour unions. But the US Senate was against, and they needed their approval. Luckily for them, both the Concorde, and the Tu-144 were costly failures: there wasn't enough demand for them. Of course, things were learnt from building them, but other things were foregone, since the people working on that were not working on other things.

In 1969, the FAA restricts noise around airports, following complaints by people who lived nearby. This induces airlines to ask manufacturers to give them less noisy aircraft. The noise regulations were real business: there are delays and cancellations because of them. (Dutton, 2002, ch. 2)

In 1974, General Electric (GE) and SNECMA partner to design the [CFM56 engine](https://en.wikipedia.org/wiki/CFM_International_CFM56#History). SNECMA wanted to get involved in the civilian market, as it had substantial experience designing military engines, but neeed a partner. Some executives met in the Paris Air Show in 1971 and made a decision. In 1999, CFM, their joint enterprise designs one of the first chevronned engines, the CFM56-B GE had

In 1976, Pratt & Whitney (P&W) and GE initiate the Variable Stream Control Engine program to study the sources of jet noise at various flight conditions (Bargsten&Gibson 2011). This was funded half/half by both NASA and the companies ([Conway 2005](https://books.google.es/books?id=q_yC0yWRks8C&pg=PT171&lpg=PT171&dq=Variable+Stream+Control+Engine&source=bl&ots=7D1Jzejhw1&sig=CNEAMaBvT9_SnUDdv6M2wDK1aMc&hl=es&sa=X&ved=0ahUKEwiCkIbt1IbKAhWGuhoKHZ4DAg4Q6AEIPzAE#v=onepage&q=Variable%20Stream%20Control%20Engine&f=false)).

In 1988, Boeing was contracted by NASA's High Speed Civil Transport (HSCT) program, to design noise-reducing high-speed jet engine technologies.

In 1989, NASA issues a research announcement informing industry that they want to develop some of these noise reducing concepts. (Bargsten&Gibson 2011).

Finally, in February 1992, NASA, FAA and the US Aircraft industry launch the Advanced Subsonic Technology program to reduce noise in engines, nacelles, airframe, and other elements of the aircraft. Chevrons were worked on here, reduction in EPNdB (2.2), and the minutiae for this program come from Golub, Rawls & Russell (2005).

It is important to note that Chevron nozzles were being teste at GE Aircraft Engines in 1996 in internally funded tests, proving the chevron concept to be the most interesting of a series of concepts for noise reduction, so they were submitted to the AST program for further testing.

Inside GE, the idea for chevrons came from previous work on tabbed nozzles nozzles with cutouts, mixer nozzles (See the UTC patent in the next secton), and general knowledge and experience over the years at the company. (Martens 2002)

## Some more patents

In 1981, United Technologies Corp secures patent \_ \_US4487017, \_Exhauster mixer for turbofan aeroengine. \_The point of it was not to reduce noise, but to increase engine efficiency:

[![UTCweird](https://nintil.com/old_assets/2015/12/31-2-17.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/utcweird/#main)

You can almost hear the anger of manufacturing engineers when they saw this. We see once again the idea of diving the flow into subflows.

Two years earlier they got US4284170A, \_Gas turbine noise supressor.\_They introduce what came to be known as tabs, based on the same mechanism as chevrons, but more promitive.

[![tabs](https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/31-2-18.png?w=630)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/tabs/#main)

In Zaman, Bridges & Huff 2012 we can find the story of tabs, and how they evolved into chevrons

So tabs were being actively developed in the 80s, and were small protrusions at the nozzle to supress noise. Tabs were among the concepts tested in the NASA AST program. One of the precursors in this field was Lockheed, that began in 1980, and shortly after got a NASA grant to keep researching.

As late as a 1992 workshop, tabs were still being discussed as viable, and NASA and the companies kept working on them. In 1995, GE, P&W, Allison Engine (an affiliate of Rolls-Royce), and Boeing, under the AST program submitted proposals to test their designs.

That same year, the term chevron is first found, referring to jagged edges in the nacelle, while tabs would now refer to hard breaks and more agressive penetration into the flow.

General Electric and NASA did some CFD work to optimise the designs, and later went on to obtain a patent (which we'll later discuss).

Meanwhile, the folks at Rolls-Royce were also working on this, and in 1981 they had a patent (US4487017A, 1981) for an _Exhaust mixer for turbofan engine_

[![rolls81](https://nintil.com/old_assets/2015/12/31-2-19.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/rolls81/#main)

Manufacturing engineers also hated this, I suppose.

In 1994, a random guy from Bedford, Anthony Errol Harris publishes the first reasonably modern description of chevrons in the literature. His patent, \_Nozzle for turbofan aeroengines \_(GB2289921A, 1994), is referenced by the landmark GE patent I talked about above. I haven't been able to find more about this person, but it seems he wasn't a worker at some UK aeroengine company, he had [his own company](http://www.ukcorporatelist.com/corp/380879.html),

[![bedford](https://nintil.com/old_assets/2015/12/31-2-20.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/bedford/#main)

Finally, we get to the GE 1997 chevron patent,_Chevron exhaust nozzle for a Gas Turbine_ (US6360528B1,1997). They ackowledge that the invention described there was made in performance of work for NASA.

[![chev](https://nintil.com/old_assets/2015/12/31-2-21.png)](https://artir.wordpress.com/2015/12/31/you-didnt-invent-that-aircraft-turbine-chevrons-edition/chev/#main)

In 2000, Boeing and Rolls-Royce agreed to undertake the Quiet Technology Demonstrator (QTD) programme. They also recruited FluiDyne Aerodynamics Laboratory for some of the tests. In the paper where the this paragraph comes from, they say that it would not have been possible without Boeing, Rolls-Royce, NASA, and QinetiQ, plus other organisations. (Bartlett et al. 2004)

According to a Boeing report, Reed et al. 2006, Boeing has been working on noise reduction almost since the end of WWII. In addiction to the first instance of the QTD programme, they describe QTD2, in partnership with GE, Goorich Corporation, NASA, and All Nippon Airlines, enabling them to share the costs of developing this technology. While QTD1 was a purely corporate programme, QTD2 also got NASA funding (Bargsten & Gibson 2001).

Finally, in 2006, GE releases their GEnx engines with chevrons, as does Rolls-Royce with their RR Trent 1000.

In 2013, CFM (GE-SNECMA) releases their LEAP chevronned engine. Here I finish my account.

## Conclusion

NASA played a huge role in pushing forward chevrons, but they did not invent them. It's hard to say who did, after I've presented the evidence here. If we ask instead who built the first ones, that'd be General Electric, so we could say that the story of chevrons begins with a general interest in noise reduction, gets specific after the College of Aeronautics report, develops further with general industry efforts, and finally materialises into a concrete product with a NASA-Industry partnership.

## Timeline

1874: Thomas Shaw patent

1952: College of Aeronautics report on noise

1953: Westley-Lilley Jet noise supression means patent

1957: Rolls-Royce noise supressor nozzle

1961: Vickers patent on noise supression (Insert in exhaust)

1965: US Supersonic Transport Program initiated by NASA SST. As program advances, subcontractors GE and Boeing note that engine noise is an issue

1969: FAA restricts noise around airports

1971: SST program cancelled, replaced by a SST Noise Reducion Technology program

1974: GE-Snecma partnership to design CFM56

1976: Pratt and Witney and GE initiate the Varaible Stream Control Engine to study sources of jet noise

1979: United Technologies tabs patent

1979: UTC Lobed mixer/inverter patent

1980: Lockheed is working on tabs

1982: Rolls-Royce exhaust mixer patent

1988: Boeing under contract for NASA develop new engine technologies for noise reduction

1989: NASA issued a research announcement informing industry of the Agency’s intention to further develop some of these new concepts

1992: NASA and FAA launch the Advanced Subsonic AST program focused on advances in noise reduction. Chevrons developed there reduce noise in 2.2dB.

1994: Anthony Errol Harris patent for a chevronned engine

1995: Several companies interested in tab-like deviced. GE, PW submit proposals in response to NASA solicitation under the AST program. Allison Engine(Affiliate of RR) and Boeing also participated.

1996: Chevron Nozzles tested at GE internally funded program. Further testing at NASA.

1997: GE Exhaust nozzle patent

1999: GE-Snecma partnership to develop CFM56-B for the A321

2000: Quiet Technology Demonstrator: Rolls-Royce/Boeing

2001: NASA Learjet tests, Honeywell Falcon 20 tests for the AST program

2003: Inner-core chevroned engines from CFM in A321

2005: Boeing, GE, Goodrich, NASA, All Nippon Airlines partner for the QT2 program

2006: GE releases Genx with chevrons

2006: Rolls-Royce releases RR Trent 1000 with chevrons

2013: CFM releases LEAP (GE-Snecma) with cevrons

## Bibliography

Alec, D. Y., Geoffrey, M. L., Westley, R., End, W., & Lane, W. (1952). _U.S. Patent No. 3,153,319_. Washington, DC: U.S. Patent and Trademark Office.

Bargsten, C. J., & Gibson, M. T. (2011). _NASA Innovation in Aeronautics: Select Technologies That Have Shaped Modern Aviation_. National Aeronautics and Space Administration, NASA Headquarters.

Bartlett, P., Humphreys, N., Phillipson, P., Lan, J., Nesbitt, E., & Premo, J. (2004, May). The joint rolls-royce/boeing quiet technology demonstrator programme. In _10th AIAA/CEAS Aeroacoustics Conference_ (Vol. 1, p. 806).

Bertram, H. U., David, M. B., & Michael, S. K. J. (1957). _U.S. Patent No. 3,092,205_. Washington, DC: U.S. Patent and Trademark Office.

Brausch, J. F., Janardan, B. A., Barter IV, J. W., & Hoff, G. E. (2002). _U.S. Patent No. 6,360,528_. Washington, DC: U.S. Patent and Trademark Office.

Conway, E. (2005). _High-Speed Dreams: NASA and the Technopolitics of Supersonic Transportation, 1945–1999_. JHU Press.

Dutton, J. A. (2002). Committee on Aeronautics Research and Technology for Environmental Compatibility Report: for Greener Skies–Reducing Environmental Impacts of Aviation. National Research Council, Washington, DC.

Golub, R. A., Rawls, J. W., & Russell, J. W. (2005). Evaluation of the advanced subsonic technology program noise reduction benefits. _NASA Center for AeroSpace Information_.

Martens, S. (2002). Jet noise reduction technology development at GE aircraft engines. _ICAS Paper_,_842_.

Morris, P. (2010). Geoffrey M. Lilley: a very brief biography. International Journal of Aeroacoustics, 9(4-5), 373-386.

Larson, R. S., & Packman, A. B. (1981). _U.S. Patent No. 4,284,170_. Washington, DC: U.S. Patent and Trademark Office.

Reed, D., Herkes, W., Shivashankara, B., & Airplanes, B. C. (2006). The Boeing quiet technology demonstrator program. In _25th International Congress of the Aeronautical Sciences_.

Rodgers, L. J. (1981). _U.S. Patent No. 4,487,017_. Washington, DC: U.S. Patent and Trademark Office.

Shaw, T. (1874). _U.S. Patent No. 157,548_. Washington, DC: U.S. Patent and Trademark Office.

Shaw, T. (1876). _U.S. Patent No. 181,288_. Washington, DC: U.S. Patent and Trademark Office.

Westley, R., & Lilley, G. M. (1952). An investigation of the noise field from a small jet and methods for its reduction.

Zaman, K. B. M. Q., Bridges, J., & Huff, D. (2012). Evolution from ‘tabs’ to ‘chevron technology’-a review. _noise notes_, _11_(1), 27-48.

_Ricón, José Luis, “You didn't invent that! Aircraft Turbine Chevrons Edition”, Nintil (2015-12-31), available at https://nintil.com/2-you-didnt-invent-that-aircraft-turbine-chevrons-edition/._