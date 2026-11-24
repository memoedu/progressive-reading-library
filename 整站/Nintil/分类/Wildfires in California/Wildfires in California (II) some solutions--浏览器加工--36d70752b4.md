In the [previous](https://nintil.com/wildfires-california) post I introduced some key aspects of the wildfire situation in California. This followup offers some suggestions for what to do about it.

We can think of firefighting as a multi-stage process that starts with a source of ignition that then progresses all the way into containment of large scale fires and evacuation of entire settlements. Accordingly at each stage there are multiple things one could do. Importantly there is no silver bullet against wildfires, and moreover there is no clever technical way to fight them: the bulk of the response is going to be "more and better of the same".

The presence of wildfires in California is not a problem that given enough resources could be solved within reason (By solved meaning ~0 uncontrolled fires occurring) as the tradeoffs involved make that impractical. Instead of solving it, the solutions below aim to manage fire and reduce its impact on human activity.

Some of the solutions below are somewhat on the speculative side. The [obvious solutions](https://www.youtube.com/watch?v=Dug0tpJ4InA) we should go for are worth stating and require little explanation: more vegetation thinning and prescribed burns (and easing [air quality](https://www.propublica.org/article/they-know-how-to-prevent-megafires-why-wont-anybody-listen) regulations to allow them), more firefighters and firefighting equipment, reducing carbon emissions, enabling CAL FIRE to employ managed wildfire, and better coordination between firefighting agencies.

If you think I should add something to this post, perhaps solutions that would be great that I have not thought about, drop me an email at Jose \[at\] ricon.xyz.

## Early detection

The way a lot of early detection currently works is that someone sees the fire and calls 911 to report it. Then firefighters have to try to find the fire and report it. Can we do better than that?

## Satellites

I wrote in the previous post that we can use satellites to assess how large a fire is after the fire occurs. Could we use satellites to spot fires as they happen and extinguish them before it is too late? This is a hard problem: To do that you would need high spatiotemporal resolution. High spatial resolution to pinpoint very small fires and high time resolution (sampling every few seconds) to be able to react fast enough.

Commonly used satellites that generate data of relevance here are useless for early detection. With the potential exception of the recent HuanJing satellites and (missing from the paper) those launched by [Planet Labs](https://www.planet.com/products/satellite-imagery/files/Planet_Combined_Imagery_Product_Specs_December2017.pdf), there is no satellite that even begins to have sub-100m accuracy (Barmpoutis et al. [2000](https://www.mdpi.com/1424-8220/20/22/6442)). In general satellites that have higher spatial resolution are flying in non-geostationary orbits so they get to pass over one given spot every few minutes. Geostationary satellites are constantly monitoring one area so they could provide real time data but their orbit is high. Planet Labs offers very impressive capabilities but _real_ real time is not one of them (for their highest spatial resolution product, they [sample](https://www.planet.com/50cm/) a given point 12 times per day)

But there is a kind of satellite that has been custom built for precisely this kind of use case (Detecting very small infrared signatures every few seconds): Satellites built to deliver [early warnings](https://spacenews.com/la%E2%80%82pleading-missile-warning-satellites-track-wildfires/) of launches of Intercontinental Ballistic Missiles ([DSP](https://en.wikipedia.org/wiki/Defense_Support_Program#:%7E:text=The%20first%20launch%20of%20a,space%2Dbased%20early%20warning%20capability.) , [SBIRS](https://en.wikipedia.org/wiki/Space-Based_Infrared_System), and [OPIR](https://www.afspc.af.mil/News/Article-Display/Article/1959289/improving-lives-maximizing-taxpayer-dollars-with-dual-use-space-capabilities/)). It is not the first time that someone calls for these satellites to be used for [wildfire detection](https://spacenews.com/editorial-using-dsp-fire-detection/), one can find such calls from 10 years ago; plausibly giving non-military entities access to currently-classified information is no trivial matter. Who knows when this will actually be available.

## Land-based systems

Satellites may be able to see everything, but so can a bunch of cameras on poles with some sprinkled matrix multiplications on top. That's what startups [Pano.ai](https://pano.ai/) or [Fireball](https://www.fireball.international/) does and it seems like a great idea to me! It may not be able to detect all fires (unless all of California ends up tiled with cameras) but perhaps it will be able to detect those that are nearby inhabited areas. One could even mount the cameras on airships and have they survey larger areas continuously. Or, for that matter, [drones](https://en.wikipedia.org/wiki/Drones_in_wildfire_management).

## Early-stage firefighting?

Fire is like cancer: it's easier to fight if caught early. Could we fight fires just as they happen? For that we need early detection and relatively fast response capabilities. An attempt at providing the latter is [Rain](https://www.rain.aero/), using drones dropping these [balls](https://www.amazon.com/Automatic-Extinguisher-Mounting-Bracket-Suppression/dp/B074KZDNKY/ref=sr_1_5?dchild=1&keywords=fire+extinguishing+ball&qid=1626189345&sr=8-5) into fires to extinguish them. This may work for some fires under some weather conditions, say when there is not much wind _and_ the fire is located in the radius of action of the drone _and_ there is a way to detect the fire before it grows beyond a very small size. \[Edit 2022-10-24\]: Since then Rain has continued to make progress and now they have larger drones and a new way of suppressing fires.

I'm not that optimistic about the use of _small_ drones to fight fires in general. Though it may be an intermediate step to better solutions like [using](https://aircargoeye.com/russias-cargo-airship-not-just-hot-air/) [airships](https://www.newscientist.com/article/dn2425-gigantic-airships-aim-to-damp-forest-fires/). Airships can hold potentially more water than the largest aerial firefighting vehicle in the world, the 747 [Supertanker](https://en.wikipedia.org/wiki/747_Supertanker) can hold. Cool as it may seem one of those companies (Wetzone Engineering) didn't seem to have succeeded and their patent is now [expired](https://patents.google.com/patent/US6769493B1/en). But the good thing about airships is that they don't need energy to stay aloft (one could tether them and release them as needed), and they can carry more water than the usual drones. Fighting fire fast requires a combination of early detection, and timely action which can in turn be achieved by a combination of pre-positioning resources and high speed delivery.

## Better communication

Something that was pointed out to me that _once_ a fire has been reported to the relevant fire department, they need to find exactly where it is and that deep in the wildland there is no WiFi, in many spots there is no long-distance radio and depending on tree coverage and mountains, no satellite-phones that work either. So they have to spot the fire and then drive all the way back to where they can notify the rest of the world. Installing a network of radio or WiFi or phone antennas would be a start. [FirstNet](https://en.wikipedia.org/wiki/First_Responder_Network_Authority) (which has been in the making for [more than a decade](https://www.theatlantic.com/politics/archive/2015/09/why-police-and-firefighters-struggle-to-communicate-in-crises/457443/), lol) might be part of the answer.

## Prevention

## Natural causes - Reducing lightning strikes?

Other than volcanoes, lightning strikes are the reason why wildfires occur without human action. Lightning strikes happen due to large scale weather patterns which are as hard to change as they sound. Is there anything that we could do to reduce the number of lightning strikes? [DARPA](https://www.tampabay.com/news/environment/in-florida-nations-only-lightning-center-closes-after-darpa-cuts-funding/2331129/) had one such program a few years ago which recently closed, coming empty-handed. Short of lots of lightning rods, there is not much that can be done here.

## Human causes

### Powerlines

Among the cause of fire that have to do with us, powerlines breaking down is a recurring one. Currently the way to deal with this is to shut off powers to the lines, leading to blackouts in the affected areas. But this doesn't always work, it requires making the decision to cut power and this is not always the route [taken](https://www.greentechmedia.com/articles/read/why-pge-didnt-cut-power-to-possibly-prevent-californias-deadliest-wildfire).

A better solution (that reduces fire risk AND ensures permanent supply) is to underground the lines. It is already the default in many countries and while expensive, the costs could be cheaper than the fires that overground lines lead to. This is easier done than: California has given [some money](https://www.ktvu.com/news/money-slated-to-put-pge-power-lines-underground-was-diverted-for-years-2-investigates) to state utility PG&E to do this and yet the company is using the money for something else. Undergrounding may be expensive and require fighting [Big Woodpole](https://woodpoles.org/portals/2/documents/TB_Undergrounding.pdf) (this is a thing, there's a woodpole lobby). A way to reduce costs here would be to use continuous cable-laying and burying machines (just like the Boring Company is [doing](https://www.boringcompany.com/prufrock) with tunnels). I have never seen these being used but I searched for them and sure enough there is a company in [Germany](https://www.youtube.com/watch?v=d7XSMHUzA5k) that has something like that, being used for fiber optic. Would need scaling up but something like this may make it possible. The project may involve wrestling away PG&E's assets from the company (probably can't trust PG&E to do it!).

At a cost of say [3M$ per mile](https://www.desertsun.com/story/news/environment/2019/10/11/cost-to-bury-california-fire-prone-power-lines-why-not/3937653002/), and 100k miles of overhead lines, undergrounding the whole thing may require 300bn$. Spread over say 10 years it would suppose 10% of California's budget. Cutting costs in half with continuous cable laying and prioritizing fire-prone areas might make this more manageable. Worth doing.

#### Taking over PG&E

The case has been made elsewhere that PG&E, an investor-owned utility that provides service in most of California (including the area most prone to fire), is not being as [properly](https://calmatters.org/economy/2019/01/pge-tubbs-fire-california-hate/) [managed](https://www.sfchronicle.com/business/article/PG-E-diverted-millions-from-burying-overhead-14897319.php#:%7E:text=PG%26E%20diverted%20%24123%20million%20from,California%20Public%20Utilities%20Commission%20found.) as it [should](https://www.cnn.com/2018/12/15/us/pge-falsifying-records/index.html). [Taking over it](https://www.gov.ca.gov/wp-content/uploads/2019/04/Wildfires-and-Climate-Change-California%E2%80%99s-Energy-Future.pdf) is one of the proposals in a report on wildfires from the office of Gavin Newsom. One can think of this as a form of self-defense on behalf of the population of the state: It's fine to have investor-owned utilities providing service as long as they don't extract monopolistic rents, but if it's indeed true that PG&E is exposing the State to unacceptably high levels of risk then there's a case to be made for installing better management. This could be accomplished in many ways: From keeping the current company but forcing a reorganization to nationalization or municipalization.

### The rest

Other than powerline failure, the rest of the causes of fire are very diverse (say, a hot tub malfunctioning as in the [Valley Fire](https://www.cnn.com/2016/08/11/us/california-valley-fire-faulty-hot-tub/index.html#:%7E:text=The%20blaze%20started%20when%20a,an%20extensive%20173%2Dpage%20report.)) and there are already electrical safety codes that should prevent this, but equipment deteriorates and I doubt there's capacity to enforce this so ensuring that no use of electricity in the WUI leads to any fire seems hopeless.

## Reducing the spread of fire

Once fire has started, what could be done to slow it down so that it's easier to manage? The obvious option here include doubling down on what is already being done like prescribed fire. But also there are others that are more ambitious.

Slowing down fire spread means creating areas that are hard to burn (firebreaks and prescribed fire), increasing humidity (potentially, but unlikely, cloud seeding) and reducing temperature (short of large-scale geoengineering, also unlikely)

## Managing WUI fires with superfirebreaks

Most of the other causes of WUI fires are "Unknown" or "Miscellaneous causes" as described in the [earlier post](https://nintil.com/wildfires-california). When there are multiple causes (Or we don't know what they are) it's best to see if there's somewhere downstream of all of those that can address all these causes at once. These fires may have diverse origins but most of them (with the exception of campfires) take place in the Wildland-Urban Interface (WUI). In relative terms it is possible to reduce the population in the WUI by making it less likely that people will want to live there (through zoning it out of existence, higher insurance premia (or broadly fairly priced [insurance](https://getdelos.com/)), making it cheaper to live away from nature).

In some cases it could be possible to have large firebreaks (There is precedent for a very large [one](https://www.sfchronicle.com/california-wildfires/article/An-800-mile-firebreak-once-protected-15713546.php) in California) around the entire community, say an 8-lane wide area paved with concrete to make sure nothing grows there, circling a given town. Expensive? Probably, but would provide some level of protection. It would be far from completely fireproof, under heavy winds embers could cross even that. But they would double as ring roads. This approach would both make it harder that fire will threaten a given community as well as fire getting out of one so a given fire will be localized and not spread beyond it. Prescribed burns accomplish the same, but unlike prescribed burns these permanent firebreaks don't need constant maintenance or high manpower needs on a yearly basis.

## Sprinklers

Using [sprinklers](https://www.youtube.com/watch?v=IQNm543k1X4) to protect [houses](https://wildfiretoday.com/documents/Outdoor_sprinkler_systems_effectiveness.pdf) is an old idea, but apparently some insurers don't trust the systems enough to reduce [premia](https://www.sfchronicle.com/business/networth/article/To-keep-insurance-some-homeowners-now-have-to-do-14294099.php) based on this, I was told. Perhaps because in some cases (like if a house nearby is on fire) they are not as [effective](https://www.nfpa.org/-/media/Files/Firewise/Fact-sheets/FirewiseFactSheetsExteriorSprinklers.pdf).

Another use of sprinklers is as temporary firebreaks on demand and to increase moisture in some parts of the state. There are some issues around water pressure or pipe diameter that would have to be worked out and this is bound to be expensive, potentially more than the concrete firebreaks, but maybe they could be used [strategically](https://www.malibutimes.com/news/article_8d4cf952-4112-11e9-82fa-eb505ab97d5f.html) in highly populated areas with a lower visual impact that paving everything with concrete would have.

## Cloud seeding

> I have become convinced that it is possible under proper circumstances to make rain.... we are on the threshold of an exceedingly important matter, for man has begun for the first time to affect the weather in which he lives, and no man can tell where such a move will finally end. - Vannevar Bush ([1951](https://www.google.com/books/edition/Hearings/p5sBttF4IXgC?hl=en&gbpv=1&bsq=become%20convinced))

If we could make it rain on demand, that could help with various issues: Depending on the amount of rain it could help with droughts, to reduce fuel aridity, as well as fight fires directly if one could induce very heavy rain.

Efforts to artificially increase rain are as old as Man, and just like alchemy beckoned chemistry, rain dances led to [cloud seeding](https://www.scientificamerican.com/article/eight-states-are-seeding-clouds-to-overcome-megadrought/), which is now widely used by multiple nations.

The problem is that it's unclear if it even works, especially for its stated aims.

The physical principles behind cloud seeding involves dispersing substances that could act as nuclei on which water would condensate and precipitate as rain. This is how rain in general forms, by [nucleating](https://physics.stackexchange.com/questions/273710/why-does-atmospheric-water-vapor-need-dust-particles-to-fall-as-rain) around particles like dust, dancing around a fire and praying to the rain gods [makes sense](https://en.wikipedia.org/wiki/Flammagenitus_cloud) if the fire is large enough.

So it stands to reason that adding _more_ nucleation centers would, all else equal, increase rain. Except that we may have been misled by assuming linearity and only a small amount of dust is required, and any additional particles released into the atmosphere do very little.

The rainmaking scene could be seen as analogous to the burgeoning field of snake oil salesman, but unlike snake oil, testing if rain on demand actually works is really hard to test. House names included [Bernard Vonnegut](https://en.wikipedia.org/wiki/Bernard_Vonnegut), brother of the more famous sci-fi author Kurt Vonnegut

So at some point the US government organized Project [Skywater](https://webcache.googleusercontent.com/search?q=cache:XDmAYG2yMfgJ:https://www.usbr.gov/history/ProjectHistories/Project_Skywater_D1%5B1%5D.pdf+&cd=1&hl=en&ct=clnk&gl=us) (1965) where the US Bureau of [Reclamation](https://www.usbr.gov/) tried silver iodide, [sulfuric acid](https://www.sciencedaily.com/releases/2014/10/141008083636.htm), salt, dry ice, and urea over a number of US states. It was considered inconclusive.

Why these compounds? Well, just [trial and error](https://www.ge.com/news/reports/cool-science-vonnegut-ge-research),

> Schaefer was testing different materials in his lab by dropping them into a repurposed home freezer. Working with dry ice, he discovered that tiny grains of the substance, which is carbon dioxide frozen to minus 109.3 Fahrenheit (minus 78.5 Celsius), transformed the air inside the freezer, created many millions of ice crystals and started a miniature snowstorm.
> 
> By fall 1946, Schaefer knew enough to take his research out of the lab. In November that year, he and GE test pilot Curtis Talbot climbed into a small Fairchild plane and tried to seed with dry ice a cloud floating above Schenectady ([see video](https://www.youtube.com/watch?time_continue=5&v=2D5s2FlA_5k&feature=emb_logo)). “Curt flew into the cloud and I started the dispenser in operation,” Schaefer wrote in his lab [notebook](https://www.google.com/books/edition/Fixing_the_Sky/zmdBon09PY0C?hl=en). “I dropped about three pounds (of dry ice) and then swung around and headed south. About the time I looked toward the rear, I was thrilled to see long streamers of snow falling from the base of the cloud through which we had just passed.”
> 
> The team kept working and Bernard Vonnegut improved on the method by using silver iodide, a substance whose crystals are similar to frozen water, to summon snow. The chemical compound was apparently so powerful at creating snow or rain that “all the air of the United States could be nucleated at one time with a few pounds of silver iodide,”

A [review](https://journals.ametsoc.org/view/journals/bams/80/5/1520-0477_1999_080_0805_arocse_2_0_co_2.xml) from 1999 notes that _In the scientific community weather modification is still viewed as a somewhat controversial topic._

A [recent](https://www.scientificamerican.com/article/eight-states-are-seeding-clouds-to-overcome-megadrought/) article at Scientific American highlights the fact that these techniques continue to be used (despite the lack of solid evidence) and points to some studies that do seem to show that seeding works.

The first report they cite is disappointing for proponents of seeding,

> In 2003, the National Research Council published a comprehensive [report](https://www.nap.edu/catalog/10829/critical-issues-in-weather-modification-research) on weather modification, highlighting these problems. It concluded that “there is still no convincing scientific proof of the efficacy of intentional weather modification efforts.”
> 
> Still, NRC recommended continued research on weather modification—in no small part because of its potential to address the West’s worsening water concerns.

Has anything changed since that? [This report](https://www.usbr.gov/main/qoi/docs/Literature_Review_and_Scientific_Synthesis_of_the_Efficacy_of_Winter_Orographic_Cloud_Seeding_Peer_Review.pdf) also cited in the essay aims to answer precisely and they conclude in a way that is worth quoting in full because the Scientific American essay makes it sound more pessimistic:

> Based on both the historical evidence and the last decade of research, **it is reasonable to conclude that artificial enhancement of winter snowpack over mountain barriers is possible**. It is very difficult to quantify the seasonal increases to be expected both in snowpack and subsequent spring runoff. This is because each target area has to be investigated as to the meteorology of the winter clouds and their seedability, and the engineering aspects of effectively seeding the clouds to maximize increases. Winter orographic cloud seeding should thus continue to be supported both from the scientific and operational community working together to further the science and operational outcomes. It must be stated however, that as of yet, no rigorous scientific study conducted as a randomized confirmatory seeding experiment with pre-defined primary response variables and requiring an established threshold of statistical significance has demonstrated that seeding winter orographic clouds increases snowfall. As such, **the “proof” the scientific community has been seeking for many decades is still not in hand.**

But in 2017, they mention, for the first time ever there's some solid-seeming evidence from project SNOWIE. The figure below (from Tessendorf et al., [2019](https://journals.ametsoc.org/view/journals/bams/100/1/bams-d-17-0152.1.xml)) shows that the rain in the test area for SNOWIE exceeded anything seen since 1988 (IOP is a seeding event; not that they are always causal, seeding happens when there are clouds that can be leveraged and those may discharge rain anyway),

![Fig. 5.](https://journals.ametsoc.org/view/journals/bams/100/1/full-bams-d-17-0152.1-f5.jpg)

The paper itself (Friedrich et al., [2020](https://www.pnas.org/content/117/10/5190)) estimates that the seeding operation during SNOWIE doubled the precipitation that would have otherwise occurred in the area of interest. But that seems to have been not that much as the [press release](https://www.colorado.edu/today/2020/02/24/cloud-seeding) that accompanied the study goes on to say that

> Based on the team’s calculations, snow fell from those clouds for about 67 minutes, dusting roughly 900 square miles of land in about a tenth of a millimeter of snow. It was barely enough snow to cling to the researchers’ eyelashes. But it was water that, if not for cloud seeding, would have stayed in the air.
> 
> “If we hadn’t seeded these clouds, they would not have produced any precipitation,” Friedrich said.

The additional rain that was generated during SNOWIE amounted to 282 Olympic pools, so 705k m^3 of additional water. This was over an approximate 100 km x 100 km patch of Idaho.

How much would we get from light rain, for reference? A very light rain (1mm per square meter, in 1 hour) over that entire area would suppose (100 km x 100 km) x 1 mm = 10000k m3, which is 14x times that which indeed doesn't sound very impressive.

The literature on cloud seeding seems to agree that only some clouds can be seeded, so the effectiveness of the practice would depend on where it's applied; for example [this repor](https://www.researchgate.net/profile/Shih-Yu-Wang-2/publication/344013366_A_modeling_examination_of_cloud_seeding_conditions_under_the_warmer_climate_in_Utah_USA/links/5f4dbfe8458515a88ba34511/A-modeling-examination-of-cloud-seeding-conditions-under-the-warmer-climate-in-Utah-USA.pdf)t for Utah suggests that with increasing temperatures and decreased rain the percentage of clouds that can be seeded decreases as well. Cloud seeding, if it works, would increase the amount of rain for clouds that are loaded with water but don't quite precipitate. These clouds are vastly more likely to be present during winter. Using it during the driest summers wouldn't probably work, and the amount of rain involved wouldn't be enough to extinguish fires by itself.

So all things considered, cloud seeding works but the extent to which is does is unclear so I wouldn't expect this to be very useful for wildfire prevention or mitigation. More research is needed.

## Vegetation replacement

California has the vegetation it has for historical reasons, but that may not be the optimal vegetation for our interests. Some have [pointed](https://www.usgs.gov/centers/werc/science/invasive-plants-and-fire-0?qt-science_center_objects=0#qt-science_center_objects) out that some invasive species may burn more easily. We could do the opposite: Figure out if we can upgrade California's ecosystem to one that is more fire-resistant, accelerating what evolution might do if given enough time. Some species like the [Canary Islands pine](https://link.springer.com/article/10.1023/B:VEGE.0000029374.64778.68) or [cypresses](https://www.smithsonianmag.com/innovation/can-cypress-trees-help-suppress-wildfires-180956597/) can resist fires, but whether they will grow in California is another matter.

## Better predictive models

Knowing where the fire is going to go can help firefighting efforts. There already exists software that does that like [Technosylva](https://www.wildfireanalyst.com/about-technosylva/) and there is at least one large tech company that is working on in-house models (I know because they told me. Information from the previous Wildfires post I wrote was part of a pitch deck they used internally to justify their work).

## Conclusion

There is no single solution to the problem of wildfires. There are measures that could be taken to reduce the frequency and area of fires, as well as to reduce loss of life and property. But there is either no one solution to the problem nor a feasible set of solutions (at least right now).

_Thanks to John Clarke Mills, interim CTO of Pano.ai for helpful comments_

_Ricón, José Luis, “Wildfires in California (II): some solutions”, Nintil (2021-07-13), available at https://nintil.com/managing-wildfires/._