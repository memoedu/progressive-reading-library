---
title: Internet Search Case Studies
description: Real-world examples of using advanced tips and tricks for effective Internet research of papers/books.
thumbnail: /doc/cs/linkrot/archiving/gwern-googlescholar-search-highlightfulltextlink-thumbnail.jpg
thumbnail-text: "Screenshot of Google Scholar search results, with an arrow pointing to the desirable fulltext link in these results, which many users are unaware of."
thumbnail-css: "outline invert-not"
created: 2019-09-23
modified: 2026-02-24
status: finished
confidence: certain
importance: 3
css-extension: dropcaps-kanzlei
...

<div class="abstract">
> Appendix to the [Internet Search Tips article](/search "‘Internet Search Tips’, Gwern 2018"){.backlink-not} covering how to search the Internet effectively: >14 case studies of challenging Internet searches drawn from the past 10 years.
>
> I present the problem, and step through the process of finding it, and describe my tacit knowledge and implicit strategies.
>
> These case studies make the prior tips more understandable by showing them off in practice.
</div>

# Littlewood

[**See main article.**](/littlewood-origin "‘Origin of ‘Littlewood’s Law of Miracles’’, Gwern 2019"){.include-annotation .redirect-from-id}

# Missing Appendix

[Anders Sandberg](!W) [asked](https://x.com/anderssandberg/status/1176040327679029249):

> Does anybody know where the online appendix to Nordhaus' ["Two Centuries of Productivity Growth in Computing"](https://pdfs.semanticscholar.org/f60f/e757587be15c29ad6ee5695bc48a44df3e8a.pdf "Nordhaus 2007") is hiding?

I look up the title in Google Scholar; seeing a friendly `psu.edu` PDF link (CiteSeerx), I click.
The paper says "The data used in this study are provided in a background spreadsheet available at `http://www.econ.yale.edu/~nordhaus/Computers/Appendix.xls`".
Sadly, this is a lie. (Sandberg would, of course, have tried that already.)

I immediately check the URL in the IA---nothing.
The IA didn't catch it at all. Maybe the [official published paper website](https://www.cambridge.org/core/journals/journal-of-economic-history/article/two-centuries-of-productivity-growth-in-computing/856EC5947A5857296D3328FA154BA3A3 "'Two Centuries of Productivity Growth in Computing', Nordhaus 2007") has it? Nope, it references the same URL, and doesn't provide a copy as an appendix or supplement. (What do we pay these publishers such enormous sums of money for, exactly?)
So I back off to checking `http://www.econ.yale.edu/~nordhaus/`, to check Nordhaus's personal website for a newer link.
The Yale personal website is empty and appears to've been replaced by a Google Sites personal page. It links nothing useful, so I check a more thorough index, Google, by searching `site:sites.google.com/site/williamdnordhaus/`. Nothing there either (and it appears almost empty, so Nordhaus has allowed most of his stuff to be deleted and bitrot).
I try a broader Google: `nordhaus appendix.xls`. This turns up some spreadsheets, but still nothing.

Easier approaches having been exhausted, I return to the IA and I pull up *all* URLs archived for his original personal website: `https://web.archive.org/web/*/http://www.econ.yale.edu/~nordhaus/*`
This pulls up way too many URLs to manually review, so I filter results for `xls`, which reduces to a more manageable 60 hits; reading through the hits, I spot `http://www.econ.yale.edu/~nordhaus/homepage/documents/Appendix_Nordhaus_computation_update_121410.xlsx` from 2014-10-10; this sounds right, albeit substantially later in time than expected (either 2010 or 2012, judging from the filename).

[Downloading it](/doc/cs/hardware/2010-nordhaus-nordhaus2007twocenturiesofproductivitygrowthincomputing-appendix.xlsx), opening it up and cross-referencing with the paper, it has the same spreadsheet 'sheets' as mentioned, like "Manual" or "Capital_Deep", and seems to be either the original file in question or an updated version thereof (which may be even better).
The spreadsheet metadata indicates it was created `04/09/2001, 23:20:43, ITS Academic Media & Technology`, and modified `12/22/2010, 02:40:20`, so it seems to be the latter---it's the original spreadsheet Nordhaus created when he began work several years prior to the formal 2007 publication (6 years seems reasonable given all the delays in such a process), and then was updated 3 years afterwards.
Close enough.

# Misremembered Book

[A Redditor asked](https://www.reddit.com/r/science/comments/d8dcm0/todays_obesity_epidemic_may_have_been_caused_by/f1ac2sy/):

> I was in a consignment type store once and picked up a book called "Eat fat, get thin". Giving it a quick scan through, it was basically the same stuff as Atkins but this book was from the 50s or 60s. I wish I'd have bought it. I think I found a reference to it once online but it's been drowned out since someone else released a book with the same name (and it wasn't Barry Groves either).

The easiest way to find a book given a corrupted title, a date range, and the information there are many similar titles drowning out a naive search engine query, is to skip to a specialized search engine with clean metadata (ie. a library database).

Searching in WorldCat for 1950s--1970s, "Eat fat, get thin" turns up nothing relevant. This is unsurprising, as he was unlikely to've remembered the title *exactly*, and this title doesn't quite sound right for the era anyway (a little too punchy and ungrammatical, and 'thin' wasn't a desirable word back then compared to words like 'slim' or 'sleek' or 'svelte'). People often oversimplify titles, so I dropped back to just "Eat fat".

This immediately turned up the book: [Richard Mackarness's](!W "Richard Mackarness") 1958 _Eat Fat and Grow Slim_---note that it is *almost* the same title, with a comma serving as conjunction and 'slim' rather than the more contemporary 'thin', but just different enough to screw up an overly-literal search.

With the same trick in mind, we could also have found it in a regular Google search query by adding additional terms to hint to Google that we want old books, not recent ones: both `"Eat Fat" 1950s` or `"Eat Fat" 1960s` would have turned it up in the first 5 search results. If we didn't use quotes, the searches get harder because broader hits get pulled in. For example, `Eat fat, get thin 1950s -Hyman` excludes the recent book mentioned, but you still have to go down 15 hits before finding Mackarness, and `Eat fat, get thin -Hyman` requires going down 18 hits.

# Missing Website

[Bučar et al 2015](https://onlinelibrary.wiley.com/doi/full/10.1002/anie.201410356 "Disappearing Polymorphs Revisited"), on the phenomenon of [disappearing polymorphs](https://en.wikipedia.org/wiki/Disappearing_polymorph) quotes striking transcripts from a major example of a disappearing crystal, when ~1998 Abbott suddenly became unable to manufacture the anti-retroviral drug [ritonavir](!W) (Norvir™) due to a rival (and less effective) crystal form spontaneously infecting all its plants, threatening many AIDS patients, but notes:

> The transcripts were originally published on the website^42^ of the International Association of Physicians in AIDS Care [IAPAC], but no longer appear there.

A search using the quotes confirms that the originals have long since vanished from the open Internet, turning up only quotes of the quotations. Unfortunately, no URL is given. The Internet Archive has comprehensive mirrors of the IAPAC, but too many to easily search through. Using the filter feature, I keyword-searched for "ritonavir", but while this turned up a number of pages from roughly the right time period, they do not mention it and none of the quotes appear. The key turned out to be to use the trademark name instead which pulls up many more pages, and after checking a few, the IAPAC turned out to have organized all the Norvir material into a single subdirectory with a convenient [`index.html`](/doc/biology/2000-iapac-norvir/index.html); the articles/transcripts, in turn, were indexed under the linked ["Description of the Problem" index page](/doc/biology/2000-iapac-norvir/description.html "'Norvir Advisory', Care 2000").

I then pulled the Norvir subdirectory with a `~/.gem/ruby/2.5.0/bin/wayback_machine_downloader wayback_machine_downloader 'http://www.iapac.org/norvir/'` command and hosted a mirror to make it visible in Google.

# Speech → Book

[Nancy Lebovitz](https://www.lesswrong.com/posts/cHEQSEPz4eipGHFy9/differential-reproduction-for-men-and-women#comment-TZQqzYPDv6vsWqfut) asked about a citation in a [Roy Baumeister speech about sex differences](https://web.archive.org/web/20071011022938/https://psy.fsu.edu/~baumeistertice/goodaboutmen.htm "'Is There Anything Good About Men?', Roy F. Baumeister 2007"):

> There's an idea I've seen a number of times that 80% of women have had descendants, but only 40% of men. A little research tracked it back to [this](https://web.archive.org/web/20071011022938/https://psy.fsu.edu/~baumeistertice/goodaboutmen.htm "‘Is There Anything Good About Men?’, Baumeister 2007"), but the speech doesn't have a cite and I haven't found a source.

This could be solved by guessing that the formal citation is given in the book, and doing keyword search to find a similar passage. The second line of the speech says:

> > For more information on this topic, read Dr. Baumeister's book *Is There Anything Good About Men?* available in bookstores everywhere, including here.

A search of _Is There Anything Good About Men_ in Libgen turns up a copy. Download. What are we looking for? A reminder, the key lines in the speech are:

> > ...It's not a trick question, and it's not 50%. True, about half the people who ever lived were women, but that's not the question. We're asking about all the people who ever lived who have a descendant living today. Or, put another way, yes, every baby has both a mother and a father, but some of those parents had multiple children. Recent research using DNA analysis answered this question about two years ago. Today's human population is descended from twice as many women as men. I think this difference is the single most under-appreciated fact about gender. To get that kind of difference, you had to have something like, throughout the entire history of the human race, maybe 80% of women but only 40% of men reproduced.

We could search for various words or phrase from this passage which seem to be relatively unique; as it happens, I chose the rhetorical "50%" (but "80%", "40%", "underappreciated", etc. all would've worked with varying levels of efficiency since the speech is heavily based on the book), and thus jumped straight to chapter 4, "The Most Underappreciated Fact About Men". (If these had not worked, we could have started searching for years, based on the quote "about two years ago".) A glance tells us that Baumeister is discussing exactly this topic of reproductive differentials, so we read on and a few pages later, on page 63, we hit the jackpot:

> The correct answer has recently begun to emerge from DNA studies, notably those by Jason Wilder and his colleagues. They concluded that among the ancestors of today's human population, women outnumbered men about two to one. Two to one! In percentage terms, then, humanity's ancestors were about 67% female and 33% male.

Who's Wilder? A C-f for "Wilder" takes us to pg286, where we immediately read:

> ...The DNA studies on how today's human population is descended from twice as many women as men have been the most requested sources from my earlier talks on this. The work is by Jason Wilder and his colleagues. I list here some sources in the mass media, which may be more accessible to laypersons than the highly technical journal articles, but for the specialists I list those also. For a highly readable introduction, you can Google the article ["Ancient Man Spread the Love Around,"](https://web.archive.org/web/20040922020546/https://www.scienceagogo.com/news/20040819224859data_trunc_sys.shtml) which was published September, 20, 2004 and is still available (last I checked) online. There were plenty of other stories in the media at about this time, when the research findings first came out. In ["Medical News Today,"](https://www.medicalnewstoday.com/), on the same date in 2004, a story under "Genes expose secrets of sex on the side" covered much the same material.
>
> If you want the original sources, read Wilder, J. A., Mobasher, Z., & Hammer, M. F. (2004). ["Genetic evidence for unequal effective population sizes of human females and males"](https://academic.oup.com/mbe/article/21/11/2047/1147770). _Molecular Biology and Evolution_, 21, 2047--2057. If that went down well, you might try Wilder, J. A., Kingan, S. B., Mobasher, Z., Pilkington, M. M., & Hammer, M. F. (2004). ["Global patterns of human mitochondrial DNA and Y-chromosome structure are not influenced by higher migration rates of females versus males"](https://www.nature.com/articles/ng1428). _Nature Genetics_, 36, 1122--1125. That one was over my head, I admit. A more readable source on these is Shriver, M. D. (2005), ["Female migration rate might not be greater than male rate"](https://www.nature.com/articles/5201329). _European Journal of Human Genetics_, 13, 131--132. Shriver raises another intriguing hypothesis that could have contributed to the greater preponderance of females in our ancestors: Because couples mate such that the man is older, the generational intervals are smaller for females (ie. baby's age is closer to mother's than to father's). As for the 90% to 20% differential in other species, that I believe is standard information in biology, which I first heard in one of the lectures on testosterone by the late James Dabbs, whose book _Heroes, Rogues, and Lovers_ remains an authoritative source on the topic.

Wilder et al 2004, incidentally, fits well with Baumeister remarking in 2007 that the research was done 2 or so years ago. And of course you could've done the same thing using Google Books: search ["Baumeister anything good about men"](https://books.google.com/books?id=qqprY-YiWY8C&printsec=frontcover&dq=Baumeister+anything+good+about+men&sa=X&ei=4vNaUZffIoe9igLB74DQBA&ved=0CDAQ6AEwAA) to get to the book, then search-within-the-book for "50%", jump to page 53, read to page 63, do a second search-within-the-book for "Wilder" and the second hit of page 287 even luckily gives you the snippet:

> *Sources and References* 287
>
> ...If you want the original sources, read Wilder, J. A., Mobasher, Z., & Hammer, M. F. (2004). "Genetic evidence for unequal effective population sizes of human females and males". _Molecular Biology and Evolution_...

# Rowling Quote On Death

Did [J.K. Rowling](!W) say the _Harry Potter_ books were about 'death'? There are a lot of Rowling statements, but checking WP and opening up each interview links (under the theory that the key interviews are linked there) and searching for 'death' soon turns up a relevant quote from [2001](http://www.accio-quote.org/articles/2001/1201-bbc-hpandme.htm "'Harry Potter and Me' (BBC Christmas Special, British version), BBC, 2001-12-28"):

> Death is an extremely important theme throughout all seven books. I would say possibly the most important theme. If you are writing about Evil, which I am, and if you are writing about someone who is essentially a psychopath, you have a duty to show the real evil of taking human life.

# Crowley Quote

[Scott Alexander](https://www.lesswrong.com/posts/vhxywjnBH6ioRnnt3/crowley-on-religious-experience#comment-3bc4kL4QnNc9TjNTz) posted a piece linking to an except titled "[Crowley](!W "Aleister Crowley") on Religious Experience".

The link was broken, but Alexander brought it up in the context of an [earlier discussion](https://www.lesswrong.com/posts/Fwt4sDDacko8Sh5iR/the-sacred-mundane#comment-qAHp6JRjeY7ixgYas) where he also quoted Crowley; searching *those* quotes reveals that it must have been excerpts from _Magick: Book 4_.

# Finding The Right 'SAGE'

[Phil Goetz](https://www.lesswrong.com/posts/CKpByWmsZ8WmpHtYa/competent-elites#comment-jzCu3bdgoQ77Y5hZN) noted that an anti-aging conference named "SAGE" had become impossible to find in Google due to a *LGBT* aging conference also named SAGE.

Regular searches would fail, but a combination of tricks worked: `SAGE anti-aging conference` combined with restricting Google search to 2003--2005 time-range turned up a citation to its website as the fourth hit, `http://www.sagecrossroads.net` (which has ironically since died).

# UK Charity Financials

The [Future of Humanity Institute (FHI) doesn't clearly provide](https://www.lesswrong.com/posts/qqhdj3W3vSfB5E9ss/siai-an-examination?commentId=7CwWf6wN2DtNfv3tF) charity financial forms akin to the US Form 990s, making it hard to find out information about its budget or results.

FHI doesn't show up in the CC, NPC, or GuideStar, which are the first places to check for charity finances, so I went a little broader afield and tried a site search on the FHI website: `budget site:fhi.ox.ac.uk`. This immediately turned up FHI's own documentation of its activities and budgets, such as the 2007 annual report; I used part of its title as a new Google search: `future of humanity institute achievements report site:fhi.ox.ac.uk`.

# Nobel Lineage Research

[John Maxwell](https://www.lesswrong.com/posts/hC83eKp9LFpw9FBks/link-holistic-learning-ebook#comment-egNZtRAF8C2KTPKsu) referred to a forgotten study on high correlation between Nobelist professors & Nobelist grad students (almost entirely a selection effect, I would bet). I was able to refind it in 7 minutes.

I wasted a few searches like `factor predicting Nobel prize` or `Nobel prize graduate student` in Google Scholar, until I searched for `Nobel laureate "graduate student"`; the second hit was a citation, which is a little unusual for Google Scholar and meant it was important, and it had the critical word *mutual* in it---simultaneous partners in Nobel work is somewhat rare, but temporally separated teams don't work for prizes, and I suspected that it was exactly what I was looking for.

Googling the title, I soon found a PDF like ["Eminent Scientists' Demotivation in School: A symptom of an incurable disease?", Viau 2004](/doc/psychology/2004-viau.pdf) which confirmed it (and Viau 2004 is interesting in its own right as a contribution to the Conscientious vs IQ question).
I then followed it to a useful paragraph:

> In a study conducted with 92 American winners of the Nobel Prize, Zuckerman (1977) discovered that 48 of them had worked as graduate students or assistants with professors who were themselves Nobel Prize award-winners. As pointed out by Zuckerman (1977), the fact that 11 Nobel prizewinners have had the great physicist Rutherford as a mentor is an example of just how significant a good mentor can be during one's studies and training. It then appears that most eminent scientists did have people to stimulate them during their childhood and mentor(s) during their studies. But, what exactly is the nature of these people's contribution.
>
> * Zuckerman, H. (1977). *Scientific Elite: Nobel Laureates in the United States*. New York: Free Press.

GS lists >900 citations of this book, so there may well be additional or followup studies covering the 40 years since.
Or, also relevant is "Zuckerman, H. (1983). The scientific elite: Nobel laureates' mutual influences. In R. S. Albert (Ed.), *Genius and eminence* (pp. 241--252). New York: Pergamon Press", and "Zuckerman H. 'Sociology of Nobel Prizes', *Scientific American* 217 (5): 25& 1967."

# Dead URL

[A link to a research article in a post by Morendil](https://www.lesswrong.com/posts/wAhgxmCf2ebHha5BJ/psa-please-list-your-references-don-t-just-link-them#comment-MMH9H4Y7iNDjfirZF) broke, he had not provided any formal citation data, *and* the original domain blocks all crawlers in its `robots.txt` so IA would not work. What to do?

The simplest solution was to search a direct quote, turning up a Scribd mirror; Scribd is a parasite website, where people upload copies from elsewhere, which ought to make one wonder where the *original* came from. (It often shows up before the original in any search engine, because it automatically runs OCR on submissions, making them more visible to search engines.) With a copy of the journal issue to work with, you can easily find the official HP archives and [download the original PDF](https://www.hpl.hp.com/hpjournal/pdfs/IssuePDFs/1989-04.pdf).

If that hadn't worked, searching for the URL without `/pg_2/` in it yields the full citation, and then that can be looked up normally. Finally, somewhat more dangerous would be trying to find the article just by author surname & year.

# Description But No Citation

A 2013 [Medical Daily](https://www.medicaldaily.com/psychologists-discover-how-people-subconsciously-become-their-favorite-fictional-characters-240435 "Psychologists Discover How People Subconsciously Become Their Favorite Fictional Characters") on the effects of reading fiction omitted any link or citation to the research in question. But it is easy to find.

The article says the authors are one Kaufman & Libby, and implies it was published in the last year. So: go to Google Scholar, punch in `Kaufman Libby`, limit to 'Since 2012'; and the correct paper (["Changing beliefs and behavior through experience-taking"](https://tiltfactor.org/wp-content/uploads2/Kaufman_Libby2012_JPSPadvanceonlinepublication.pdf "Kaufman & Libby 2012")) is the first hit with fulltext available on the right-hand side as the text link "[PDF] from `tiltfactor.org`" & many other domains.

# Finding Followups

[Is soy milk bad for you](https://www.lesswrong.com/posts/X6APQeHhXH9mbredM/soylent-orange-whole-food-open-source-soylent#comment-PWDQq3f5qSqWRc82c) as one study suggests? Has anyone replicated it? This is easy to look into a little if you use the power of reverse citation search!

Plug `Brain aging and midlife tofu consumption` into Google Scholar, one of the little links under the first hit points to "Cited by 176"; if you click on that, you can hit a checkbox for "Search within citing articles"; then you can search a query like `experiment OR randomized OR blind` which yields [121 results](https://scholar.google.com/scholar?q=experiment+OR+randomized+OR+blind&btnG=&as_sdt=20005&sciodt=0%2C9&cites=14459450472515815282&scipsc=1). The [first result](https://n.neurology.org/content/78/23/1841.short "'Long-term soy isoflavone supplementation and cognition in women: A randomized, controlled trial', Henderson et al 2012") shows no negative effect and a trend to a benefit, the second is inaccessible, the second & third are reviews whose abstract suggests it would argue for benefits, and the fourth discusses sleep & mood benefits to soy diets. At least from a quick skim, this claim is not replicating, and I am dubious about it.

# How Many Homeless?

Does NYC really have 114,000+ homeless school children? This case study demonstrates the critical skill of *noticing* the need to search at all, and the search itself is almost trivial.

[Won't someone think of the children?]{.margin-note} In March 2020, as [New York coronavirus cases began their exponential increase](!W "2020 coronavirus pandemic in the United States") centered in Manhattan (with a similar trend to Wuhan/Iran/Italy), NYC Mayor [Bill de Blasio](!W) refused to take social distancing/quarantine measures like ordering the NYC public school system closed, and this delay until 16 March contributed to the epidemic's unchecked spread in NYC; one justification was that there were "114,085 homeless children" who received social services like free laundry through the schools. This number has been widely cited in the media by the _NYT_, _WSJ_, etc., and was vaguely sourced to "state data" reported by "Advocates for Children of New York". This is a terrible reason to not deal with a pandemic that could kill tens of thousands of New Yorkers, as there are many ways to deliver services which do not require every child in NYC to attend school & spread infections---but first, is this number even true?

[Basic numeracy: implausibly-large!]{.margin-note} Activists of any stripe are untrustworthy sources, and a number like 114k should make any numerate person uneasy even without any [Fermi estimation](!W "Fermi problem") or fact-checking; "114,085" is suspiciously precise for such a difficult-to-measure or define thing like homelessness, and it's well-known that the population of NYC is ~8m or 8,000k---is it really the case that around 1 in every 70 people living in NYC is a homeless child age ~5--18 attending a public school? They presumably have at least 1 parent, and probably younger siblings, so that would bring it up to >228k or 1 in every <35 inhabitants of NYC being homeless in general. Depending on additional factors like transiency & turnover, the fraction could go much higher still. Does that make sense? No, not really. This quoted number is either surprising, or there is something missing.

[Redefining "homeless".]{.margin-note} Fortunately, the suspiciously-precise number and attribution make this a good place to start for a search. Searching for the number and the name of the activist group instantly turns up [the source press release](https://advocatesforchildren.org/articles/114000-nyc-students-were-homeless-last-school-year-new-data-reveals/ "New Data Show Number of NYC Students who are Homeless Topped 100,000 for Fourth Consecutive Year"), and the reasons for the bizarrely high number are revealed: the statistic actually redefines 'homelessness' to include living with relatives or friends, and counts any experience of any length in the previous year as rendering that student 'homeless' at the moment.

> The data, which come from the New York State Education Department, show that in the 2018–2019 school year, New York City district and charter schools identified 114,085, or one in ten, students as homeless.  More than 34,000 students were living in New York City's shelters, and more than twice that number (73,750) were living 'doubled-up' in temporary housing situations with relatives, friends, or others..."This problem is immense.  The number of New York City students who experienced homelessness last year---85% of whom are Black or Hispanic---could fill the Barclays Center six times," said Kim Sweet, AFC's Executive Director. "The City won't be able to break the cycle of homelessness until we address the dismal educational outcomes for students who are homeless."

The [_WSJ_'s article](https://www.coalitionforthehomeless.org/todays-read-new-york-city-had-114000-homeless-students-last-year/ "Today's Read: New York City Had 114,000 Homeless Students Last Year") (but not headline) confirms that 'experienced' does indeed mean 'at any time in the year for any length of time', rather than 'at the moment':

> City district and charter schools had 114,085 students without their own homes at some point last year, topping 100,000 for the fourth year in a row, according to state data released in a report Monday from Advocates for Children of New York, a nonprofit seeking better services for the disadvantaged. Most children were black or Hispanic, and living "doubled up" with friends, relatives or others. But more than 34,000 slept in city shelters at some point, a number larger than the entire enrollment of many districts, such as Buffalo, Rochester or Yonkers.

[Less than meet the eye.]{.margin-note} So the actual number of 'homelessness' (in the sense that everyone reading those media articles understands it) is less than a third the quote, 34k, and that 34k number is likely itself a loose estimate of how many students would be homeless at the time of a coronavirus closure. This number is far more plausible and intuitive, and while one might wonder about what the underlying NYS Education Department numbers would reveal if fact-checked further, that's probably unnecessary for showing how ill-founded the anti-closure argument is, since even by the activists' own description, the relevant number is far smaller than 114k.

# Citation URL With Typo

["Evolution of the Human Brain: From Matter to Mind", Hofman 2015](/doc/iq/high/2015-hofman.pdf "'Evolution of the Human Brain: From Matter to Mind', Hofman 2015"), discusses the limits to the intelligence of increasingly large primate brains due to considerations like increasing latency and overheating. One citation attempting to extrapolate upper bounds is "Biological limits to information processing in the human brain", Cochrane et al 1995.

The source information is merely a broken URL: `http://www.cochrane.org.uk/opinion/archive/articles.phd` which stands out for looking doubly-wrong: ".phd" is almost certainly a typo for ".ph*p*" (probably muscle memory on the part of Hofman from "PhD"), but it also gives a hint that the entire URL is wrong: why would an article or essay be named anything like `archive/articles.php`? That sounds like an *index* page listing all the available articles.

After trying and failing to find Cochrane's paper in the usual places, I returned to the hint. The Internet Archive doesn't have that page under either possible URL, but the directory strongly hints that all of the papers would exist at URLs like `archive/brain.php` or `archive/information-processing.php`, and we can look up all of the URLs the IA has under that directory---how many could there be? [A lot](https://web.archive.org/web/*/http://www.cochrane.org.uk/opinion/archive/*), but only one has the keyword "brain" in it, providing us [the paper itself](/doc/iq/1995-cochrane-biologicallimitstoinformationprocessinginthebrain.html "'Biological limits to information processing in the human brain', Cochrane et al 1995").

If that hadn't worked, there was at least one other version hiding in the IA. When I googled the quoted title "Biological limits to information processing in the human brain", the hits all appeared to be useless citations repeating the original Hofman citation---but for a crucial difference, as they cite a different URL (note the shift to an 'archive.cochrane.org' subdomain rather than the subdirectory `cochrane.org.uk/opinion/archive/`, and change of extension from `.html` to `.php`):

- hit 5:

    > Biological Limits to Information Processing in the Human Brain. Retrieved from: `http://archive.cochrane.org.uk/opinion/archive/articles/brain9a.php`
- hit 7:

    > Biological Limits to Information Processing in the Human Brain. Available online at: `http://archive.cochrane.org.uk/opinion/archive/articles/brain9a.php`; Da Costa ...

Aside from confirming that it was indeed a '.php' extension, that URL gives you [a second copy of the paper in the IA](https://web.archive.org/web/20161201053731/http://archive.cochrane.org.uk/opinion/archive/articles/brain9a.php). Unfortunately, the image links are broken in both versions, and the image subdirectories also seem to be empty in both IA versions, though there's no weird JS image loading badness, so I'd guess that the image links were always broken, at least by 2004. There's no indication it was ever published or mirrored anywhere else, so there's not much you can do about it other than to contact Peter Cochrane (who is still alive and actively publishing although he leaves this particular article off his [publication list](https://petercochrane.com/personal/publications)).

# Connotations

A commenter [who shall remain nameless](https://www.lesswrong.com/posts/oFMywHmJffsCSDNB7/using-evolution-for-marriage-or-sex#comment-AJ5xdSRjq7mwtR7Ea) wrote

> I challenge you to find an example of someone saying "this den of X" where X does not have a negative connotation.

I found a [positive connotation within 5s](https://www.memphisdailynews.com/news/2012/nov/15/this-den-of-grizzlies-players-doesnt-bluff/ "This Den of Grizzlies Players Doesn't Bluff") using my Google hotkey for `"this den of "`, and, curious about further ones, found additional uses of the phrase in regard to dealing with rattlesnakes in Google Books.

# Too Narrow

A failure case study: [The\_Duck](https://www.lesswrong.com/posts/wKWvodoAt3zRhyC4x/rationality-quotes-november-2012#comment-c4yECdhdDHoMvNWEs) looked for but failed to find other uses of a famous Wittgenstein anecdote. His mistake was being *too specific*:

> Yes, clearly my Google-fu is lacking. I think I searched for phrases like "sun went around the Earth," which fails because your quote has "sun went round the Earth."

As discussed in the search tips, when you're formulating a search, you want to balance how many hits you get, aiming for a sweet spot of a few hundred high-quality hits to review---the broader your formulation, the more likely the hits will include your target (if it exists) but the more hits you'll return. In The\_Duck's case, he used an overly-specific search, which would turn up only 2 hits at most; this should have been a hint to loosen the search, such as by dropping quotes or dropping keywords.

In this case, my reasoning would go something like this, laid out explicitly: '"Wittgenstein" is almost guaranteed to be on the same page as any instance of this quote, since the quote is about Wittgenstein; LW, however, doesn't discuss Wittgenstein much, so there won't be many hits in the first place; to find this quote, I only need to narrow down those hits a *little*, and after "Wittgenstein", the most fundamental core word to this quote is "Earth" or "sun", so I'll toss one of them in and... ah, there's the quote!'

If I were searching the general Internet, my reasoning would go more like "'Wittgenstein' will be on, like, a *million* websites; I need to narrow that down a *lot* to hope to find it; so maybe 'Wittgenstein' *and* 'Earth' *and* 'Sun'... nope, nothing on the first page, so toss in `'goes around' OR 'go around'`---ah there it is!"

(Actually, for the general Internet, just `Wittgenstein earth sun` turns up a first page mostly about this anecdote, several of which include all the details one could need.)

# Try It

Someone asked on IRC: "anybody here know that one artist with the really creepy art sytle [sic] that starts with a z?"

I googled: 'that one artist with the really creepy art sytle [sic] that starts with a z'. It was hit #2, [Zdzisław Beksiński](!W).
(DuckDuckGo, incidentally, buries Beksiński several pages in, and I didn't find him in Bing at all.)

# Really, Just Try It

Quanticle asked:

> There's a sci-fi book I'm thinking of, where the protagonist is a scout soldier fighting an endless war against an insectoid species.
> It reads like a cross between _Ender’s Game_ and _Starship Troopers_ (but is not written by John Scalzi or is _The Forever War_) and the main story takes place inside a frame story where two other people are actually "reading" this soldier's memories from his salvaged battlesuit.
> There is a planet called "Golden", where the soldier is allegedly from.
> Does anyone have any idea what I'm talking about?

The search `book about a soldier from the planet golden` immediately turned up [John Steakley's](!W "John Steakley") [_Armor_](!W "Armor (novel)").
(This was showing off a little---_Armor_ is well-regarded and difficult to forget, and I'd read it a long time ago and already knew the answer, pace the hacker koan[^Knight].)

Quanticle noted that "You know, I searched for similar phrases, but I ended up fixating on the soldier's key phrase, where he called his battle-trance "The Machine", and that dragged in lots of irrelevancies."
(A good intuition for search engine use would shy away from using any word or phrase as incredibly generic as "the machine".)

[^Knight]: ["Tom Knight and the Lisp Machine"](http://www.catb.org/jargon/html/koans.html "Some AI Koans") (from the [Jargon File](!W)):

    > A novice was trying to fix a broken [Lisp machine](!W) by turning the power off and on.
    >
    > [Knight](!W "Tom Knight (scientist)"), seeing what the student was doing, spoke sternly: “You cannot fix a machine by just power-cycling it with no understanding of what is going wrong.”
    >
    > Knight turned the machine off and on.

# (Try It!)

FeepingCreature asked, while designing a compiler for a custom language,

> Hey, what was the official name for Lisp's "data and code" thing?

I already knew that it is "[homoiconicity](!W)", but I bet that `official name for Lisp's "data and code" thing` would work if I tried it in Google.
It did.

# Yes, That Works Too

[Grayson81](https://www.reddit.com/r/slatestarcodex/comments/p21d1t/for_gods_sake_google_it/h8ibsff/):

> One thing that’s rather shocking to those of us who used search engines (and even directories like Yahoo before they got the idea of becoming real search engines from Google) is just how good they’ve got at understanding a vague, poorly written or mistaken search.
>
> ...I remember trying to explain how Google works to my mother ten years ago and explaining why “who’s that actress? You know, the one with the eyes. Not [Katy Perry](!W)” isn’t a question that a computer can answer. Now she can Google exactly that and all of the top results are telling her that she’s thinking of [Zooey Deschanel](!W)!

# Comics

[Julia Galef](https://juliagalef.com/) tweeted:

> I read a webcomic ~15 years ago that I've been unable to find since, even with my best google-fu. It involved a robot living a bleak life as a working stiff. At the end he cracked open his "skull" and there was a small dying creature inside. The art style was less cartoony, and more like Moebius, I think? And maybe it was wordless? And, sorry, it wasn't a "webcomic" in the sense of a long-running thing. It was a self-contained story, maybe 15 pages long?

Ultimately rediscovering that

> The comic was called ["Headcase"](/doc/fiction/science-fiction/2004-chivers-headcase.pdf "'Headcase', Chivers 2004") and it was by Sam Chivers.

Unfortunately, no mirrors of it appeared online or on Chivers's current website, and discussions of it mentioned that it was interesting for being an [Adobe Flash](!W) webcomic. Worse still, nothing useful appeared [in the Internet Archive for the original website](https://web.archive.org/web/2020*/http://www.realitytax.com/*)---somehow the IA appeared to have missed any relevant `.swf` files, and 'head'/'case' turned up no relevant looking filenames. It might have been buried in the opaquely-named images, and my usual next step would be to download the IA archives and inspect every image, but in other hits, I found that an obscure comics publisher had published an anthology involving Chivers, and [closer inspection](https://theslingsandarrows.com/prophecy-anthology-volume-1/ "'<em>Prophecy Anthology Volume 1</em>, review', Plowright 2020") confirmed that "Headcase" was in fact published in their (long out of print) 2004 anthology _Prophecies: Volume 1_. (Not a prophetic name inasmuch as there was no volume 2.)

In one of the usual ironies of linkrot, Chivers presumably taking down "Headcase" for print publication in _Prophecy_ may have preserved it, as while I am unable to find any digital copies, the paper version is easily obtained as a used book & scanned at modest cost.

# Beating PDF Passwords

<span id="astronomy">[A physics article](https://cognitivemedium.com/vme)</span> mentioned they had been unable to get [an old 1973 interview](/doc/science/physics/astronomy/1973-drake.pdf "'Life on a Neutron Star: An Interview With Frank Drake', Drake 1973") in a popular magazine; as is usually the case for non-scholarly magazines, after looking thoroughly, I could find no trace of it anywhere (not even in libraries or used-magazine sellers) other than an expensive DVD collection of back issues 1970--2010 still being sold by the publisher. Reasoning that if they had digitized the archives and were even selling it as a DVD collection, they ought to provide subscribers access to them as well, I signed up---they didn't! So I resorted to the DVD, as, worst-case, I should be able to get it running under [WINE](!W "Wine (software)") if nothing else, and can screenshot the interview.

The DVDs turned out to store all the PDFs as encrypted PDFs and the metadata in an ancient opaque database format I'd never heard of. Despite WINE AppDB's claims, the viewing software only partially worked, and I set about attacking the PDFs directly. They used actual encryption, so pdftk couldn't strip the passwording. Given the viewing software, I hypothesized that there was either a single master password or per-PDF passwords stored in the database.

In the hopes of it being a single short master password, I installed [John the Ripper](!W) (JtR) jumbo edition and extracted the hash of a random file to attack: `/snap/john-the-ripper/current/run/pdf2john.pl *.pdf > ~/hash`. (Note: pdf2john is not in the default JtR, and it depends on JtR internal files so you can't easily just copy it out of the Github repo & run it, as I discovered the hard way. You need to install the jumbo edition.) The password hashes of all the PDFs indeed turned out to be the same, so it used a master password. A simple attack with default password-space could be executed as `john-the-ripper ~/hash`. While I waited for all of the DVDs to copy, I saw that JtR was getting something like only a hundred thousand hashes/s on my 16 Threadripper CPU cores, and did not have any success up to 5-character passwords.

If the password wasn't really short, CPU wouldn't be enough. I decided to switch to [Hashcat](!W) to put my 2×1080ti Nvidia GPUs to good use, as they ought to run hundreds of times faster than JtR. (To convert the JtR hash format to Hashcat hash format, you delete the colon-separated filename field at the beginning of each line.) Hashcat uses a [powerful but confusing](https://hashcat.net/wiki/doku.php?id=mask_attack) DSL of specifying the exact password-space, and I made a reasonable guess that if the original programmer was so lazy as to use a single master password, he would also use a simple alphanumeric password (uppercase + lowercase + decimal numbers), and nothing harder to type or read. To specify the PDF hash type and an attack starting at 1-character alphanumeric & increasing, I wound up with the incantation `hashcat -m 10500 ~/hash.cat -w 3 --force -a 3 --increment -1 '?l?u?d' ?1?1?1?1?1?1?1?1?1?1?1`.

Hashcat worked much better and within an hour had bruteforced on the order of 170 billion hashes and up somewhere around 8 characters. This did not succeed either. At this point, another programmer thought it'd be fun to participate and, while reverse-engineering the executable to see how it decrypted PDFs, suggested that the master password was probably hardcoded as a string literal inside the viewer executable. One could just dump all the strings inside it with the CLI utility `strings *.exe > strings.txt`, and then use it as a Hashcat password list. To my chagrin, when I finally got around to trying `cat strings.txt | hashcat -m 10500 ~/hash.cat -w 3`, it finished within 1s.

The password turned out to be `B775tO11dQvu74`. I was right that it was alphanumerical, but at a length of 14 characters, I doubt I would have brute-forced it. (He successfully reverse-engineered it and discovered the viewer had been used for several other magazine archives as well, apparently, and simply switched master passwords to decrypt each one; the other passwords left in the executable were `PbS19LuXd2pTXw`, `1386r8wRrH01`, & `mfU33QQNlAFGI1`.)

I then decrypted the PDF (`for PDF in *.pdf; do pdftk "$PDF" input_pw "B775tO11dQvu74" output foo.pdf && mv foo.pdf "$PDF"; done`), extracted & uploaded the interview, and archived the collection elsewhere.

# Lewontin's Thesis

In [a vituperative review in _Nature_](https://www.nature.com/articles/266283a0.pdf "Caricature of Darwinism") in 1977-03-17, the Harvard-professor [R. C. Lewontin](!W) excoriated [Richard Dawkins's](!W "Richard Dawkins") classic [_The Selfish Gene_](!W) and [sociobiology](!W) in general, giving as an example

> For more than 40 years evolutionary theory has remained free of a naive selectionism, but in recent times there has been a return to the extreme form of the adaptationist program, as evolutionists have rediscovered behavior. Beginning with the undoubted truth that behavior must, like morphology and physiology, be subject to the force of natural selection, the new Panglossians end with the old error that all describable behavior must be the direct product of natural selection. The scientific manifestation of this trend can be seen in every issue of say, _The American Naturalist_, which is permeated by the language, if not the formal apparatus, of game theory, and in the development of the school of 'sociobiology', among whose more extraordinary productions is a recent highly praised dissertation explaining [_fellatio_](!W) and [_cunnilingus_](!W) among the upper middle classes as an adaptive response to constant resources. The popular manifestation of this new caricature of Darwinism reaches its most extreme form in _The Selfish Gene_ by Richard Dawkins.

As is common in book reviews, Lewontin provides no citations, and [2 biologists](https://x.com/DevoEvoMed/status/1478785200964476929) were curious but unable to figure out what Lewontin was referring to despite searching.

The thesis in question is easy to find in under a minute, because the context gives so many hints: Lewontin refers to it as notorious & widely discussed so it will have many substantive citations (if only to attack it); it is 'recent' (and sociobiology was a heated controversy so it is unlikely to be 'recent' in the sense of 'a quiet field of research still mulling over a provocative paper from 2 decades' before, but more like 'within the past 2 or 3 years' & certainly at least 1970--1977), it is a 'dissertation' and so single-authored & almost certainly a PhD thesis by someone who became at least a post-grad researcher (because a master's thesis would be too low-status to be discussed or praised, or singled out for abuse in _Nature_---it would be unclassy for a chaired Harvard professor to attack such a junior grad student's work there like that), and it likely uses the words "fellatio" and "cunnilingus" as technical terms & decorous Latinate scientific censoring.

If we plug into Google Scholar a date-range of 1970--1977 and the simplest possible query `fellatio cunnilingus "evolutionary psychology" OR sociobiology` or `fellatio cunnilingus sociobiology` or `fellatio cunnilingus "evolutionary psychology"`, we see in GS 2 hits (for the former) or among the hits (latter), the immediately-relevant looking ["Human sociobiology: Pair-bonding and resource predictability (effects of social class and race)", Weinrich 1977](/doc/genetics/selection/natural/human/1977-weinrich.pdf "'Human sociobiology: Pair-bonding and resource predictability (effects of social class and race)', Weinrich 1977") and ["Human Reproductive Strategy: I. Environmental Predictability And Reproductive Strategy; Effects Of Social Class And...", Weinrich 1976](/doc/genetics/selection/natural/human/1976-weinrich.pdf "'Human Reproductive Strategy: I. Environmental Predictability And Reproductive Strategy; Effects Of Social Class And Race. II. Homosexuality And Non-Reproduction; Some Evolutionary Models', Weinrich 1976"), both by the same author ([148](https://scholar.google.com/scholar?cites=15430285300114043496&as_sdt=20000005&sciodt=0,21)+[16](https://scholar.google.com/scholar?cites=4719348106040602943&as_sdt=20000005&sciodt=0,21) citations, quite healthy); the single-authorship & `search.proquest.com` domain for the latter immediately tells us that it's a PhD thesis; clicking verifies that the thesis was at Harvard (which gives it both prestige Lewontin loathes & ensures he could easily hear of it); the similarity of titles suggests that the paper is a condensed version of the thesis (reading the paper suggests this isn't entirely true but is more of an update); Weinrich did indeed go on to a long career (at [San Diego University](!W), publishing [up until 2014](https://scholar.google.com/citations?user=qIeHxgkAAAAJ&view_op=list_works&alert_preview_top_rm=2&sortby=pubdate)); there are attacks like [Lande 1987](/doc/genetics/selection/natural/human/1978-lande.pdf "Are Humans Maximizing Reproductive Success? [with Reply]") showing it did not pass without notice; and even the ProQuest preview of the abstract looks consistent with Lewontin's summary (there can't be that many such theses!).

So, we can be sure that Lewontin is referring to Weinrich 1976.

# Teller's "Atom Alphabet"

Nuclear physicist [Edward Teller](!W) wrote a rhyming 'atom alphabet' about the nuclear era, but only a few of the letters like A/B/S are ever quoted. Did he write a *whole* alphabet?

[Tracing citations back](https://www.reddit.com/r/unsong/comments/4pzyvq/edward_tellers_atom_alphabet_1946/) to a _Time_ magazine & [Laura Fermi's](!W) memoir strongly implies that he did not, and only write A/B/F/H/S.
(There has been at least one effort [to write the rest](https://www.reddit.com/r/unsong/comments/zckyg0/completing_edward_tellers_atom_alphabet_with/).)

# Pressley et al 1989

In [a discussion of learning](https://www.patreon.com/posts/reading-and-85345515), [Andy Matuschak](https://andymatuschak.org/) referenced a paper on an [illusion-of-depth](/doc/psychology/cognitive-bias/illusion-of-depth/index) in reading comprehension (related to illusions of learning from using cramming rather than [spaced repetition](/spaced-repetition "‘Spaced Repetition for Efficient Learning’, Gwern 2009")), but mentioned he had been unable to find a copy anywhere to verify it.
The citation for this paper was:

> Pressley, M., Ghatala, E. S., Pirie, J., & Woloshyn, V. E. (1990). ["Being really, really certain you know the main idea doesn't mean you do"](/doc/psychology/cognitive-bias/illusion-of-depth/1990-pressley.pdf). [_National Reading Conference Yearbook_, 39](/doc/psychology/linguistics/1990-zutell-literacytheoryandresearch39thnationalreadingconference.pdf "‘<em>Literacy Theory and Research: Analyses from Multiple Paradigms. Proceedings of the Annual Meeting of the National Reading Conference (39th, Austin, Texas, November 28–December 2, 1989)’, Zutell & McCormick 1990"), 249--256.

I rose to the challenge.

[Standard checks.]{.margin-note} Matuschak is indeed correct that this paper does not show up in any of the usual places, nor does 'yearbook' #39 seem to show up; this nut will not be cracked instantly.
We do not see any encouraging hints if we google the citation (only a sporadic handful of later citations to it, which are sporadic enough to suggest that they too [are citing papers they have not read](/leprechaun#citogenesis-how-often-do-researchers-not-read-the-papers-they-cite "‘Leprechaun Hunting & Citogenesis § Citogenesis: How Often Do Researchers Not Read The Papers They Cite?’, Gwern 2014") & that this will be hard to find).
University & ProQuest databases turn up nothing for either.

This begins to look anomalous, so I broadened the search in Google.
Here I stumbled across several of the yearbooks hosted at what looks like the National Reading Conference's website; a targeted `site:` search, alas, fails to turn up anything useful.
They may have scanned some later yearbooks, but apparently not the 1989 one...?
Unfortunately, a dead end.

[Barkless dogs.]{.margin-note} So we turn to book sources, like used-book search engines.
We can find many of these yearbooks used at reasonable prices, but *not* #39---not a trace of it!
This is odd.
Being the 39^th^ yearbook, with the others often available, would imply that it is available too: such serial publications don't usually vary that much from year to year---if the ones before & after it are easy to get, it should be too.
What one notices is that the titles don't look anything like "_National Reading Conference Yearbook_, 39": this citation must be wrong, that's not how they were titled!
With this in mind, we can search for a used copy to buy & scan, but this would be premature to do: now we have explained the prior absence of hits, and need to redo our searches; we thought there was no scan online before, but we know that was misleading so it may exist after all.

[Alternate titles.]{.margin-note} Knowing this, we can search more broadly in Google, and skimming search results, look what we find!
The PDF snippet reveals that our quarry, "Proceedings of the Annual Meeting of The National Reading Conference (39^th^...)" has been hidden behind the long uncited title "_Literacy Theory and Research: Analyses from Multiple Paradigms_".
Well, no wonder you can't find it normally, and also (disappointingly but unsurprisingly), no wonder everyone copies the same incorrect citation.

![Screenshot of key Google search hit, revealing the Academia.edu PDF copy of ERIC scan of _National Reading Conference Yearbook_ #39.](/doc/technology/google/2023-07-08-gwern-google-searchcasestudy-pressleyetal1990academiaeduhit.png)

[Alternate paths.]{.margin-note} Downloading it, Pressley et al 1989 turns out to be buried on pg256 of this PDF; now that we know what to look for, this book turns out to have been easily findable after all---we can readily find the original non-Academia.edu PDF on our old friend [ERIC](!W "Education Resources Information Center")^[ERIC is one of the good websites for fulltext & scans. It's always a harbor in the storms of the Internet, with irreplaceable scans, especially of older gray literature like pre-WWW government publications or preprints.] and can find other yearbooks easily.
We can also double-check other strategies: for example, if we had known the full names of the authors rather than the abbreviated ones in the citation, and we had googled something like "Michael Pressley, Elizabeth Ghatala, Jennifer Pirie, Vera E. Woloshyn", that would have matched the indexed fulltext PDFs immediately.
(Since you can often find the full names of authors even if the citation abbreviates them, this is a good tactic to know.)

Thus, in this instance, it's crucial to remember that citations can be inaccurate and one must try variations.
Over-fixating on the book title can hamper efforts to locate the article, which was, in reality, merely a click away.

# Oliver Heaviside

<div class="epigraph">
> Mathematics is of two kinds, Rigorous and Physical. The former is Narrow: the latter Bold and Broad. To have to stop to formulate rigorous demonstrations would put a stop to most physico-mathematical inquiries. Am I to refuse to eat because I do not fully understand the mechanism of digestion?
>
> apocryphal (mishmash of [Oliver Heaviside](!W) & [H. S. Carslaw](/doc/math/1928-carslaw.pdf "‘Operational Methods in Mathematical Physics’, Carslaw 1928"){.backlink-not})
</div>

There is a witty pragmatic quote about the philosophy of science attributed to the hardscrabble physicist Oliver Heaviside; it is a wise one, as many people will refuse to acknowledge some scientific discovery for lack of a completely rigorous & explicated mechanism, even though this would lead to such absurdities as denying the existence of gravity or selective breeding.
I wished to use it in such a context, so I went looking for a proper source.

My first good hit was to [Edge 1983](/doc/science/physics/1983-edge.pdf "Oliver Heaviside (1850–1927)—Physical Mathematician"), which is an interesting biographical profile of Heaviside, and opens with that as epigraph.
Despite this promising hit, Edge provides no reference for that.
([Lutzen 1979](/doc/math/1979-lutzen.pdf "‘Heaviside's Operational Calculus and the Attempts to Rigorize It’, Lützen 1979") was no good either.)
Further searching in Google Books turns up hits dead-ending in the 1940s without citations---concerningly, about half a century after Heaviside *should* have written it in his prime in the 1890s.

Since all Heaviside writings should be long since in the public domain & digitized, I switched to targeting Internet Archive for book scans.

As usual, I start with what seems to be the key of the quote, the part about eating dinner.
There I find *part* of the key sentence in Heaviside's book [_Electromagnetic Theory_ I](https://archive.org/details/electromagnetict02heavrich/page/8/mode/2up ) (emphasis added):

> Nor is the matter an unpractical one. I suppose all workers in mathematical physics have noticed how the mathematics seems made for the physics, the latter suggesting the former, and that practical ways of working arise naturally. This is really the case with resistance operators. It is a fact that their use frequently effects great simplifications, and the avoidance of complicated evaluations of definite integrals. But then the rigorous logic of the matter is not plain! Well, what of that? **Shall I refuse my dinner because I do not fully understand the process of digestion?** No, not if I am satisfied with the result. Now a physicist may in like manner employ unrigorous processes with satisfaction and usefulness if he, by the application of tests, satisfies himself of the accuracy of his results. At the same time he may be fully aware of his want of infallibility, and that his investigations are largely of an experimental character, and may be repellent to unsympathetically constituted mathematicians accustomed to a different kind of work.

You will notice that aside from a troubling modification of the word 'dinner' → 'eat', the rest of the paragraph bears no resemblance to the quote.
It's not truncated, or eliding parts---it's just completely unlike the quote.
Nor is it to be found on the previous page, or the next page, or anywhere in the book.
You can search all of the phrases and check them one by one, and you will not find it.

OK... Bizarre... But perhaps there is a different version in another book?
We all repeat ourselves, and edit later publications, and whatnot.
If we persevere on IA, we will find a striking section header & later sentence from Heaviside's subsequent [_Electromagnetic Theory_ II](https://archive.org/details/electromagnetict02heavrich/page/4/mode/2up) (emphasis added to second quote but not section header):

> **Rigorous Mathematics is Narrow, Physical Mathematics Bold And Broad** § 224
>
> Now, mathematics being fundamentally an experimental science, like any other, it is clear that the Science of Nature might be studied as a whole, the properties of space along with the properties of the matter found moving about therein. This would be very comprehensive, but I do not suppose that it would be generally practicable, though possibly the best course for a large-minded man. Nevertheless, it is greatly to the advantage of a student of physics that he should pick up his mathematics along with his physics, if he can. For then the one will fit the other. This is the natural way, pursued by the creators of analysis. If the student does not pick up so much logical mathematics of a formal kind (commonsense logic is inherited and experiential, as the mind and its ways have grown to harmonise with external Nature), he will, at any rate, get on in a manner suitable for progress in his physical studies. **To have to stop to formulate rigorous demonstrations would put a stop to most physico-mathematical inquiries.** There is no end to the subtleties involved in rigorous demonstrations, especially, of course, when you go off the beaten track. And the most rigorous demonstration may be found later to contain some flaw, so that exceptions and reservations have to be added. Now, in working out physical problems there should be, in the first place, no pretence of rigorous formalism. The physics will guide the physicist along somehow to useful and important results, by the constant union of physical and geometrical or analytical ideas. The practice of eliminating the physics by reducing a problem to a purely mathematical exercise should be avoided as much as possible. The physics should be carried on right through, to give life and reality to the problem, and to obtain the great assistance which the physics gives to the mathematics. This cannot always be done, especially in details involving much calculation, but the general principle should be carried out as much as possible, with particular attention to dynamical ideas. No mathematical purist could ever do the work involved in Maxwell's treatise. He might have all the mathematics, and much more, but it would be to no purpose, as he could not put it together without the physical guidance. This is in no way to his discredit, but only illustrates different ways of thought.

This is not a republication, but a separate book.
This is now quite bizarre: somehow, someone has hybridized two Heaviside quotes from (at least) two different books.
Further, the quotes have been tampered with: the sentiment about narrow/rigorous/physical/bold/broad is there in the original, certainly, but not as simplified in the apocryphal quote.
So we are missing a step: someone reformulated it.

We now know what to search for, and to be wary of shenanigans.
Once we do, it is but a google away to find Carslaw 1929 (["Operational Methods in Mathematical Physics"](/doc/math/1928-carslaw.pdf){#carslaw-2}) remarking (emphasis added):

> §2. Heaviside himself hardly claimed that he had 'proved' his operational method of solving these [partial differential equations](!W) to be valid. With him \[cf. _loc_. _cit_., p. 4. \[_Electromagnetic Theory_, by Oliver Heaviside, vol. 2, p. 13, 1899.\] \] **mathematics was of two kinds: _Rigorous_ and _Physical_. The former was _Narrow_: the latter _Bold_ and _Broad_.** And the thing that mattered was that the Bold and Broad Mathematics got the results. "To have to stop to formulate rigorous demonstrations would put a stop to most physico-mathematical enquiries." Only the purist had to be sure of the validity of the processes employed.

And that completes the search: the quote is apocryphal, and is the fusion of a sentence in one Heaviside book *and* a section header & sentence in another Heaviside book *as* paraphrased by an obscure physicist 30 years later!

My best guess for how this happened is that someone read the Carslaw 1929 paper, and the first Heaviside book (since the 'dinner' core is not in Carslaw), then about a decade later, recalled a streamlined version (with the tell-tale simplification), which then took off on its own.

# Electricity Elasticity

In February 2025, Patrick McKenzie posted a podcast transcript on power grid economics, ["Power plays: grid economics and engineering, with Travis Dauwalter"](https://www.complexsystemspodcast.com/episodes/electricity-grids-travis-dauwalter/), with an energy consultant.
One part I found interesting was about the non-response of Americans to electricity prices, having [a low elasticity](https://en.wikipedia.org/wiki/Price_elasticity_of_demand) in a Chicago randomized experiment:

> **Travis Dauwalter**: Yeah, there's this really great study that some academics pursued—I can't remember the year—they essentially used a light in and around the Chicago area. There was a light that you could plug into a kitchen outlet, it tended to be the kitchen, just some hub in your home. The light would be red if power prices were quite high and they wanted to encourage you to reduce your load, and green when power prices were fine.
>
> They were hoping to observe a lot of response to the light as a signal for what the market prices were, but it turns out that we're not very sensitive to the price of electricity. We just really like it as a modern amenity. If the price is a little high—you mentioned you can save 3¢ times whatever your consumption is—it just ends up not being that much money and most people are willing to pay the premium.

This makes sense to me, considering how cheap LED lights are, but of course, many such claims do not hold up to scrutiny, so I wanted to check, but like most podcasts, citations are not provided.

Well, that was fine.
My intuition told me this should be easy to find, because the internal evidence here is promising.
The time window & key terms we are looking for are:

#. **When**: this study was probably some time ago, given the vagueness of Travis's description of when it was done, but not *too* long ago, because it relies on some semi-sophisticated electronics & market mechanisms and Travis doesn't describe it as some amazing pre-Internet anomaly from the Stone Age, nor is Travis himself all *that* old.

    So we would roughly ballpark this as 1990--2020.
#. **Where**: "Chicago" is not a 'default' city, nor one easily confused with others, misspelled, or called by another name. Further, McKenzie had recently moved to Chicago, as he has mentioned online in many places, and his guest likely knew that, and that would prime him to remember this study.

    So this study almost certainly did involve Chicago, which alone ought to make this nearly trivial to search for. There just can't be that many studies involving Chicago!
#. **What**:

    - the "*red light*" has the ring of truth:

        it is a concrete, logical, memorable detail, which would hardly ever show up in unrelated research.
    - the study had to have been on *electricity*.

        It could not possibly have been another utility like sewer, policing, natural gas, Internet bandwidth, or water (eg. because water is so cheap, a bulk commodity hard to measure, and one whose price doesn't change over time).
        There is no other utility it could be.

So my instinctive first search is in Google Scholar (because it is described as a formal study, and GS will narrow the search down a lot), for the keywords (but not phrases or quotations, because we have none here) [`Chicago power saving red light electricity price`](https://scholar.google.com/scholar?q=chicago%20power%20saving%20red%20light%20electricity%20price) (note no quotes).
If this didn't yield anything promising, I would try removing "price", "power saving", & "electricity" in that order; only if that failed would I fall back to a general Google search, because I would expect that to bring in many unrelated hits.
I would also try varying description of the light: maybe it turned red but writeups didn't mention that, and simply describe it as a 'color orb' (assuming that the color-coding would be obvious) or a 'glowing sphere', perhaps.
(Particularly in the post-BERT Google Search era, words in a search query are interpreted broadly.)

![GS search results for `Chicago power saving red light electricity price`, showing the goal as #2 and visibly relevant snippets.](/doc/technology/google/2025-02-12-gwern-googlescholar-pricelightsearchquery.jpg)

However, the second GS hit (after a likely irrelevant 1991 book) shows a snippet for the 2009 preprint of the Allcott 2011 paper ["Rethinking real-time electricity pricing"](/doc/economics/mechanism-design/2011-allcott.pdf) about a mid-2000s randomized experiment, which reads:

> … Specifically, I examine the *Energy*-Smart *Pricing* Plan, which has operated in *Chicago* since … information technologies that include, among other things, household *energy management* …

This sounds promising, as it is the right time & place, and the title is consistent with Travis's claim about the price information being unexpectedly ineffective at reducing demand, which would indeed provoke a "rethinking".
Clicking through to the preprint, and doing `C-f` to search the text for `" red"` (because 'red' is a suffix of too many words and would take too long), we soon find on page 9 that:

> In an effort to increase the ease with which households could observe, and thus respond to, hourly price fluctuations, program managers introduced a device called the PriceLight. This is a small plastic globe that changes colors in real time on a continuous spectrum from blue to red, indicating low to high electricity prices. CNT offered PriceLights to all ESPP participants in 2006. Of the 223 households that submitted requests, 47 were randomly selected to receive the device.

Bingo. This device exactly matches the description.

The results discussed in the abstract does not exactly match the description: Travis describes it as insensitivity, while the paper abstract emphasizes that there was *some* sensitivity but it was in such a way as to make it largely useless for any kind of society-wide rollout.
However, I'd say broadly that Travis is correct that the upshot is that "we're not very sensitive to the price of electricity".
Further, we can see GS notes that this paper has been "Cited by 667" other papers, which for an arcane economics paper like this, is a remarkably high citation (usually you'd see <100), and so we can be confident that we have found the device name ("PriceLight"), and the canonical writeup Travis is relying on, and that we can rely on the takeaway that "ordinary Chicago households in the 2000s were price-inelastic in electricity use".

Because this was so easy---the first look at the first search immediately revealed it---I was curious if OpenAI's new ["Deep Research"](https://openai.com/index/introducing-deep-research/ "‘Introducing Deep Research: An agent that uses reasoning to synthesize large amounts of online information and complete multi-step research tasks for you. Available to Pro users today, Plus and Team next’, OpenAI 2025") tool [could make heads or tails of it](https://chatgpt.com/share/67a94e77-2844-8006-b798-9fb2b11724d7).
I pasted in the same two paragraphs as above with "Please find this study".
DR responded (as it often asks clarifying questions before doing its slow expensive autonomous research):

> Do you have any additional details on this study, such as the names of the researchers, the institution that conducted it, or the approximate year of publication? If not, I will conduct a broad search based on the description you provided.

which I shut down:

> I do not. That's all I know, sorry. You can look up the original podcast if you think it'll help, but it probably won't.

DR quickly found the PriceLight, noting it turned red, and highlighted Allcott 2011:

> ...Economist Hunt Allcott later published an academic analysis of the same experiment in _Resource and Energy Economics_ (2011)...

It also provided several relevant links for further reading, and gave what looks like a reasonable overview.
So I consider this a successful use of DR.

I sent the links to McKenzie on Twitter so he could add them to the transcript & save everyone else the effort.
I was surprised when he responded that he had made an effort to find the PriceLight citation himself, but [he gave up after 30 minutes](https://x.com/patio11/status/1889245397144793314); it seems likely he picked the wrong key terms, and so wound up drowning in a morass of hits related to Chicago or electricity pricing, but not hitting the crucial intersection like I & DR did.

So, DR could've saved McKenzie quite a bit of time in looking for this citation.
(And apparently it was difficult enough to be worth writing this case-study to explain how I found it.)

# Pikachu T-Shirt

(2026-02-21) Papercuts of incompetence: it is frustrating just how badly designed and glitchy many things are, even when it is about trying to throw money at someone.
A depressing exercise is to, once in a while, just write down everything that goes wrong in trying to do some ordinary computer task (in the spirit of ["they may never tell you"](https://pointersgonewild.com/2019/11/02/they-might-never-tell-you-its-broken/ "‘They Might Never Tell You It’s Broken’, Chevalier-Boisvert 2019")).
Today's example: [Uniqlo's](!W) "[Pikachu](!W) is recharging" t-shirt, revealing a fractal of bugginess and incompetence, requiring a surprising amount of searching…

#. I see an ad in my Google news feed on my smartphone, which shows me the image of [a Pikachu recharging t-shirt](https://www.uniqlo.com/us/en/products/E474426-000/00).
Cute! Maybe I could get it for myself or family.

    So I click on it and go to the Uniqlo... homepage? What?
    Why am I not going to the item they just sold me on?
    I *want* to give you my money, why are you hiding it from me?
#. OK, they are incompetent at running ads, and clicking on one of their ads for something does not even take you to the thing it's an ad for. But I can see a link while scrolling to their _Pokemon_ collection.

    Problem solved? No. It's not there.
#. What‽ Alright, I switch back to laptop. I search on the Pokemon page for "Pikachu" and "recharging".

    No, I'm not missing it, it really doesn't seem to be there.
#. OK, I'll do a site search. I click on the magnifying glass and search "recharging".

    Zero hits! How is this even possible?
    Does the title not mention the most important thing about it?
    Do they not have any product descriptions?
    Do their images have no alt text?
    Does not a single review even mention or allude to one of two visual elements?

    It will turn out that there is in fact a collapsed, hidden description on the item page... which is simultaneously unhelpful because it applies to all the products, omitting the "adorable phrases" you might be searching for, and is not indexed anyway:

    > <div class="collapse">
    > Pokémon
    >
    > Pikachu and other Pokémon friends have once again gathered for UT. This collection features delightful sketch style designs which capture both leisurely and energetic scenes, along with adorable phrases that perfectly represent each Pokémon. You'll surely find a style just your type.
    >
    > ©2025 Pokémon. \
    > ©1995--2025 Nintendo/[Creatures Inc.](!W)/[GAME FREAK](!W) inc.
    > </div>
#. OK, I search "Pikachu" even though that will doubtless bring up too many hits and be tedious. There is one hit…

    It is the Pokemon page which, I have already observed, includes no Pikachu recharging.
#. Fine, surely *Google* can find it?

    "Uniqlo Pikachu" pulls up the same useless pages, various social media discussions of unrelated merchandise, and, helpfully, places I can buy the t-shirt… on [eBay](!W), [Depop](!W), [Poshmark](!W), etc.

    Apparently random eBay scalpers have better SEO than a [$22]($2026)b-revenue e-tailer.
#. After scrolling all the way to the bottom and checking the second set of image results, I finally find an image link which seems valid… to the UK Uniqlo site.
   But now I know the official title of the "Pikachu recharging t-shirt"!

   The official title is: "Pokémon Sketch UT Graphic T-Shirt".

   Thanks, that's very helpful.
   (Notably, this title as a Google search also pulls up mostly everyone-but-Uniqlo, so it's not an issue of lacking the right search query.)
#. But I can reverse-engineer the UK URL, to find the American URL, and finally see the right listing.

    And it is *sold out* in every size.

    So, they have been advertising to me something I cannot even buy from them!
#. On a side note, we can appreciate how bad the item page is.

    !["Pikachu is recharging" Uniqlo t-shirt item page screenshot](/doc/design/typography/rubrication/2026-02-24-uniqlo-pikachuisrechargingtshirt-itempage.jpg)

    It is hard to figure out that the item is sold out because the layout is confusing and the "Out of stock" text is some of the smallest, most faded-out text on the page, while things like "Sale" are highlighted [in bright red](/red "‘Rubrication Design Examples’, Gwern 2019")---what does it mean to have a "sale" on a sold-out item that one cannot pre-purchase, one wonders?

    The "store locator" is another false hope, as it is broken, not that I expect it to still be in stock in any physical stores given the scalper prices.
    (I stopped at a Uniqlo store I was walking by about a month later, and did not find any Pokemon merchandise.)

    The page manages to waste most of its screen real estate on two duplicate photographs of the t-shirt and then a near-duplicate zoom-in.
    It wastes some more real estate on a black bar for sales spam, a social media 'share' icon (to, bizarrely, only Twitter and Facebook), a bunch of utility icons irrelevant to an anonymous signed-out viewer, a broken store-availability widget, and a size popup alert, while hiding most of the details in collapses.

    While this is not great, this item page's design is the rare bright spot in my litany: it could be a *lot* worse!
    There's no GDPR or cookies modal popup covering the page, there's no newsletter nag...
    It manages to be one of the few e-commerce pages not substantially improved by using adblock.
#. OK… Well, I'll sign up for notifications about a restock.
   (Which they make no promises about, bizarrely, despite a stockout being, one would think, a sign it should be guaranteed to be restocked; and which come with a peculiar restriction to 8AM---so it might be sold out even after the alert).

    I have a Uniqlo account, no big deal.

    It will not let me log into my old account. I didn't change it or anything, but I am locked out. I have to reset my password via my email account.
#. I reset my password, and go to get notified. (I reset it to the old password, which works, and raises questions about what is going on in the Uniqlo backend.)

    I am blocked because I have not 'logged in', despite resetting the password just now.
#. I log in, and I go to get notified.

    I am blocked because I have not "verified my email account"…
    The email account I just, y'know, used to reset the password.
    And also the same email account that I used for several orders years before.
    (I love the Uniqlo [Airism boxer underwear](https://www.uniqlo.com/us/en/men/innerwear/airism/boxer-briefs).)
#. I verify my email account. I go to get notified.

    The popup is confusing (do I need an app…? *absolutely not*).
    I do it 2--3×, but there is no confirmation that it works, and nothing about the page or website changes, and I still see "Notify Me" as a button.
    (Note that when I "add it to my wishlist", it does change state to confirm that it was correctly added, to a little red heart.)
#. I don't know if I will be notified, and I don't want it enough to spend [$40]($2026) on eBay for it.

    I give up and instead of Uniqlo getting my money, they get nothing, and I write a rant instead.

Uniqlo ad, *you had one job*: show me something I could buy.
And you have failed at it in a remarkable number of ways.

I sacrificed all my bandwidth and screen real estate and privacy to a maximalist invasive adtech ecosystem, and then you can't even do the simplest thing right!

Now, did I report any of this to Uniqlo?
I often report small bugs to people, to help out and to see what happens.
In this case, I did not bother.

It is not possible for people to fail to notice problems like "items do not have descriptions" or "titles are meaningless", nor that "I cannot search for a specific item, even in Google".
These are problems that any functional e-commerce website solved in the 1990s, back when most [software was terrible](/improvement#microsoft); what is striking is that this is my experience in February 2026.
These problems are *so* obvious to anyone using the Uniqlo web site that their persistence can only reflect deep organizational pathology, like a contempt for design.

Hence, there is no point trying to contact Uniqlo with my narrative of woe; it can, instead, serve as a cautionary example and a demonstration of how search skills are still useful.
