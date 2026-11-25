# 附加内容：Sociological explanations

---

## 正文

在我此前关于 Google memo 的文章中，我反复提到，women 在 STEM education 中的 proportion 存在 international gap。有些人对此提出批评，举出了 United Arab Emirates、India 或 Saudi Arabia 等例子。我曾试图在此前的文章中回应其中一些批评，但从一些反应来看，那些回应并不十分有说服力，而且我承认，写得缺乏条理。

因此，这里对这个问题作一次更有条理的探讨。conclusion 仍然相同：如果我们以正确的方式进行 calculations，那么几乎所有地方的 Women 在 STEM 中都是 underrepresented（<40%）。

* * *

我拿到了这份[来自 Unesco 的 dataset](http://data.uis.unesco.org/)，其中包含许多 countries 在不同时期的情况，关键是还包含 STEM 中 women 的 %，以及 choose STEM 的 women 的 %。我们先看看初步情况。这个 dataset 将 tertiary education fields 进行 groups，分成若干 categories。与我们有关的是：

-   Information and Communication Technologies（信息与通信技术）
-   Engineering, Manufacturing and Construction（工程、制造与建筑）
-   Natural Sciences, Mathematics and Statistics（自然科学、数学与统计学）

此外，其中还有

-   Science, Technology , Engineering and Mathematics（大致作为上述类别的 aggregate）

有些 datasets 只有前面 3 类。遇到这种情况，我就将它们 aggregated。我最终的 sample 包含 874 个 observations，时间从 1999 到 2015，涵盖 122 个 countries，每年约有 40-50 个 observations（并不总是相同的 countries，而且有些 countries 严重 underrepresented）。

下面第一个 plot 根据 observations 是否来自“rich” countries 来 divides 它们。这里，“rich”指 log(GDPpc)>10，而且来自 natural resources 的 GDP 的 % 低于 20%。cutoff 大致位于 Greece、Slovenia 或 Poland 这样的 countries。

![Figure\_1.png](https://nintil.com/old_assets/2017/08/24-1.png)

两种情况下，medians 都低于 40，而且 rich countries 的 mean 和 variability 都更小。这与我此前讨论过的 OECD data 相符。按 GDP 来看，情况如下（每个 dot 都是某个 country 在某一年的 observation，dot 的 size 取决于 population）：

![Figure\_1-1.png](https://nintil.com/old_assets/2017/08/24-2.png)

这表明，STEM 中 female students 的 % 可能存在先 increase 后 decrease 的 pattern。我的 sample 中有许多 small countries，其 % 呈现出 high variability，这可能会增添 noise：

![Figure\_1-2](https://nintil.com/old_assets/2017/08/24-3.png)

是的，看来确实如此。那么，有没有哪个 country 在它自身的 time series 中，on average 高于 40？有，有几个。我将整个 dataset 中的 mean 和 obervation count 用 plot 展示出来。

![Figure\_1-2.png](https://nintil.com/old_assets/2017/08/24-4.png)

据此，ranking 顶部和底部的 countries 是：

|  | female\_STEM |
| --- | --- |
| Country |  |
| \--- | \--- |
| Myanmar | 65.543585 |
| Sierra Leone | 58.974360 |
| Liberia | 57.545090 |
| Antigua and Barbuda | 53.283025 |
| Algeria | 49.145176 |
| Mongolia | 48.623639 |
| Tunisia | 48.171220 |
| Bahrain | 48.013888 |
| Brunei Darussalam | 47.911507 |
| Grenada | 47.670250 |
| Oman | 46.778222 |
| Qatar | 46.775434 |
| Uruguay | 45.484430 |
| Saudi Arabia | 45.380228 |
|  |  |
| Netherlands | 20.456234 |
| Benin | 19.837320 |
| Kenya | 19.276040 |
| Mozambique | 19.236314 |
| Iraq | 18.521590 |
| Switzerland | 18.417970 |
| Lao People's Democratic Republic | 18.264617 |
| Burkina Faso | 17.181350 |
| Cambodia | 16.414561 |
| China, Macao Special Administrative Region | 15.756433 |
| Aruba | 14.893620 |
| Ethiopia | 12.048119 |
| Eritrea | 7.492512 |
| Niger | 5.882350 |
| Chad | 4.081630 |

这里的 variability 相当大，需要解释。我尝试考察了不同的方面，最终找到了第一个 explanation：并非在所有 countries，Women 都 equally likely 去 attend university。换言之，如果你的 students 中 90% 都是 female，那么即使只有很小的 proportion 会 choose STEM，她们也会 outnumber men。为了对此 correct，我 calculated 了在 total university graduates 中 50% 为 female 的情况下，我们会 observe 到的 **hypothetical gender proportion in STEM**。具体做法是，先得到 men 中有多少 % 会 go to STEM，以及 women 中有多少 % 会 go to STEM。有了这两个 values，假设两者的 number 为 equal，它们的 quotient 就能告诉我们，STEM 中每位 women 对应多少 men，由此便能得到 hypothetical proportion。

我认为，这对于了解 men 和 women 真正的 choosing 情况，以及避免 attend uni 的 female 的 % 对我们的 results 造成 corrupting，都很重要。我在下面将两个 variables 用 plot 展示出来，同时画出一条 line of equiproportionality。正如你所见，几乎在所有情况下，每一个 observation 都 falls below 它：一旦我们 account for female students 的 %（以及几个我稍后会讨论的 outliers），STEM 中 females 的 % 就往往会 fall。

![Figure\_1-3.png](https://nintil.com/old_assets/2017/08/24-5.png)

与此前一样，我们按 wealth 将它分开来看：

![Figure\_1-4.png](https://nintil.com/old_assets/2017/08/24-6.png)

这里 rich 的含义与此前相同。chart 右侧那些高于 40 的 dots，是 Qatar 或 Saudi Arabia 等 oil-rich arab countries。

![Figure\_2-1.png](https://nintil.com/old_assets/2017/08/24-7.png)

那个出现在 40 以上的 rich country（其余大多是 oil-rich arab states），是 2006 年的 Brunei Darussalam。我们看看它此后的变化：

![Figure\_1-5.png](https://nintil.com/old_assets/2017/08/24-8.png)

显然是一个 outlier。Brunei 的 population 很小，2006 年为 350.000 人。因此，如果我们假设 access to university 存在 parity，就可以确立这样一个 claim：**自 1999 年以来，没有任何 developed country 的 STEM 中 women 的 % 曾高于 40%**。我们能将其 generalise 到其他 countries 吗？（注意，这里说的是 STEM，不是 engineering）

我们来看看所有曾在任何一年高于 40% 的 countries。这样做会得到如下完整名单：Burundi、Liberia、Sierra Leone、Cambodia、Gambia、Eritrea、Bangladesh、Kyrgyzstan、Angola、Myanmar、Palestine、Samoa、India、Swaziland、Morocco、Grenada、Algeria、Azerbaijan、Panama、Oman、Saudi Arabia、Brunei Darussalam、Qatar。

![Figure\_1-6.png](https://nintil.com/old_assets/2017/08/24-9.png)

其中一些 countries 只有少量 datapoints。如果我们 discard 只有两个或更少 observations 的 countries（Algeria、Bangladesh、Burundi、Gambia、Grenada、Liberia、Myanmar、Samoa 和 Sierra Leone），再 discard 那些 on average 低于 40% 的 countries，就只剩下 Saudi Arabia、India 和 Swaziland。它们的情况如下：

![Figure\_1-7.png](https://nintil.com/old_assets/2017/08/24-10.png)

这些情况需要一个 explanation。我在此前的一篇文章中讨论过 India，也讨论过 Saudi Arabia。Swaziland 的 population 约为 1 million 人，而我实在找不到任何 data，表明那段时期针对 women 的 stereotypes 有所 increase。这可能是 noise，也可能是真实的 trend。看来，我们可以看看 data 数量最多的 countries（超过 15 个 datapoints）。

![Figure\_1b](https://nintil.com/old_assets/2017/08/24-11.png)

UK 发生了什么！OECD data 也显示，science、mathematics 和 computing subfield 在[那两年之间出现了 9 points 的 jump](https://stats.oecd.org/Index.aspx?DataSetCode=EAG_GRAD_ENTR_FIELD#)。我倾向于认为这是 data 的 redefinition 所致，但谁知道呢。

那么，在 developed countries 内部，各个 country 相互比较时，各自有多 variable：

![Figure\_2-2.png](https://nintil.com/old_assets/2017/08/24-12.png)

这些 countries 按 increasing population 进行 sorted。再次可以看到，variance 随 population 而 decreasing（blue line 同时表示 mean 和 median，因为两者在这里几乎相同）

![Figure\_1-8.png](https://nintil.com/old_assets/2017/08/24-13.png)

如果我们接受 countries 内部很大一部分 variance 都是 noise，那么就可以再次回到整体视图，

![Figure\_1-9.png](https://nintil.com/old_assets/2017/08/24-14.png)

又是 India（！）。India 告诉了我们一件已知的事：GDPpc 与 STEM 中 females 的 % 之间存在 relation。考虑到 India 的 population，它应该能为具有其 GDP 水平的 country 应有的 % 提供一个很好的 estimate。

最后，我们可以回到 development（以 GDP 作为 proxi'ed）与 STEM 中 women 的 % 之间的 relationship。这个 fit 不包括 natural resource rents 超过 20% 的 countries（该 value 显示在 colorbar 中）。

![Figure\_1-12.png](https://nintil.com/old_assets/2017/08/24-15.png)

免得你觉得，我可能忽略了一个 %muslim-women in STEM correlation：

![Figure\_1-14.png](https://nintil.com/old_assets/2017/08/24-16.png)

但这可能是由 muslim-GDP correlation 所 driven，而后者又与 STEM 中的 females 存在 correlated 关系。

![Figure\_1-15.png](https://nintil.com/old_assets/2017/08/24-17.png)

先在这里停下来作个小结：关键在于，这些 variables（以及其他 variables）中有许多是 correlated 的。我纳入了 fm\_STEM 和 fm\_EDU，即 STEM 以及 education 和 healthcare 中的 women to men ratio；equality\_index，即 OECD SIGI 的 women discrimination index；Nrrent，即来自 natural resources 的 GDP 的 fraction；以及 IQ，它取自最新且 cross-nationally valid 的 dataset（[NIQ-dataset](https://www.researchgate.net/project/Worlds-IQ)。

![Figure\_1](https://nintil.com/old_assets/2017/08/24-18.png)

一种 approach 是做 multivariate OLS，但这会让 collinearity 难以处理。我们也可以先对一些 variables 做 OLS，然后取 residuals，再做更多 OLS，以 control for causally antecedent variables。但这需要时间，而且这里的 causality 可能朝许多方向运行。因此，我选择用 SEM（Structural Equation Modeling）对 variables 的 causal influence 进行 model。采用这种方法，我们先 defines 预期 data 具有的 relations 的 set，包括 cause 和 correlations，然后 solver 尝试寻找 coefficients，使 model 尽可能 resemble observed values。

我具体使用的 model 是：

> -   Development（latent variable）explains life expectancy、log(child mortality)、log(GDP per capita) 和 IQ。
> -   Share of muslims 与 development 存在 covaries 关系（direction 未 specified）
> -   由 [OECD's SIGI](http://www.genderindex.org/) 所 measured 的 Equality，depends on development 和 share of muslims
> -   tertiary education 中 women 的 %，depends on development 和 equality
> -   tertiary education 中所有 women 里 choose STEM 的 %，与 tertiary education 中所有 men 里 choose STEM 的 %，两者的 ratio（female\_over\_male）depends on development 和 equality。preference ratio 为 1，意味着 men 和 women 以 equal proportions 来 choose STEM。更高的 ratios 意味着 women 比 men 更 likely 去 choose STEM。
> -   GDP per capita 通过从 total GDP 中 substracting natural resources 的 share 来进行 corrected。

我有 3023 个 observations，但有许多 empty items。例如，只有 8.4% 的 entries（两年）具有 equality index，而所有 rows 中有 20-25% 具有 female\_over\_male variable。model 使用 [maximum likelihood with robust standard errors](http://lavaan.ugent.be/tutorial/est.html) 以及 full information estimation 来 estimated，从而对于每一 pair 的 variables，所有具有 entries 的 rows 都会被考虑。如果我只使用 complete rows，correlations 也类似。仅考虑 statistically significant（p<0.05）的 correlations，result 如下：

![2017-08-20 17-11-34 的屏幕截图.png](https://nintil.com/old_assets/2017/08/24-19.png)

其中的 values 是 standardised coefficients（读作：variable A 增加 1 standard deviation，会 leads to variable B 增加 X standard deviations），而 development 的 components 上方的 values，则是这些 variables 进入 development latent variable 的 loadings。

换言之，development 本身并不能 predict preference ratio 的 changes。但 equality 可以，而且确实会 negatively 地 predict 它。对于 muslims 而言，women 在那里 overrepresented 的原因，并不在于 % muslim 本身的 effect，而在于通过较少的 equality 产生的 indirect effect。

### Engineering：工程学

上面的讨论针对 STEM。但具体到 engineering 又如何呢？Engineering 往往比一般的 STEM 有更大的 gaps，因此值得研究 engineering 中的 gender gap 在各个 countries 之间如何 differs。我也有这方面的 data，因此提供几张 plots。

首先是 STEM 与 engineering 之间的 relation（使用我的 hypothetical variable）：

![engi.png](https://nintil.com/old_assets/2017/08/24-20.png)

这意味着，正如预期，两者之间存在 relationship，但几乎从未出现过某个 observation 中 engineering 的 % female greater than STEM 的 % 的情况。这可能表明，在世界各地的 STEM 内部，women 正在其他 subjects 中 clustering。我们来 define 一个 Engineering /STEM ratio，看看它相对于 GDP 如何 behaves，

![stemengi.png](https://nintil.com/old_assets/2017/08/24-21.png)

将 Engineering 中的 hypothetical % of female 纳入上述 structured model，仅对 development yields 一个 significant correlation (d=-.26)，而且这个 effect 是 direct，并非由 equality mediated，这与 STEM 中的 % of female 不同。

本节的完整 data 可以在[这里](https://pastebin.ubuntu.com/25378749/)找到，采用 csv format。dataset 中还有更多 variables，如果你确实想了解，欢迎在评论中提问。

### Conclusion：值得关注的 Countries

在我看来，这篇文章最重要的收获，是一份在某种意义上不 match 这一 trend 的 countries 名单。应该对这些 countries 进行研究，以理解它们之间 differences 的 causes。

一组 countries 是 Saudi Arabia 和 United Arab Emirates。这些 countries 具有 low-equality、high income、high % muslim 的特征，而且 STEM 中 women 的 share 相对较高。

第二组 countries 是 Switzerland 和 Netherlands，这些 rich western countries 的 equality 很高，而 % 相对较低。（我们可以在这里加上 Macao）

第三组是 Italy、Malaysia 和 India，这些 countries 的 STEM 中 women 的 share 相对较高，尤其是 Malaysia。（不过，[Malaysia](https://www.digitalnewsasia.com/insights/the-gender-gap-in-stem) 与 India 一样，STEM related industries 中 women 的 share 更小。这些 women 都去了哪里？）

第四组是 STEM 中 women 很少的 low income countries，例如 Chad、Niger 或 Iraq。

最后，第六组包含一些我们可以视为 developed、但 cultures 不同的 reference countries：Sweden、Denmark、Japan、South Korea、UK 和 US。

## 附加内容：Sociological explanations

这与文章的主要话题有点无关。我曾考虑把这篇文章分成两篇，但最终决定还是写成一篇。

Sociologists 并非视而不见。他们知道 female/male 的 choices of major 存在 international differences，但他们往往提出 culture-based 的 explanations。我在下面摘录了 [Maria Charles (2011)](http://www.annualreviews.org/doi/abs/10.1146/annurev.soc.012809.102548) 最近一篇 review 的部分内容。这篇 review 涉及的内容不止 STEM 中的 women，但我在这里聚焦于我们感兴趣的内容。

> 尽管一些 degree programs——其中最突出的是 business、law 和 medicine——随着时间的推移已经 integrated，但自 1980s 以来，United States 和其他 industrial countries 中各 fields 的 gender distributions changed 相当少 (Bradley 2000; Jacobs 1995, 2003; Xie &
> 
> Shauman 2003, England & Li 2006)。此外，cross-national analyses reveal，socioeconomic modernization 与 mathematical 和 technical fields 中的 female representation 之间存在 weak、有时甚至 negative 的 relationships，而这些 fields 在当今 industrial West 是如此强烈地 male-typed (Charles & Bradley 2002, 2009; Charles 2011)。例如，在 Turkey 的 computer science programs 中，women 的 represented 程度与 Sweden 大致相当 (Charles & Bradley 2006; 另见 Lagesen 2008 对 Malaysia 的研究)。
> 
> 而在 engineering programs 中，women’s presence 实际上在 developing 比在 advanced industrial societies 中更强。Figure 2 针对一个包含 44 个 countries 的 sample depicts 这一 relationship。它显示 GDP per capita 与 engineering 中的 female representation 之间存在 negative correlation (−0.48)。$$..$$
> 
> 上述 patterns 引出了两个 questions，以下各节将予以讨论：为什么我们在一些 indicators 上看到 equalization，而在另一些上看不到？以及，为什么一些 curricular 和 career outcomes 在 advanced industrial countries 中更加 gender-typed？
> 
> 事实上，越来越多的 Evidence 表明，一些 facilitated female access to labor markets 和 educational systems 的相同 structural 和 cultural forces，也 contributed to 这些 institutions 内部的 sex segregation。
> 
> 在 higher education 中，同时推动 increased female participation 和 sex segregation 的 historical pressures，在 policy agendas 和 official discourse 中清晰可见。自 twentieth century 中叶以来，national governments 和 international organizations 一直努力通过 implementing 围绕 prevailing gender stereotypes 而设计的 programmatic changes，来 increase colleges 和 universities 中的 female enrollment (Charles & Bradley 2002, Bradley & Charles 2004)。1953 年，United Nations Educational, Scientific and Cultural Organiza- tion 向各国 ministries of education 提出了以下 resolution。UNESCO (1953, p. 263) 决议
> 
> 为 facilitate women’s access to higher education，并使其 abilities 的运用为 society 带来最大福祉，university studies 应 permit women 在特别 suited to feminine aptitudes 的 fields 中 specialize，并为她们如今正在获得机会进入的新 careers，assure 她们更 adequate training。

1953 年可能确实如此。但最近几十年还是这样吗？要说有什么变化，那就是现在有一股强大的力量在推动 equality，尤其是通过 increasing STEM 中 women 的 share。我猜，之所以没有人呼吁在 female-heavy fields 中实现 equalisation，是因为这些 fields 往往 less lucrative 或 useful，因此从一个没有任何 taste 的 pure profit maximiser 的角度来看，是 irrational 的。如果假定 tastes 是 equal，那么 men 做的是正确的事，而 women 因为 stereotypes 没有这样做，所以需要被 nudged 向 STEM 的是她们，而不是 men。

> 为什么 CAREER OUTCOMES 在 ADVANCED INDUSTRIAL CONTEXTS 中更加 GENDER-TYPED？对这个问题，一个显而易见的回答是，生活在 affluent societies 的人们更容易 absorb 与追求 female-typed occupational 和 educational careers 相关的 material costs。换言之，在 advanced industrial contexts 中，满足 stereotypically female preferences 的 economic capacity 更大。$$这一 interpretation 与 United States 的 individual-level research 的 findings 是 consistent 的。例如，来自 high-income families 的 American girls 在 college 中 study math 和 science 的可能性更低 ([Ma 2009](http://www.jstor.org/stable/10.1525/sop.2009.52.2.211?seq=1#page_scan_tab_contents))。{但请注意，也有 contrary findings，这值得今后探索（作者注}$$ \*\*一些 scholars 反对那些关于 \*\***sex segregation、援引 women’s preferences 和 choices 的 explanations，因为这一路径的 inquiry 似乎会把注意力从 inequality 的 structural 和 cultural causes 上移开。**但承认 gender-differentiated aspiration 并不是 blame the victim，除非在考虑 preferences 时，将其与它们产生的 social contexts 隔离开来。**这一 explanation**  **并不要求 gendered dispositions 是 innate，甚至不要求它们是 deeply internalized。如上所述，educational 和 career aspirations 是由关于自身 competencies 和 affinities 的 beliefs、关于特定 social positions 的 masculine 或 feminine task content 的 beliefs，以及关于哪些 categories of persons 是这些 positions 的 appropriate incumbents 的 beliefs 所 shaped 的。**

> **这些 beliefs constitute 人们作出 life choices 的 cultural environment，而且它们可能是 sex segregation 的 consequence，而不仅仅是其 cause。**
> 
> Aspirations 也由关于 educational 和 occupational pursuits 的 nature 和 purpose 的更一般的 beliefs 所 shaped。在 advanced industrial societies 中，parents 和 educators 通常劝告年轻人，或许尤其是 girls，要“追随自己的热情”，并在那些能让他们实现“真实自我”的 fields 中学习和工作。尽管这些所谓的 postmaterialist values 正在全球传播，但如今它们在 affluent、late-modern societies 中最为清晰可见 (Inglehart 1997, Inglehart & Baker 2000, Meyer & Jepperson 2000, Beck & Beck-Gernsheim 2001)。\*\*在这些 contexts 中，Curricular 和 career choices 已不仅是 practical economic decisions；它们也代表着 identity construction 和 self-affirmation 的 acts。由于 gender 仍然是 human identity 如此 fundamental 的一个 axis，individual self-expression 往往导致“对 \*\*\*\*gendered selves 的 expression\*\*” (Charles & Bradley 2009)。
> 
> **关于 self expression 的 Normative mandates，通过 supporting culturally masculine 或 feminine aspirations 的 development 和 realization，intensify sex segregation。在这一过程中，stereotypes 和 gender identities 被 entrenched，educational 和 occupational fields 的 gender labeling 被 reinforced，而 sex segregation 被 legitimized**。例如，打算“学习自己所热爱的东西”的 American girls，不太可能考虑 male-labeled science、engineering 或 technical fields，尽管这些 degrees 提供了 relative material security。mathematics 和 science 与 female gender identities 的 nonalignment，甚至可能在这些 postmaterialist contexts 中 generate attitudinal aversion。新的 evidence 表明，boys 和 girls 对 mathematics 的 attitudes，在 developed societies 中确实比在 developing 和 transitional societies 中 diverge 得更多 (Charles & Bradley 2009)。
> 
> 如上所述，advanced industrial countries 中的 High levels of sex segregation，也受到 labor markets 和 educational systems 的 structural features 的 supported。这些 features 包括 large service sectors 和 diversified systems of higher education。它们已在世界各地出现，但这些 transformations 在 affluent European 和 North American countries 中开始得更早，迄今也最为显著 (Benavot 2006)。

换言之：Sociology 认为，women 确实 prefer 更多 STEM。但在 developing countries 中，由于 economic factors，gap 更小。而 preferences 是 socially constructed 的，所以人们“自由选择”意味着“人们自由表达 society 教会他们表达的东西”。

另一篇 article ([Mann & DiPrete, 2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3791309/)) 讨论了这一 gap，并提到本博客已经讨论过的许多关键点：women 表现出对 liberal arts 的更强 preference；high ability women 往往比 high ability men 具有更高的 verbal ability。这篇 paper 既没有 affirms，也没有 rejects 这些 preferences 的 differences 可能具有 biological origin。

[Bradley & Charles (2009)](http://cedar.wwu.edu/cgi/viewcontent.cgi?article=1002&context=sociology_facpubs) 专门尝试通过 analysing 44 个 countries，来探究 segregation by field 的 cause。

> 我们 argue，关于 women’s status 的 conventional evolutionary models 无法为 field of study 中 sex segregation 的 cross-national 和 historical variability 提供 satisfactory account，因为 **它们 underestimate 了 gender-essentialist ideology 的 enduring cultural force**（即关于 fundamental 和 innate gender differences 的 cultural beliefs）；事实证明，即使在最 liberal egalitarian societies 中，这种 ideology 对 shaping life experiences、expectations 和 aspirations 也有极强的影响（Fenstermaker and West 2002; Charles and Grusky 2004; Correll 2004; Ridgeway 2006）。**此外，gender-essentialist beliefs 的 segregative effect 还被 Western culture 对 individual self-expression 和 self-realization 的强烈强调所 intensified**；这种强调自第二次世界大战以来一直在全球传播（Meyer and Jepperson 2000; Frank and Meyer 2007），如今在 affluent late-modern societies 中最为明显（Inglehart 1997; Beck and Beck-Gernsheim 2001）。$$...$$ **与许多 evolutionary theorists 的 predictions 相反 {在 sociological 意义上}，我们 find，在更 economically developed contexts 中，academic fields 普遍有更 greater segregation 的 tendency。** $$...$$ field of study 中的 Sex segregation，其 legitimacy 并非 derives 自 male primacy 的 norms，而是 derives 自它与 gender-essentialist beliefs 和 dispositions 之间看似存在的 homology。因此，与许多类型的 vertical inequality 相比，它受到的 political 和 social contestation 更少，也容易与 self-expressive value systems 相 reconciled。事实上，按 Inglehart 的严格含义，这类 sex segregation 甚至可能不 constitute “inequality”。$$...$$ 总之，我们的 pooled 44-country analyses 的 results suggest：(1) figure 1 中揭示的 sex segregation 与 economic development 的 association，至少部分 attributable to economic structure 的 cross-national differences（即 postindustrialism）；(2) mathematics 的 affinity 中 aggregate-level gender differentiation 确实有助于 predict math-intensive fields 中的 female representation（尤其是 math 和 natural sciences）。$$...$$ 与我们提出 sex segregation 和 gendered curricular affinities 之间存在 positive feedback effects 的 argument 一致，table 2 中的 descriptive statistics reveal，**advanced industrial societies 中 expressed affinity for mathematics 的 gender gap 比 developing/transitional societies 中更大**，且达到 significantly 的程度（t-ratio p 2.15; P p .037）。在前一 group 中，声称自己“like math”或“like math a lot”的 girls，其 percentage on average 比作出相同声称的 boys 的 percentage 低 5.42 points。在 developing/transitional societies 中，这个 gap 只有 2.05 points。正如 table 5 所示，这些 country groups 在 related indicators 上也存在 large、statistically significant differences，包括 professed affinity for science（“我 like science”）以及 math-related careers 的 aspirations（“我想从事涉及 mathematics 的工作”）中的 gender gaps。这些 gaps 并不 correspond to girls 的 mathematical achievement 的简单 differences；事实上，table 2 表明，更 economically developed countries 中的 math achievement gap 略小。26 table 5 中的 mean differences 提供了 preliminary evidence，表明 national prosperity 和 self-expressive value systems 的兴起会 promote gender-differentiated curricular dispositions 的 development（或 expression）。women 和 men 的这些 dispositions 所呈现的 cross-national variability，其背后的 causal processes 是今后 research 的一个重要 area。

> 在 developing and transitional societies 中，multivariate models confirm，随着 per capita GDP increases，存在 greater segregation 的 net tendency。我们 suggest，economic modernization 并不具有 modernization scholars 所期待的 degendering effect，因为在 greater scarcity 的 conditions 下，任何对 lower-paid female-typed careers 的 preference，其 realization 都更 constrained（而且更不 culturally legitimate）。37 从这个角度看，increasing sex segregation 可能 reflect 了 university-educated persons 所承受的 normative 和 material pressures 的 decreasing：这些 pressures 要求他们不论 gender，都在 lucrative scientific and technical fields 中 obtain credentials，并在被认为对 national development 和 international economy 中的 competitiveness 至关重要的 fields（如 engineering 和 natural sciences）中 build human capital。**换言之，随着 national prosperity grows，material security 和 national economic development 这些 instrumental goals，在 curricular decisions（由 students 本人、parents、family members 和 educational gatekeepers 作出）中的地位越来越不 central**。但这种 segregative GDP effect 只在一定范围内是 measurable 的。在 advanced industrial societies 中，national prosperity（GDP）的进一步 increases 并不对 curricular distributions exert segregative effect，因为如今在 mass-societal level 上，existential security 已被普遍视为理所当然。在这些 contexts 中，Strongly diversified university curricula 通常被赞颂为 individual self-realization 的 instruments。为了追求热爱（或自以为的热爱）而 Sacrificing material rewards，更具有 culturally legitimate 的地位——尤其对 women 而言，因为人们通常并不期待她们成为 primary breadwinners。随着 individuals 寻求 express 其 essential（male 和 female）selves，academic fields 的 gender labeling intensifies，这些 fields 中的 distributions 也变得更 closely aligned with gender-specific curricular dispositions。**因此，在 advanced industrial segregation regimes 中，girls 的 relative mathematical affinities 的 Crossnational variability（而不是 realize 这些 affinities 的 economic capacities 的 variability）成为各 curricular fields 中 gender distributions 的 decisive determinant。**

最后，至于 reducing 这个 gap 的方法，他们的建议之一是，强制 girls 学习更多 maths 和 science related courses

> 此前的 comparative and historical research 提供了 evidence，表明 **对 curricular choice 的 limitations——无论是通过在整个 secondary school 阶段对 math 和 science coursework 提出 universal requirements，还是通过更强地依赖 merit-based curricular placement——都会 results in scientific and technical fields 中更强的 women representation**，以及 peers 对 students 的 educational aspirations 更弱的 influence（OECD 1986; Barinaga 1994; Hanson, Schaub, and Baker 1996; Buchmann and Dalton 2002）。41 **这些 restrictive educational policies 与 United States 及许多其他 advanced industrial societies 的 practices 严重不符；在那里，high-level math 和 science 是 optional 的，而“freedom of choice”受到广泛赞颂**（Frank and Meyer 2002; Schwartz 2004）。但要求 conform to gender norms 的 social pressures，往往在 adolescence 期间 peak（Entwisle and Greenberger 1972; Gaskell 1985）。**Allowing girls 在 teenage years 期间 opt out of science 和 mathematics courses，会通过 limiting 后续 curricular options 而 increases higher education 中的 sex segregation**（Charles et al. 2001; Xie and Shauman 2003），**也会通过 reducing 在 male-typed programs 中 attained stereotype-eroding critical mass 的 high-achieving girls 的 likelihood 而产生这种影响**（RiegleCrumb, Farkas, and Muller 2006; Frank et al. 2008），还会通过 reinforcing scientific 和 mathematical activities 的 gender labeling 而产生这种影响。42 **虽然更 universal 的 secondary school requirements 可能让一些人觉得 antidemocratic，但这一顾虑必须与另一种 possibility 权衡：girls 和 boys 看似 free 的 choices，其实受到那些被视为理所当然的 assumptions 的 constrained，即他们会喜欢什么、擅长什么；也受到他们可能预期的 social sanctions 的 constrained——如果他们选择 pursue gender-atypical educational and career paths 的话。**

forced choice 对 women 的 effects 值得 studying。

他们还讨论了 segregated schools，但予以 reject，原因似乎是反感许多 proponents 所提出的理由。“我们 oppose X，因为我们认为 Y 是 false，而认为 Y 是 true 的人 favour X”。

> 有人建议，将 Enrollment 于 single-sex secondary schools 或 classrooms 作为一种手段，以便在具有 formative 作用的 adolescent years 期间 reducing gender 的 salience 和 gender performances 的 frequency（Thompson 2003）。尽管一些 beneficial psychological 和 pedagogic effects 已被 documented（Lee and Marks 1990; Shapka and Keating 2003），任何推动 single-sex education 的 large-scale initiative 都无疑会带来许多 unintended consequences（例如，Datnow, Hubbard, and Conchas 2001）。**更重要的是，我们注意到，single-sex educational policy initiatives 的 philosophical rationale，往往正是 based upon 那些 gender-essentialist presumptions；而最初正是这些 presumptions 帮助促成了 scientific and technical fields 的 segregation（Hoff Sommers 2000; Gurian 2002）。**在我们看来，对 public schools 或 high school mathematics classes 实施 Intentional sex segregation，是一种 unrealistic policy option。

因此，sociologists 再一次并非视而不见：他们

_Ricón, José Luis, “Women in STEM: a cross-country view”, Nintil (2017-08-24)，可获取于 https://nintil.com/women-in-stem-a-cross-country-view/._

---

## 原文

In my previous articles about the Google memo, I mentioned repeatedly the existence of an international gap in the proportion of women in STEM education. Some people criticised this, citing examples of, for example, United Arab Emirates, India, or Saudi Arabia. I tried to answer some of those critiques in previous posts, but I gather from some reactions that it wasn't very convincing, and I admit it was disorganised.

So here is a more structured exploration of that. The conclusion is still the same: Women are underrepresented in STEM (<40%) mostly everywhere, if we do our calculations the right way.

* * *

I got my hands on this[dataset from Unesco](http://data.uis.unesco.org/) that contains many countries across time, and crucially,the % of women in STEM, and the % of women who choose STEM. Let's have a look at how things look initially. The dataset groups tertiary education fields into a bunch of categories. The ones of relevance to us are:

-   Information and Communication Technologies
-   Engineering, Manufacturing and Construction
-   Natural Sciences, Mathematics and Statistics

And in addition they have

-   Science, Technology , Engineering and Mathematics (As an aggregate of those, roughly)

In some datasets they have only the 3 former. When that happened, I aggregated them. My final sample has 874 observations, from 1999 to 2015, and covers a 122 countries, with around 40-50 observations per year (not always the same countries, and some are heavily underrepresented).

The first plot below divides observations by whther they are "rich" countries or not. In this case, "rich" means that log(GDPpc)>10, and that % of GDP from natural resources under 20%. The cutoff is at countries like Greece, Slovenia, or Poland.

![Figure\_1.png](https://nintil.com/old_assets/2017/08/24-1.png)

In both cases, medians are below 40, and rich countries show both a smaller mean and variability. It fits with OECD data that I have already discussed. By GDP, it looks like this (Each dot is an observation of a country in a given year, size of the dot depends on population):

![Figure\_1-1.png](https://nintil.com/old_assets/2017/08/24-2.png)

This suggest a pattern of increase and then decrease in the % of female students in STEM. There are many small countries in my sample that show high variability in the % that might be adding noise:

![Figure\_1-2](https://nintil.com/old_assets/2017/08/24-3.png)

So yes, so it seems. Now, are there any country that, within its own time series, on average, is over 40? Yes, a few. I plot mean over the dataset, and the obervation count.

![Figure\_1-2.png](https://nintil.com/old_assets/2017/08/24-4.png)

According to this, the countries at the top and bottom of the ranking are:

|  | female\_STEM |
| --- | --- |
| Country |  |
| \--- | \--- |
| Myanmar | 65.543585 |
| Sierra Leone | 58.974360 |
| Liberia | 57.545090 |
| Antigua and Barbuda | 53.283025 |
| Algeria | 49.145176 |
| Mongolia | 48.623639 |
| Tunisia | 48.171220 |
| Bahrain | 48.013888 |
| Brunei Darussalam | 47.911507 |
| Grenada | 47.670250 |
| Oman | 46.778222 |
| Qatar | 46.775434 |
| Uruguay | 45.484430 |
| Saudi Arabia | 45.380228 |
|  |  |
| Netherlands | 20.456234 |
| Benin | 19.837320 |
| Kenya | 19.276040 |
| Mozambique | 19.236314 |
| Iraq | 18.521590 |
| Switzerland | 18.417970 |
| Lao People's Democratic Republic | 18.264617 |
| Burkina Faso | 17.181350 |
| Cambodia | 16.414561 |
| China, Macao Special Administrative Region | 15.756433 |
| Aruba | 14.893620 |
| Ethiopia | 12.048119 |
| Eritrea | 7.492512 |
| Niger | 5.882350 |
| Chad | 4.081630 |

This is a substantial amount of variability that needs to be explained. I tried looking at different things, and in the end I found a first explanation: Women are not equally likely to attend university in all countries. In other words, if 90% of your students are female, then even if a tiny proportion choose STEM, they will outnumber men. To correct for this, I calculated the **hypothetical gender proportion in STEM** that we would observe if 50% of total university graduates were female. I did did by getting which % of men go to STEM, and which % of women go to STEM. With those two values, assuming equal number for both, their quotient gives us how many men per women would be in STEM, and from that one can get the hypothetical proportion.

I think this is important to see what men and women are really choosing, and avoiding the % of female who attend uni from corrupting our results. I plot both variables below, along with a line of equiproportionality. And as you can see, in almost all cases, every single observation falls below it: once we account for % of female students (and a few outliers that I'll discuss), the % of females in STEM tends to fall.

![Figure\_1-3.png](https://nintil.com/old_assets/2017/08/24-5.png)

As before, let's break it down by wealth:

![Figure\_1-4.png](https://nintil.com/old_assets/2017/08/24-6.png)

Here rich means the same as before. Those dots above 40 at the right of the chart are oil-rich arab countries like Qatar or Saudi Arabia.

![Figure\_2-1.png](https://nintil.com/old_assets/2017/08/24-7.png)

That one rich country (The others are mostly oil-rich arab states) that appears above 40 is Brunei Darussalam in 2006. Let's see how it has evolved:

![Figure\_1-5.png](https://nintil.com/old_assets/2017/08/24-8.png)

Clearly an outlier. Brunei has a small population, 350.000 people in 2006. So we establish the claim that: **In no developed country, since 1999, the % of women in STEM has been above 40%**, if we assume parity in access to university. Can we generalise to other countries? (This is STEM, not engineering, note)

Let's look at all the countries that were above 40% in any given year. If we do that, we get this full list: Burundi, Liberia, Sierra Leone, Cambodia, Gambia, Eritrea, Bangladesh, Kyrgyzstan, Angola, Myanmar, Palestine, Samoa, India, Swaziland, Morocco, Grenada, Algeria, Azerbaijan, Panama, Oman, Saudi Arabia, Brunei Darussalam, Qatar.

![Figure\_1-6.png](https://nintil.com/old_assets/2017/08/24-9.png)

Some of these countries have only a few datapoints. If we discard those with just two observations or less (Algeria, Bangladesh, Burundi, Gambia, Grenada, Liberia, Myanmar, Samoa, and Sierra Leone), and on top of that we discard the ones that are under 40% on average, we are left with Saudi Arabia, India, and Swaziland. They look like this:

![Figure\_1-7.png](https://nintil.com/old_assets/2017/08/24-10.png)

Now these need an explanation. India was discussed in a previous post of mine, as was Saudi Arabia.  Swaziland has a population of about 1 million people, and I cannot really find any data for an increase in stereotypes against women in that period. It could be noise, or it could be an actual trend. This sounds like we could have a look at the countries with the most number of data (Over 15 datapoints).

![Figure\_1b](https://nintil.com/old_assets/2017/08/24-11.png)

What happened in the UK! OECD data also says that there was [a jump of 9 points between those two years](https://stats.oecd.org/Index.aspx?DataSetCode=EAG_GRAD_ENTR_FIELD#) in the science, mathematics, and computing subfield. I would think that this is due to a redefinition of the data, but go guess.

Now, within the developed countries, how variable is each of it, compared to each other:

![Figure\_2-2.png](https://nintil.com/old_assets/2017/08/24-12.png)

The countries are sorted by increasing population. Again there is decreasing variance with population (blue line is both mean and median, as they are almost the same here)

![Figure\_1-8.png](https://nintil.com/old_assets/2017/08/24-13.png)

If we accept that much of the variance within countries is noise, then we can return once more to the full view,

![Figure\_1-9.png](https://nintil.com/old_assets/2017/08/24-14.png)

Again, India(!). India tells us something that we knew: That there is a relation between GDPpc and % of females in STEM. India should be good estimate of what the % should be for a country of its GDP, given its population.

Finally, we can return to the relationship between development (as proxi'ed by GDP) and % of women in STEM. The fit is excluding those countries with natural resource rents over 20% (value that is in the colorbar).

![Figure\_1-12.png](https://nintil.com/old_assets/2017/08/24-15.png)

Lest you think that maybe I'm obviating a %muslim-women in STEM correlation:

![Figure\_1-14.png](https://nintil.com/old_assets/2017/08/24-16.png)

But this could be driven by a muslim-GDP correlation, which in turn is correlated with females in STEM.

![Figure\_1-15.png](https://nintil.com/old_assets/2017/08/24-17.png)

To stop here and sum up: the point is that many of these (and others) variables are correlated. I have included fm\_STEM, and fm\_EDU, the women to men ratio in STEM and in education and healthcare, equality\_index, the OECD SIGI index of women discrimination, Nrrent, the fraction of GDP that comes from natural resources, and IQ, collected from the most up to date and cross-nationally valid dataset ([NIQ-dataset](https://www.researchgate.net/project/Worlds-IQ).

![Figure\_1](https://nintil.com/old_assets/2017/08/24-18.png)

One approach to this would be to do multivariate OLS, but this makes it difficult to deal with collinearity. We could instead do OLS on some variables, then take the residuals and do more OLS to control for causally antecedent variables. But that would take time, and here we may have causality running in many directions. For this I chose to model the causal influence of the variables using SEM (Structural Equation Modeling). With this, one defines the set of relations that one expects the data to have, both cause and correlations, and the solver tries to find the coefficients that make the model most resemble the observed values.

The model in particular that I'm using is:

> -   Development (latent variable) explains life expectancy, log(child mortality), log(GDP per capita) and IQ.
> -   Share of muslims covaries with development (direction not specified)
> -   Equality, as measured by [OECD's SIGI](http://www.genderindex.org/)  depends on development and share of muslims
> -   The % of women in tertiary education  depends on development and equality
> -   The ratio of % of all women in tertiary education who choose STEM to the % of all men in tertiary education who choose STEM (female\_over\_male) depends on development and equality. A preference ratio of 1 means that men and women choose STEM in equal proportions. Higher ratios mean that women are more likely than men to choose STEM.
> -   GDP per capita is corrected by substracting the share of natural resources from total GDP.

I have 3023 observations, but many empty items. For example, only 8.4% of the entries (two years) have an equality index, and 20-25% of all rows have the female\_over\_male variable. The model is estimated using[maximum likelihood with robust standard errors](http://lavaan.ugent.be/tutorial/est.html) , and full information estimation so that for each pair of variables, all rows with entries are considered. The correlations are similar if I only use complete rows. The result, considering only statistically significant (p<0.05) correlations is:

![Captura de pantalla de 2017-08-20 17-11-34.png](https://nintil.com/old_assets/2017/08/24-19.png)

The values there are standardised coefficients (read as: an increase of 1 standard deviation in variable A leads to an increase of X standard deviations in variable B), and the values above the components of development are the loadings of those variables into the development latent variable.

In other words, development in and of itself does not predict changes in the preference ratio. But equality does, and it does predict it negatively. For muslims, the reason women are overrepresented there does not have to do with an effect of % muslim itself, but an indirect effect via less equality.

### Engineering

The discussion above is for STEM. But what about engineering in particular? Engineering tends to feature bigger gaps than STEM in general, and so studying how the gender gap in engineering differs between countries. I also have data for that, and so I offer a few plots.

First, the relation between STEM and engineering (using my hypothetical variable):

![engi.png](https://nintil.com/old_assets/2017/08/24-20.png)

This means that, as expected, there is a relationship between both, but that it is almost never the case that the % female in engineering in an observation is greater than % in STEM. This may indicate that within STEM, everywhere in the world, women are clustering in other subjects. Let's define a Engineering /STEM ratio and see how it behaves wrt GDP,

![stemengi.png](https://nintil.com/old_assets/2017/08/24-21.png)

Including the hypothetical % of female in Engineering in the structured model above yields a significant correlation only for development (d=-.26), and this effect is direct, not mediated by equality, unlike for the % of female in STEM.

Full data for this section can be found [here](https://pastebin.ubuntu.com/25378749/) in csv format. There are more variables in the dataset, feel free to ask in the comments if you really want to know.

### Conclusion: Countries to pay attention to

The key takeaway from this post, in my opinion, is a list of countries that do not match the trend,  in some sense. These countries should be studied to understand the causes of the differences between them.

One group of countries is Saudi Arabia and the United Arab Emirates. These are low-equality, high income, high % muslim countries that have a relatively high share of women in STEM.

A second group of countries is Switzerland and Netherlands, rich western countries high in equality, and with relatively low %. (We may add in Macao here)

A third group, Italy, Malaysia, and India,  countries that have a relatively high share of women in STEM, especially Malaysia. ([Malaysia](https://www.digitalnewsasia.com/insights/the-gender-gap-in-stem), like India, however, has a smaller share of women in STEM related industries. Where are those women going?)

A fourth group, low income countries with few women in STEM, like Chad, Niger, or Iraq.

Finally, a sixth group with reference countries that we may take as developed, but with different cultures: Sweden, Denmark, Japan, South Korea, the UK, and the US.

## Bonus: Sociological explanations

A bit unrelated to the main topic of the post. I thought of splitting this post in two, but in the end I decided to go for one.

Sociologists are not blind. They know there are international differences in female/male choices of major, but they tend to propose explanations that are culture-based. I copy some bits from a recent review, by [Maria Charles (2011)](http://www.annualreviews.org/doi/abs/10.1146/annurev.soc.012809.102548), below. The review deals with more things than women in STEM, but I focus on what interests us here.

> Although some degree programs—most no tably business, law, and medicine—have integrated over time, gender distributions across fields have changed rather little since the 1980s in the United States and other industrial countries (Bradley 2000; Jacobs 1995, 2003; Xie &
> 
> Shauman 2003, England & Li 2006). Moreover, cross-national analyses reveal weak, sometimes negative relationships between socioeconomic modernization and female representation in the mathematical and technical fields that are so strongly male-typed in the industrial West to- day (Charles & Bradley 2002, 2009; Charles 2011). For instance, women are about as well represented in computer science programs in Turkey as in Sweden (Charles & Bradley 2006; see also Lagesen 2008 on Malaysia).
> 
> And in engineering programs, women’s presence is, in fact, stronger in developing than in advanced industrial societies. Figure 2 depicts this relationship for a sample of 44 countries. It shows a negative correlation (−0.48) between GDP per capita and female representation in engineering. \[..\]
> 
> The patterns described above beg two questions, which are considered in the following sections: Why do we see equalization on some indicators but not others? And why are some curricular and career outcomes more gender-typed in advanced industrial countries?
> 
> Evidence is growing, in fact, that some of the same structural and cultural forces that have facilitated female access to labor markets and educational systems have also contributed to sex segregation within these institutions.
> 
> In higher education, simultaneous historical pressures toward increased female participation and sex segregation are clearly evident in policy agendas and official discourse. Since the middle of the twentieth century, national governments and international organizations have endeavored to increase female enrollment in colleges and universities by implementing programmatic changes designed around prevailing gender stereotypes (Charles & Bradley 2002, Bradley & Charles 2004). In 1953, the following resolution was directed toward national ministries of education by the United Nations Educational, Scientific and Cultural Organiza- tion. UNESCO (1953, p. 263) resolved
> 
> that to facilitate women’s access to higher education and the use of their abilities for the greatest good of society, university studies permit women to specialize in fields particularly suited to feminine aptitudes and assure them more adequate training for the new careers now being opened up to them.

That may have been the case in 1953. But is it the case in recent decades? If anything, there is now a strong push towards equality, especially by increasing the share of women in STEM. I suppose that the reason why there are no calls for equalisation in female-heavy fields is that those fields tend to be less lucrative or useful, and so from the perspective of a pure profit maximiser without any taste, irrational. If one assumes that tastes are equal, then men are doing the right thing, but women are not due to stereotypes, and so it is them, not men, who have to be nudged towards STEM.

> WHY ARE CAREER OUTCOMES MORE GENDER-TYPED IN ADVANCED INDUSTRIAL CONTEXTS? One obvious response to this question is that people who live in affluent societies can more easily absorb the material costs associated with  pursuit of female-typed occupational and educational careers. In other words, the economic capacity to indulge stereotypically female preferences is greater in advanced industrial contexts.\[This interpretation is consistent with findings from individual-level research in the United States. For example, American girls who come from high-income families are less likely to study math and science in college ([Ma 2009](http://www.jstor.org/stable/10.1525/sop.2009.52.2.211?seq=1#page_scan_tab_contents)). {But note that there are also contrary findings, this deserves future exploration (Author note}\] \*\*Some scholars object to explanations of \*\***sex segregation that reference women’s preferences and choices because this line of inquiry seems to divert attention away from the structural and cultural causes of inequality.** But acknowledging gender-differentiated aspiration does not blame the victim unless preferences are considered in isolation from the social contexts in which they emerge. **This explanation**  **does not require that gendered dispositions be innate or even deeply internalized. As discussed above, educational and career aspirations are shaped by beliefs about one’s own competencies and affinities, beliefs about the masculine or feminine task content of specific social positions, and beliefs about the categories of persons who are appropriate  incumbents for such positions.**

> **These beliefs constitute the cultural environment in which life choices are made, and they may be a consequence, not only a cause, of sex segregation.**
> 
> Aspirations are also shaped by more general beliefs about the nature and purpose of educational and occupational pursuits. In advanced industrial societies, parents and educators commonly exhort young people, and perhaps girls in particular, to “follow their passions” and to study and work in fields that will allow them to realize their “true selves.” Although these so-called postmaterialist values are spreading globally, they are today most clearly evident in affluent, late-modern societies (Inglehart 1997, Inglehart & Baker 2000, Meyer & Jepperson 2000, Beck & Beck-Gernsheim 2001). \*\*Curricular and career choices become more than practical economic decisions in these contexts; they also represent acts of identity construction and self-affirmation. Because gender remains such a fundamental axis of human identity, individual self-expression often results in “expression of \*\*\*\*gendered selves\*\*” (Charles & Bradley 2009).
> 
> **Normative mandates for self expression intensify sex segregation by supporting the development and realization of culturally masculine or feminine aspirations. In the process, stereotypes and gender identities are entrenched, gender labeling of educational and occupational fields is reinforced, and sex segregation is legitimized**. For example, American girls who aim to “study what they love” are unlikely to consider male-labeled science, engineering, or technical fields, despite the relative material security provided by such degrees. The nonalignment of mathematics and science with female gender identities may even generate attitudinal aversion in these postmaterialist contexts. New evidence suggests that boys’ and girls’ attitudes toward mathematics indeed diverge more in developed than in developing and transitional societies (Charles & Bradley 2009).
> 
> High levels of sex segregation in advanced industrial countries are also supported by structural features of labor markets and educational systems, as discussed above. Such features include large service sectors and diversified systems of higher education. These have appeared worldwide, but the transformations began earlier and have so far been most pronounced inaffluent European and North American countries (Benavot 2006).

In other words: Sociology says that  women do prefer more STEM. But in developing countries there is a smaller gap due to economic factors. And preferences are socially constructed, so people "freely choosing" means "people freely expressing what society has taught them to express".

Another article ([Mann & DiPrete, 2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3791309/)) discussed the gap, and it mentions many of the key points already discussed in this blog: that women show a stronger preference for liberal arts, that high ability women tend to have higher verbal ability than high ability men. The paper neither affirms nor rejects a possible biological origin of those differences in preferences.

[Bradley & Charles (2009)](http://cedar.wwu.edu/cgi/viewcontent.cgi?article=1002&context=sociology_facpubs) specifically try to address cause of segregation by field by analysing 44 countries.

> We argue that conventional evolutionary models of women’s status cannot provide a satisfactory account of cross-national and historical variability in sex segregation by field of study because **they underestimate the enduring cultural force of gender-essentialist ideology** (i.e., cultural beliefs in fundamental and innate gender differences), which has proven to be extremely influential in shaping life experiences, expectations, and aspirations, even in the most liberal egalitarian societies (Fenstermaker and West 2002; Charles and Grusky 2004; Correll 2004; Ridgeway 2006). **The segregative effect of gender-essentialist beliefs is intensified, moreover, by a strong Western cultural emphasis on individual self-expression and self-realization** that has been diffusing worldwide since World War II (Meyer and Jepperson 2000; Frank and Meyer 2007) and is today most clearly evident in affluent late-modern societies (Inglehart 1997; Beck and Beck-Gernsheim 2001). \[...\] **Contrary to the predictions of many evolutionary theorists {In a sociological sense}, we find a general tendency for greater segregation of academic fields in more economically developed contexts.** \[...\] Sex segregation by field of study derives its legitimacy not from norms of male primacy, but from its seeming homology with gender-essentialist beliefs and dispositions. It is therefore less politically and socially contested than are many types of vertical inequality, and it is easily reconciled with self-expressive value systems. In fact, this type of sex segregation may not even constitute “inequality” in Inglehart’s strict sense. \[...\] In sum, the results of our pooled 44-country analyses suggest that (1) the association of sex segregation with economic development revealed in figure 1 is at least partly attributable to cross-national differences in economic structure (i.e., postindustrialism) and (2) aggregate-level gender differentiation in affinity for mathematics does help predict female representation in math-intensive fields (especially in math and the natural sciences). \[...\] Consistent with our argument positing positive feedback effects between sex segregation and gendered curricular affinities, the descriptive statistics displayed in table 2 reveal a significantly **larger gender gap in expressed affinity for mathematics in advanced industrial than in developing/transitional societies** (t-ratio p 2.15; P p .037). In the former group, the percentage of girls who claim to “like math” or “like math a lot” is on average 5.42 points lower than the percentage of boys who make this claim. This gap is only 2.05 points in developing/transitional societies. As can be seen in table 5, large, statistically significant differences between the country groups also exist on related indicators, including gender gaps in professed affinity for science (“I like science”) and aspirations for math-related careers (“I would like to work in a job involving mathematics”). These gaps do not correspond to simple differences in girls’ mathematical achievement; in fact, table 2 shows that the math achievement gap is slightly smaller in the more economically developed countries.26 The mean differences shown in table 5 provide preliminary evidence that national prosperity and the rise of self-expressive value systems promote development (or expression) of gender-differentiated curricular dispositions. The causal processes underlying cross-national variability in such dispositions among women and men is an important area for future research.

> In developing and transitional societies, multivariate models confirm a net tendency for greater segregation as per capita GDP increases. We suggest that economic modernization does not have the degendering effect that modernization scholars expect because realization of any preference for lower-paid female-typed careers is more constrained (and less culturally legitimate) under conditions of greater scarcity.37 From this perspective, increasing sex segregation may reflect decreasing normative and material pressures for university-educated persons, regardless of gender, to obtain credentials in lucrative scientific and technical fields and to build human capital in fields (like engineering and the natural sciences) that are deemed crucial for national development and competitiveness in the international economy. **In other words, instrumental goals of material security and national economic development are decreasingly central to curricular decisions (by students themselves, parents, family members, and educational gatekeepers) as national prosperity grows**. But this segregative GDP effect is measurable only up to a point. Among advanced industrial societies, further increases in national prosperity (GDP) do not exert a segregative effect on curricular distributions because existential security is now widely taken for granted at the mass-societal level. Strongly diversified university curricula are commonly celebrated as instruments for individual self-realization in these contexts. Sacrificing material rewards in order to pursue a passion (or presumed passion) is more culturally legitimate—especially for women, who are generally not expected to be primary breadwinners. As individuals seek to express their essential (male and female) selves, the gender labeling of academic fields intensifies, and distributions across these fields become more closely aligned with gender-specific curricular dispositions.**Crossnational variability in girls’ relative mathematical affinities (rather than variability in economic capacities to realize such affinities) thus becomes a decisive determinant of gender distributions across curricular fields in advanced industrial segregation regimes.**

Finally, as to ways of reducing the gap, they suggest, among other things, to force girls to study more maths and science related courses

> Previous comparative and historical research provides evidence that**limitations on curricular choice—either through universal requirements for math and science coursework throughout secondary school or through stronger reliance on merit-based curricular placement—results in stronger representation of women in scientific and technical fields** and a weaker influence of peers on students’ educational aspirations (OECD 1986; Barinaga 1994; Hanson, Schaub, and Baker 1996; Buchmann and Dalton 2002).41 **Such restrictive educational policies are strongly at odds with practices in the United States and many other advanced industrial societies, where high-level math and science are optional and “freedom of choice” is widely celebrated** (Frank and Meyer 2002; Schwartz 2004). But social pressures to conform to gender norms often peak during adolescence (Entwisle and Greenberger 1972; Gaskell 1985). **Allowing girls to opt out of science and mathematics courses during their teenage years increases sex segregation in higher education by limiting subsequent curricular options** (Charles et al. 2001; Xie and Shauman 2003), **by reducing the likelihood that a stereotype-eroding critical mass of high-achieving girls will be attained in male-typed programs** (RiegleCrumb, Farkas, and Muller 2006; Frank et al. 2008), and by reinforcing the gender labeling of scientific and mathematical activities.42 **While more universal secondary school requirements may strike some as antidemocratic, this concern must be balanced against the possibility that girls’ and boys’ seemingly free choices are constrained by taken-for-granted assumptions about what they will like and what they are good at and by the social sanctions that they may anticipate should they elect to pursue gender-atypical educational and career paths.**

The effects of forced choice for women are worth studying.

And they discuss, but reject, segregated schools on what seems to be aversion to the reasons given by many proponents of it. "We oppose X because we think Y is false, and people who think Y is true favour X".

> Enrollment in single-sex secondary schools or classrooms has been suggested as one means of reducing the salience of gender and the frequency of gender performances during the formative adolescent years (Thompson 2003). Although some beneficial psychological and pedagogic effects have been documented (Lee and Marks 1990; Shapka and Keating 2003), any large-scale initiative toward single-sex education would undoubtedly bring a host of unintended consequences (e.g., Datnow, Hubbard, and Conchas 2001). **More important, we note that the philosophical rationale for single-sex educational policy initiatives is often based upon those very same gender-essentialist presumptions that have helped give rise to the segregation of scientific and technical fields in the first place (Hoff Sommers 2000; Gurian 2002).** Intentional sex segregation of public schools or high school mathematics classes strikes us as an unrealistic policy option.

Thus, sociologists, again, are not blind: they

_Ricón, José Luis, “Women in STEM: a cross-country view”, Nintil (2017-08-24), available at https://nintil.com/women-in-stem-a-cross-country-view/._