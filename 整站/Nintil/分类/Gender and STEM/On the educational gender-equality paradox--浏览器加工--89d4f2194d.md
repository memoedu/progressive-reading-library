It has been argued that in developed countries, or more concretely, in countries with higher gender equality, women are less represented in the STEM fields. Here I look at this claim and its veracity. This post was prompted by the comments of an anonymous reader, whom I thank for his thoughtful discussion of the issue at hand.

## Addendum

Note (2019-07-01): @rubenarslan sent me some good points about this post; I am noting down here the key points.

The correlation is probably not as strong, and the CIs around the regression line are wider than they seem. The countries in the plot are not iid (they have some common shared history). Also (as should be obvious from the plots) a linear model with a single predictor is not the best fit for this; it may well be that there is a correlation that then tapers off; if we only took the countries that rank above a SIGI of -0.3, we would observe a far weaker correlation, perhaps 0.1, or 0.2, not 0.5.

Next, the way 'STEM' has been measured may vary between countries, I mentioned in the first version of this post that for example there might be intra-STEM variation that STEM would not capture, but this is worse: What if what UNESCO has measured as STEM differs by country? That would also introduce some bias; which would be corrected by looking at unique degrees (e.g. just computer science). I did this as best as I could a while ago and it is still true. The gender index I used, SIGI is what I regard as the best index for measuring what I think intuitively is the best definition of gender equality. For example, I don't regard there being different average pay for men and women as gender inequality, but some indices do. (However, those differences may come about because of gender inequality, and it is that root cause I want to look at). Gender indices correlate with each other; SIGI and GGGI correlate r~0.7. I expect that the correlation will still remain no matter what measure we use though.

In sum, when plotting both things together, I think there still be a correlation, but we should be more uncertain about the magnitude, and probably assume it's lower than it seems (R2 may well be 0.2 rather than 0.5)

The main topic of this post was to look at correlation, and what the correct metric to put in the y axis is (% females in STEM, or controlling somehow for % females in university). But looking at causality is also important.

Regarding causality, I want to emphasise that the causal story discussed in the post is if anything suggestive by not definitive: It is coherent with the data, but so could be others. Equality is most likely not doing the causal work in the correlation; equality is strongly correlated to GDP and many other variables that could be potential causes. It is premature at this stage to say more than "Something related to economic development changes the gender composition in STEM".

Now, the post itself: 

Note that here we assume and take for granted that the worldwide under-representation of women in STEM can be explained by a mixture of preferences and different distributions of skills by gender, without having to resort to a large role for discrimination - [as I've argued](https://nintil.com/categories/gender-and-stem/)\-. This post is about why there are differences between countries, instead of there being a constant gender gap.

The starting point here is [Stoet and Geary (2018),](https://journals.sagepub.com/doi/abs/10.1177/0956797617741719) a paper made famous by its Figure 3(b), reproduced here:

![](https://artir.files.wordpress.com/2019/06/stoet.png)

The Y axis is the  Global Gender Gap Index, an attempt of measuring gender equality across countries; the X-axis is not what it seems to be though.

The anonymous reader brought a series of critiques of the plot above. One is that the gender equality index they are using, the GGGI, contains in its construction somewhat correlated values to the variables of interest; namely it includes female university enrolment; although this is a subcomponent of a subindex of GGGI, and so it shouldn't completely bias the analysis. It is also unclear that a few of the components of the index track social norms regarding equality; some of them may reflect the very professional choices that we are studying here.

Solution: Use a better gender equality index like SIGI. SIGI looks at [formal and informal laws, social norms, and practices](https://www.genderindex.org/). This is the same index I used for my series of post on this same topic, a while ago.

A second critique may be that the authors of the paper didn't used what the Unesco provides as  a STEM variable, but instead summed the students in the fields classified as "Natural sciences, mathematics, statistics, information and communication sciences, engineering, manufacturing, and construction". This may be a minor critique as these sets of disciplines vaguely sound STEMish.

The most substantial point of disagreement was the measurement of Women among STEM graduates. It may seem like the plot above is just number of women in STEM/total students in STEM, but apparently it is not so; and it is not well explained in the paper.

As an example, the values for the variable "Percentage of female enrolment by field of study: tertiary education, for STEM", from the Unesco dataset [they use](http://data.uis.unesco.org/#), for Finland never goes below 27%. Yet, in the plot it seems that Finland has around 20% female in STEM!

If we go to the [supplementary materials](https://journals.sagepub.com/doi/suppl/10.1177/0956797617741719/suppl_file/Supplemental_Material.pdf), and check Finland (column 3) we find that 20%. Similarly in Albania, we get from UNESCO 50% of women in STEM; but the equivalent figure from the paper is 38.6%. This may be because they are not using the real variable for STEM that UNESCO provides. But my anonymous reader contacted the authors and intriguingly they replied that the plotted variable is

> not “percentage of STEM graduates who are women” but “percentage of STEM graduates who are women, weighted by overall female graduation rate”

How exactly this weighting was done remains a mystery. At this stage, we should plot the data itself. The raw correlation itself looks like this:

![download](https://artir.files.wordpress.com/2019/06/download.png)

This is not quite Stoet-Geary's figure, but my own, using the entire sample (I take all countries with data available after 2012, and take the mean value for % female in STEM), and I use SIGI instead of GGGI. There is some correlation but it doesn't seem as striking. Note that there are two ceiling effects at play here; % female in STEM doesn't go over 100%, and SIGI doesn't go above 0. Okay, so what should we do? We should perhaps study the 4 variables that determine the % of women in STEM:

-   Male university attendance rate
-   Female university attendance rate
-   % of male students who chooses STEM
-   % of female students who chooses STEM

It could be, for example, that  in more gender-equal countries less men are interested in STEM. Or it could be that less men go to university. A change in any of the variables above could change the % of women in STEM. We should try to find metrics that isolate the changes we are interested in.

If we make them into ratios we can study how they change. For example the ratio of % Female students in STEM/% Female students not in STEM gives us a measure of the relative preferences of women regarding field of studies, and this metric is immune to the female university attendance rate; if the relative preferences stayed constant there would be no correlation. Do women tend to prefer less STEM in more developed countries? Yes they do:

![download (1)](https://artir.files.wordpress.com/2019/06/download-1.png)

So we can reproduce the key conclusion of the paper with better methods: A better parametrization of the "female in STEM" variable, a better gender equality index, and the whole sample in the dataset. The plot looks the opposite for men: In more gender-equal countries, more men choose STEM professions;

![download (6)](https://artir.files.wordpress.com/2019/06/download-6.png)

In that sense, the effect is broadly symmetric for both genders. According to my anonymous reader, in his own re-analysis of the original data, if one looks at GGGI vs % female in STEM, one gets basically no correlation,

![sg-basic](https://artir.files.wordpress.com/2019/06/sg-basic.png)

In my case, I find a correlation coefficient of 0.25, most likely due to a better gender equality index. But importantly, the most striking results can only be attained after one applies corrections for % female in university, regardless of the equality index used.

Thus, if we ask: "In more gender-equal countries, are there less women in STEM?" The answer ranges from "Not really" to "Yes, there is a small correlation of equality->less women in STEM". But we should ask the full range of questions: "In more gender-equal countries, is the % of women in university higher?" And the answer is a very obvious yes:

![download (7)](https://artir.files.wordpress.com/2019/06/download-7.png)

And finally, the question I think is behind the study, and at the core of the debate the study is embedded in: "In more gender-equal countries, do women prefer to study less STEM careers and study non-STEM instead?" And the answer here is a unequivocal yes, they do. (The reverse is true for me, although the effect is not as strong). This is a mere look at correlation; we'll get into causation in a moment.

## It's not just them

The paper points to other research in the area looking at a similar question that sometimes finds null results; however they are also able to explain why this might be the case:

> Our current findings agree with those of previous studies in that sex differences in mathematics and science performance vary strongly between countries, although we also believe that the link between measures of gender equality and these educational gaps (e.g., as demonstrated by Else-Quest, Hyde, & Linn, 2010; Guiso, Monte, Sapienza, & Zingales, 2008; Hyde & Mertz, 2009; Reilly, 2012) can be difficult to determine and is not always found (Ellison & Swanson, 2010; for an in-depth discussion, see Stoet & Geary, 2015). We believe that one factor contributing to these mixed results is the focus on sex differences in absolute performance, as contrasted with sex differences in academic strengths and associated attitudes.

In that 2015 paper they reference they study academic performance, and that is not related to equality of any kind.

> it is not simply the absolute level of mathematical competence that predicts entry into STEM fields, but also the level of competence relative to other academic skills. Individuals with strong mathematical abilities are more likely to pursue non-STEM careers if they also have strong verbal abilities, which include more women than men (Lubinski & Benbow, 2006). Our findings across all four PISA assessments revealed that in most countries boys have relatively better mathematics achievement and girls relatively better reading achievement, with implications for college and later occupational patterns, in particular high achievers entering STEM professions. These intra-individual differences may in fact be relatively more important than mean sex differences in pursuit of STEM careers

I also have looked at this question in my series of posts on the [issue](https://nintil.com/categories/gender-and-stem/)

UNESCO [looked at one particular subset of STEM](https://en.unesco.org/EQUALS/ICT-GE-paradox) careers: ICT (Information and Communications Technology) using again the GGGI, and they get the s[ame conclusions](https://en.unesco.org/EQUALS/ICT-GE-paradox). Now, this also points to another conceptual issue: STEM is lumping together careers like computer science and biology, and male/female preferences for these two vary to begin with. It could be the case that within STEM, equality/development redistributes students around, maybe in more developed countries you see more women in biology and less in CS; that might look like STEM doesn't vary with equality/development, yet its underlying university degrees do vary in this case.  They(UNESCO) are not using -it seems- a corrected measure like Stoet-Geary did, yet the correlation reemerges.

## Is it preferences changing?

The result above is just a narrow one: just about the relative preference for STEM and nonSTEM. But there other gender differences that vary with gender equality.

[Schmitt (2014)](https://link.springer.com.sci-hub.tw/chapter/10.1007%2F978-3-319-09384-0_11) surveys the literature on cultural variation of gender differences; a literature going back decades,the general view seems to be that _most sex differences in expressed personal values emerge more strongly in nations with egalitarian sex role socialization and greater sociopolitical gender equity._ 

This of course seems to run counter to one popular explanation, at least for the STEM case: That those differences are socialized, and that men and women are left unbiased, they will have a similar preference set. Indeed, in previous posts here at Nintil I looked at papers in sociology where a similar theory was tested. The theory was that 'gendered' values arose in now developed countries (For whatever reason) and then they spread via television and the media to other countries as they develop. Thus according to this theory, the causal factor would be existing stereotypes in culturally dominant countries, and its spread would happen in tandem with development and (For unrelated reasons), norms of gender equality, making development or gender equality a confounder. Alas, this view doesn't seem to get strong empirical support ([Here](https://nintil.com/2017/08/24/women-in-stem-a-cross-country-view/), scroll down to the bottom)

What else changes with equality? Many things have been posited to do; Table 11.1, 11.2, 11.3 look at different predictions made by various theories on why changes may occur. Related to our interests here, one might think that if a driver of career choice are abstract preferences about what sort of task one likes, studying those may help us see if the changes in relative preferences for STEM are spurious of if they are coherent with the story that preferences tell. In this case, contrary to the STEM case, there is no cultural variation,

> Occupational Preferences and Interests Sex differences in occupational preferences and interests have been documented such that men report significantly more interest in systematic, thing-oriented professions and women report significantly more interest in empathetic, people-oriented professions (Konrad et al. 2000; see also Nettle 2007; Su et al. 2009). Lippa (2010) found across 53 nations that men’s and women’s occupational preferences are entirely unrelated to egalitarian sex role socialization and greater sociopolitical gender equity.

Contrary to this, [@Scientific\_Bird](https://twitter.com/Scientific_Bird/status/1144952586648375296) points me to the ROSE study; a where they ask 15 year olds across the world what their interests are and they do find that more equal countries show larger inter-gender variance. I discussed this finding in a previous [post](https://nintil.com/2017/08/24/women-in-stem-a-cross-country-view/). \[This section needs expanding\]

## Why does this happen?

Stoet-Geary's proposed explanation is that academic skill gaps early on lead to difference in career enrolment later. They argue that boys are relatively better at science and girls are better at reading (More broadly one could cast this in terms of the spatial or numeric subcomponents of IQ vs verbal). This is true at the same time that girls outperform boys in both categories. But as what matters is relative choice, this ends up in a skewed distribution by career.

In turn, we need to explain why would this academic gap vary with gender equality. The paper doesn't go deeply into this, but there is an analysis at the end of the paper pointing to the role of economic development and welfare states: If STEM careers pay more, and living in a wealthier society with a safety net reduces the incentive to aim for high pay (Richer societies can support, say, more poets),then what is left influencing the preferences in those developed countries are the underlying biological biases towards different careers.

Looking at papers that cite Stoet-Geary, we find [Jiang et al. (2018)](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463) who study MOOC completion. This may be an even better measure, because even in a country that goes as far as banning women from university, they may still be able to access MOOCs from home; also MOOCs are generally cheaper than a university-level education, so even poorer students will be able to access them. Consistent with Stoet-Geary, the paper finds

> The smaller male-favoring gender gaps in STEM MOOC enrollment and completion in less gender-egalitarian and less economically developed countries indicate that MOOCs might offer broad country-level social benefits for less socially and economically developed countries. Free and easy access to MOOCs in developing countries allows females to try out STEM courses that are not easily available to them in their local communities. This finding also aligns with the educational-gender-equality paradox found by Stoet and Geary, i.e., the gender differences in the magnitude of relative academic strengths and pursuit of STEM degrees rose with increases in national gender equality \[[47](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463#pone.0202463.ref047)\]. These phenomena can be explained by the expectancy value theory \[[11](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463#pone.0202463.ref011)\]. The life-quality pressures in less gender-equal and less economically developed countries may increase females’ utility value of pursuing a STEM education and career, which in turn promotes females’ STEM engagement \[[48](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463#pone.0202463.ref048)\]. Pursuing a STEM education and career may be more appealing to females from less socially and economically developed countries, because STEM occupations are usually well paid and can provide economic security. On the other hand, the cost for females from more socially and economically developed countries to forgo a STEM career is relatively small, since there may be a higher level of social and economic security \[[47](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463#pone.0202463.ref047)\]. At the same time, females from more developed countries may be more influenced by gender essentialist ideology \[[22](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463#pone.0202463.ref022),[24](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0202463#pone.0202463.ref024)\], which in turn reduces their interest and engagement in STEM. We suggest that future studies be conducted to understand females’ decision-making process to enroll in and complete STEM MOOCs.

This evidence is also consistent (in that economic incentives can cause significant gender gaps) with my own observation that there is a cross-national correlation between teacher pay and gender composition of the teaching workforce. Quoting myself,

> One possible explanation is differences in pay. For teachers, for example, there is a correlation between how much male teachers earn relative to other university educated males, and the % of male teachers (r between 0.5 and 0.6). If females are less financially driven, we would expect a lower correlation for females. If so, then we would also expect that in countries with a higher ratio of teacher pay to the average, we would see less women. An this seems true to some extent. From OECD [Figure D5.a](https://www.dge.mec.pt/sites/default/files/EPIPSE/education_at_a_glance_2016.pdf), for primary education and [this](https://data.oecd.org/eduresource/women-teachers.htm#indicator-chart), countries with the lowest pay ratio for males were as follow (parenthesis are the % of female teachers) Hungary (96.95%), Italy(95.88%), Czech Republic(92.83%), the US(87.16%), and Norway(74.79%). Countries with the highest ratio, Luxembourg(74.5%), Finland(79.49%), Belgium(81.71%), Denmark(69.14%), Sweden(77.17%), and Slovenia(96.94%). Low pay ratio countries average 89.6% female. Countries with high pay ratio average 79.82% female. This lends some support to the idea that pay differentials are driving the observed choices of fields.

![Screenshot from 2019-06-27 08-08-14](https://artir.files.wordpress.com/2019/06/screenshot-from-2019-06-27-08-08-14.png)

Other proposed explanations found in the [wikipedia article](https://en.wikipedia.org/wiki/Gender-equality_paradox#cite_note-:2-3) for the "paradox" can be explained away as follows: First, even if the GGGI has issues, better indices like SIGI still correlate in the same way. Argueably, if the effect is indeed driven by economic development or welfare states, developed countries are also more gender equal. Second, another explanation, that there is a higher level of anti-female bias in developed countries, looking at so called implicit stereotypes via the IAT (Implicit Association Test) is eo ipso suspect: IAT may be as well a measure of reality: If most engineers around you are male, of course you will associate male with engineer more strongly; it is rational to do so. There is at least one paper ([Eagly & Linn, 2015](https://d-miller.github.io/assets/MillerEaglyLinn2015.pdf)) looking at wether outcomes cause stereotypes or viceversa, and unsurprisingly,

> Both causal directions between gender composition in science and gender-science stereotypes are thus plausible, although **gender composition likely influences stereotypes more directly than stereotypes influence gender composition**. The impact of stereotypes on gender compositions would be mediated over many years as women enroll in STEM courses and seek employment in STEM fields, whereas the impact of gender compositions on stereotypes can be more immediate (Lenton et al., 2009). Furthermore, **some of our study’s results would be difficult to explain if gender composition did not influence stereotypes in some way**. For instance, if the gender composition of science majors in college did not affect stereotypes, then stereotypes of individuals with and without college education should not differ. Another alternative hypothesis is that individuals with and without college education might differ on average if other correlated individual-level variables (e.g., age or socioeconomic status) influence stereotypes. However, our data supported neither hypothesis because **college education predicted stronger implicit stereotypes, but only in nations where men dominated science majors (see Figure 3). In contrast, college education predicted weaker implicit stereotypes in nations where women dominated science majors**. Compared with those alternative hypotheses, the associative-propositional model (Gawronski & Bodenhausen, 2006, 2011) can more parsimoniously account for the cross-level interactions with women’s representation in science, as discussed earlier.

Thus developed countries show a higher implicit bias, linking together men and STEM. But this is no indictment of us as sexists: It is an indictment of the whole IAT paradigm: Accurate representations of reality are not bias; it is a bias is the truth is expected to be an equal association. Going back to Stoet-Geary's proposed explanation; I was able to find a single piece of work ([A bachelor thesis in economics!](https://creativematter.skidmore.edu/cgi/viewcontent.cgi?article=1124&context=econ_studt_schol)) further exploring it. The author looks at the relation between welfare state policies and occupational segregation (Not quite the STEM gap at a university level, but arguably a close enough substitute), finding an association between social expenditures and occupational segregation.  

## Conclusion

Gender equality is associated with both more women in university, and less women choosing STEM vs non STEM. The net effect is less robust, and so the correlation equality->% women in STEM is weaker.

While I have not explored here the causal association between equality and more women going to university, the intuitive explanation that "they were legally/socially barred from doing so and equality removes those contraints" does seem plausible pending some further analysis.

As for the causal association between equality and relative preference for STEM and non STEM, Stoet-Geary's favored explanation, one appealing to economic incentives, and driven by economic development and the rise of the welfare state seems true as far as I've been able to find.

This does not rule out other factors playing a role; for example it can be that in less gender-equal countries, women are _forced by their parents_ to study STEM, e.g. to signal their smarts to potential husbands.  I'd be interested in hearing if you think Stoet-Geary's explanation suffices or if we need something else.

_Ricón, José Luis, “On the educational gender-equality paradox”, Nintil (2019-06-26), available at https://nintil.com/gender-paradox/._