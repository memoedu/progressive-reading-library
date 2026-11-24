### The New Development Economics

If you, like me, think that randomized controlled trials of international-development interventions are the bee’s knees, you might be interested in reading [_The New Development Economics_](https://research.hks.harvard.edu/publications/getFile.aspx?Id=317) by Dani Rodrik (long, but reasonably skimmable for the good parts). Rodrik makes a number of great points, including most saliently that the distinction (in evidential quality) between randomized and observational studies is smaller than the “randomistas” tend to think:

> \[W\]hat is striking about the public discussion that followed the dissemination of the Cohen-Dupas paper \[finding that free bednet distribution works better in Kenya\] is the wealth of reasons opponents of free distribution could offer as to why these results could not be generalized. The debate on free distribution versus cost-sharing was hardly settled. Randomized evaluation did _not_ yield hard evidence when it comes to the actual policy questions of interest. This should not have been a surprise: the only truly hard evidence that randomized evaluations typically generate relates to questions that are so narrowly limited in scope and application that they are in themselves uninteresting. The “hard evidence” from the randomized evaluation has to be supplemented with lots of soft evidence before it becomes usable.

But more broadly, Rodrik makes the great point that “macro” or big-picture, policy-focused development economics and “micro” RCT-driven interventions are closer than people realize. This has implications for the epistemology of micro-development—after reading, I’ve updated somewhat against the solidity of RCT evidence—but also for the policy strategy of macro-development. Rodrik has an interesting vision for a “new policy mindset” with some great examples at the end. (h/t Will Poff-Webster)

### Art, computerized

If all these self-important statistics debates leave you cold, you might be cheered up by the more fun side of stats: somebody has [trained a deep neural net to describe artworks](http://noviceartblogger.tumblr.com/). It’s amusingly good at calling abstract art like it sees it:

> A white bow tie on top of a purple suit on a beach or I think a person that has been placed on a surfboard. Reminds me of a surfboard, painted as an advertisement.

### Also under “religious wars…”

A randomized trial of 40 researchers [reports that typesetting goes faster in Word than LaTeX, with fewer errors](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0115069), though the LaTeX users reported higher satisfaction:

> We show that LaTeX users were slower than Word users, wrote less text in the same amount of time, and produced more typesetting, orthographical, grammatical, and formatting errors. On most measures, expert LaTeX users performed even worse than novice Word users. LaTeX users, however, more often report enjoying using their respective software.

I’m going to [defy the data](http://lesswrong.com/lw/ig/i_defy_the_data/) on this one—both parts of it, in fact. (h/t [Jess Riedel](http://blog.jessriedel.com/2015/01/28/links-for-january-2015/), whose [call for a LaTeX replacement](http://blog.jessriedel.com/2014/11/12/zotero-is-great-tex-should-be-better/) I sympathize with!)

### Aristotle

Speaking of defying the data! Aristotle is notorious for beliving that men have more teeth than women, the implication being that he hated science and didn’t believe in trying to falsify his beliefs.

Turns out [that’s basically not true](https://scientiasalon.wordpress.com/2014/10/03/rescuing-aristotle/)—Aristotle actually had a quite scientific outlook, but was just misinformed about the data in this case, possibly due to the fact that lots of people had missing teeth in ancient Greece. Article includes many more debunkings of other “Aristotle facts” and some cool intellectual history.

### The sand trade

Maybe I should take a page from the Aristotle-haters and check this evidence myself, but in case you didn’t know, the “international sand trade” [is a thing](http://www.harvarddesignmagazine.org/issues/39/built-on-sand-singapore-and-the-new-state-of-risk). And it’s apparently pretty geopolitically important:

> With the rise of sand trading, the nation-state has entered a dangerously fluid phase. With the coastal earthworks that are under way throughout Southeast Asia and the Middle East—a series of reclamations so large that they nearly encroach on sovereign borders—territory has acquired an unprecedented liquidity.

The legal sand trade is estimated at 70 _billion_ dollars per year. Who knew?

### American Fuzzy Lop

If that’s just not enough surprise for one day, maybe you should run [American Fuzzy Lop](http://lcamtuf.coredump.cx/afl/) on your code.

`afl-fuzz` (as it’s called) is a **fuzz tester**, a tool for generating random inputs to find bugs in code. It uses a couple cool heuristic techniques for guiding its search that end up making it seem much “smarter” than the sum of its parts—for instance, when given the [`libjpeg`](http://libjpeg.sourceforge.net/) binary as a black box to work on, it relatively quickly figured out [how to make a valid JPEG from scratch](http://lcamtuf.blogspot.com/2014/11/pulling-jpegs-out-of-thin-air.html). You can read about the clever combination of heuristics it uses, like branch-coverage checking and genetic recombination of test inputs, in a [whitepaper](http://lcamtuf.coredump.cx/afl/technical_details.txt). (h/t [Dan Luu](http://danluu.com/))