In a [post](https://nintil.com/alzheimers-clinical-trials) in my [Alzheimer's](https://nintil.com/categories/alzheimer-s-disease/) series I discussed the not-so-promising monoclonal antibodies against amyloid beta. There are a few other therapies one could discuss, especially tau antibodies, but first I wanted to examine a particular one that has nothing to do with the most popular approaches currently on their way to the clinic. This is a relatively shallow examination of the topic, focused on the question of whether it works, and less so on how it works.

That intervention is [GENUS](https://www.alzforum.org/therapeutics/genus), or Gamma Entrainment Using Sensory Stimuli, being developed by [Cognito](https://cognitotx.com/about-us) Therapeutics, a spinout of the Boyden and Tsai labs. GENUS works by exposing the patient to blinking lights and/or ultrasound at 40Hz. This sounds relatively simple, safe, cheap, and non-invasive. But does it work?

Cognito has ran a number of tests in humans. There was a promising study ([Overture](https://www.alzforum.org/news/conference-coverage/does-synchronizing-brain-waves-bring-harmony#Overture) study) in March 2021, with 46 patients receving the treatment (light and sound at 40Hz) and 28 a sham (which the company didn't say what it was, but presumably it was the device set either at a different fequency, or without blinking). Many patients in this trial abandoned it because of the time commitment: one hour a day. The results look perhaps interesting but not particularly conclusive, the 5 cognitive measures used showed disparate results, hippocampus volume declined in similar ways, and total brain volume declined more slowly on the therapy, as measured by MRI. These are all standardized estimates, which I wonder if they are the raw values or values adjusted for covariates, and if so whether this adjustment was correctly done. Another study that was started at around the same time as Overture ([Etude](https://clinicaltrials.gov/ct2/show/NCT03661034) study \[French speakers will be amused\]) was completed in 2019 but the results never published which is suspect. They tried a variety of dosing, 1 hour per day, 2 1 hour sessions per day, 30 minutes per day twice a day, 2 hour sessions twice a day. This study brings to mind what happened with Aducanumab where one trial was kind of positive and another wasn't. Imagine if only the positive one had been published, then the evidence would have probably seemed better. Was this the same with Cognito? I don't know.

Part of the Overture study was analyzed in Cimenser et al. ([2021](https://www.frontiersin.org/articles/10.3389/fnsys.2021.746859/full)). The number of patients in the analysis is quite low (14 in treatment, 8 in sham) so drawing strong conclusions is premature, but looking at cognitive decline (by the ADCS-ADL [scale](http://medavante.vo.llnwd.net/v1/External/Merck/Prodromal/ADCS-MCI-ADLI/English/Tutorial/MK-8931%20019-00%20ADCS-MCI-ADL%20Activity%20Daily%20Liv%20US%20\(English\)_Training.pdf)) they show a statistically significant worsening in the sham group (all patients declined by 3 points on average) and improvement in the treatment group measured around a year after the initial assessment. In the treatment group, 9 out of 14 patients improved or maintained the score. This is a difference between control and intervention, after a year, of about 4 points in that scale.

A reason to be skeptical is that they didn't analyze the full dataset, and they did not look at all the endpoints they measured. A [press release](https://www.alzforum.org/news/conference-coverage/does-synchronizing-brain-waves-bring-harmony#Overture) from Cognito did look at other cognitive scores besides ADCS-ADL in a larger fraction of the sample. Not all of those showed improvement, in particular not their chosen primary endpoint (MADCOMS). I assume this refers to modified ADCOMS (They don't seay how it was modified), and [ADCOMS](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5013117/) is a recently proposed cognitve assessment tool that blemds MMSE, ADAS-Cog and CDR-SB according to how informative each of the sub-items in each is to predict an outcome (aMCI, amnestic mild cognitive impairment). Interestingly, the most heavily weighted items are the CDR-SB ones which may explain why it is so prevalent and preferred to MMSE or ADAS-cog.

![image-20221024145901051](https://nintil.com/images/2022-10-24-genus-alzheimers/image-20221024145901051.png)

There might be something to ADCS-ADL in particular (unless it's just noise). Given the p-value, one can't make the result go away by means of controlling for multiple comparisons (e.g. Bonferroni, in this case setting the cutoff p-value to 0.01; 0.0009 is still well below). One difference between that metric and the others is that ADCS-ADL involves asking someone related to the patient about how functioning the patient is in their daily life, as opposed to a doctor making the assessment directly based on a test they can witness. At first I thought that it might be that, say, because the patients were one hour a day on therapy, the related individuals interacted less with the patients and hence saw fewer examples of AD-impaired behaviors. But so was the sham group so this can't be the reason. So it's either noise or the treatment is differentially affecting the subcomponents of the tests; if we had access to the raw data we could confirm this.

## How big are the effect sizes?

So let's suppose this works. How does it compare to monoclonal antibody therapy? If we take CDR-SB, for which I have the values for the various trials in the corresponding post, and we take the point estimate from Overture at face value (0.43 points in favor of the intervention) then it compares quite favorably to aducanumab. This is in the same ballpark as the highest effect size reported in the EMERGE and ENGAGE studies (In particular, the high dose EMERGE group).

That might not be good enough though, per Andrews et al. ([2019](https://alz-journals.onlinelibrary.wiley.com/doi/pdfdirect/10.1016/j.trci.2019.06.005)) one needs a 1-2 point improvement in CDR-SB for clinical benefit. The MMSE effect on the other hand would qualify as clinically useful.

## Non-GENUS studies

In principle, anyone could pulse 40 Hz sensory inputs at people and run a clinical trial. Is there evidence from other evidence in humans besides the trials ran by Cognito?

Looking for "big" (>100 patients) trials, I was able to find the [NeuroRX](https://clinicaltrials.gov/ct2/show/NCT03484143?term=gamma&cond=Alzheimer+Disease&draw=3&rank=2) Gamma - Pivotal phase trial, a 228-patient test of [this](https://www.vielight.com/devices/vielight-neuro-gamma/) device on AD. The study was completed in May 2022, but no results have been posted yet. They also have a smaller, earlier, feasability study that I can't find data for.

[Another](https://clinicaltrials.gov/ct2/show/NCT03657745) trial (2000 participants) will be testing an phone app that blinks the screen at 40Hz. Results should be out late in 2023.

## In mice!

I've discussed the human data first, but there is a reason why those studies exist in the first place: It has been observed for some time that AD patients have a reduction in gamma activity as the disease progresses. At some point someone wondered if inducing gamma activity directly would do something good. Mice (or cells in culture) can be genetically engineered to directly drive gene expression based on light (optogenetics). Then, one can try to induce gamma activity by pulsing external stimuli at the right frequency.

One such paper is Iaccarino et al. ([2016](https://www.nature.com/articles/nature20587)) from the Boyden and Tsai labs in [5xFAD](https://www.alzforum.org/research-models/5xfad-b6sjl) mice; these mice produce an overwhelming amount of amyloid beta and present some neuronal loss and cognitive impairment (But no tau pathology). When putting them in a box with lights flickering at various frequencies, only the group exposed to 40Hz flickers saw a reduction in amyloid burden. The same was found in wildtype aged mice and a different model (APP/PS1). The proposed mechanism for this is the activation of microglia, which scoop up extracellular Ab. The effects shown below were transient, after 24 hours the control and treatment groups had similar levels of plaque.

![image-20221025132755873](https://nintil.com/images/2022-10-24-genus-alzheimers/image-20221025132755873.png)

Moreover they tried yet another model, this time [TauP301S](https://www.alzforum.org/research-models/tau-p301s-line-ps19) (No amyloid plaques, but presenting neurodegeneration and tau pathology). Seven days of 1 hour per day of 40Hz flicker reduced phosphorylated tau levels by 40%

A second paper (Martorell et al., [2019](https://www.cell.com/cell/fulltext/S0092-8674\(19\)30163-1?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867419301631%3Fshowall%3Dtrue)) from the same labs looked then at cognition and amyloid load in 6 month-old 5xFAD mice, using sound estimulation rather than light, again once a day. In two memory tasks (novel object recognition and novel object location), mice exposed to 40 Hz stimuli (but not random noise) showed improved performance relative to no stimulation:

![image-20221025135010819](https://nintil.com/images/2022-10-24-genus-alzheimers/image-20221025135010819.png)

In concordance with this, amyloid load was reduced in the same way: 40 Hz does it but not 8Hz, 80Hz, or random noise

![image-20221025135112673](https://nintil.com/images/2022-10-24-genus-alzheimers/image-20221025135112673.png)

This looks promising: The effects are as good or better than what others are showing with small molecule drugs, only that here the procedure is much cheaper and argueably safer, with perhaps some minor risk of epilepsy I guess.

There are a few more of these but as far as I could see the results point in the same direction, and I could not find studies contradicting or criticising these findings, so tentatively we can take them as true.

## Conclusion

Given the available evidence, gamma entrainment looks as promising as state of the art disease-modifying drugs (amyloid beta monoclonal antibodies) for Alzheimer's Disease, and with a markedly better safety profile. I agree with the [FDA's](https://www.businesswire.com/news/home/20210112005364/en/Cognito-Therapeutics-Receives-FDA-Breakthrough-Device-Designation-for-Next-Generation-Digital-Therapeutic-in-Alzheimer%E2%80%99s-Disease) decision to grant Cognito the designation of Breakthrough Device.

It remains to be seen if the treatment passes a cost-benefit analysis. It does not stop the disease, and the time commitment of one hour a day may outweigh the benefits of slower cognitive decline.

_Ricón, José Luis, “Blinking lights to slow down Alzheimer's?”, Nintil (2022-10-26), available at https://nintil.com/genus-alzheimers/._