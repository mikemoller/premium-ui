---
name: ux-psychology
description: Which "laws of UX" hold up and how to use them honestly — peak–end rule, response-time limits and the Doherty threshold, goal-gradient and endowed progress, aesthetic–usability effect, Zeigarnik vs Ovsiankina, onboarding tutorials vs contextual help — each graded by the strength of its primary evidence. Use when a design decision leans on psychology, when someone cites a UX law, when designing progress, onboarding, celebrations or end-of-flow moments, or when reviewing claims in design docs and marketing.
---

# UX psychology, graded by evidence

Sites like [Laws of UX](https://lawsofux.com/) are useful indexes, but their pages range from well sourced to no primary citation at all. Before a "law" drives a design decision, check what the original study measured and whether it replicated. This skill does that for the ones that come up most.

| Effect | Evidence | Use it for |
| --- | --- | --- |
| Response-time limits (0.1 / 1 / 10 s) | Strong, long-standing | Speed budgets, feedback |
| Peak–end rule | Strong in lab and clinic; analogy in software | Fixing the worst moment, designing endings |
| Goal-gradient, endowed progress | Good (field experiments) | Progress bars, checklists |
| Aesthetic–usability effect | Mixed, runs both ways | A reason to polish, never a reason to skip testing |
| Doherty threshold (400 ms) | The number isn't in the source | Use 100 ms and INP ≤ 200 ms instead |
| Zeigarnik (unfinished tasks remembered better) | Fails to replicate | Don't rely on it; Ovsiankina is the useful one |

## Response-time limits — strong

- 0.1 s feels instant; 1 s keeps the train of thought; 10 s is the limit of attention ([Nielsen](https://www.nngroup.com/articles/response-times-3-important-limits/), from Miller 1968 and Card et al. 1991). The current web measure is INP, "good" at ≤ 200 ms at the 75th percentile ([web.dev](https://web.dev/articles/inp)).
- **Doherty threshold:** Laws of UX says productivity soars under 400 ms. The source, Doherty & Thadani's 1982 IBM report, measured mainframe terminal work and argued for sub-second response; programmers did 180 transactions an hour at 3 s and 371 at 0.3 s. **400 ms doesn't appear in it** ([reproduction](https://jlelliotton.blogspot.com/p/the-economic-value-of-rapid-response.html)). Cite it for "faster is disproportionately more productive", not for the number.
- How to apply: the `perceived-performance` skill.

## Peak–end rule — strong, with a caveat for software

**What the studies show.** People judge a past experience mostly by its most intense moment and its end, and barely by how long it lasted.

- Cold water: a 60 s trial in 14 °C water vs the same 60 s plus 30 s while the water warmed slightly. Most people chose to repeat the *longer* one ([Kahneman, Fredrickson, Schreiber & Redelmeier 1993](https://www.psychologicalscience.org/journals/psychological-science/j.1467-9280.1993.tb00589.x/)). Retellings quote 80% or 69%; the exact share is unverified, so don't quote it.
- Film clips: duration barely moved remembered ratings ([Fredrickson & Kahneman 1993](https://bear.warrington.ufl.edu/brenner/mar7588/Papers/fredr-kahneman-jpsp1993.pdf)).
- Colonoscopy: remembered pain tracked the worst and last moments ([Redelmeier & Kahneman 1996](https://pubmed.ncbi.nlm.nih.gov/8857625/)). A randomised trial with 682 patients: adding a short, milder ending made the whole procedure remembered as less unpleasant, and slightly more patients came back ([Redelmeier, Katz & Kahneman 2003](https://pubmed.ncbi.nlm.nih.gov/12855328/)).
- Meta-analysis of 174 effect sizes: peak and end effects are large (r ≈ 0.58) and robust; duration's effect is about zero ([Alaybek et al. 2022](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html)).
- Limits: weaker for long, unbounded experiences such as holidays or hotel stays; it needs a clear start and end.

**The caveat.** No study tests software completion screens, confetti or retention directly. Applying it to apps is a plausible inference ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)), and fits best for **bounded flows with a clear end**: checkout, onboarding, finishing a project, a workout or a long task.

**How to apply it**

1. **Fix the worst moment first.** A negative peak weighs as much as a positive one: lost work, a crash at the critical moment, a confusing error. Removing it beats adding delight anywhere.
2. **Find the moments that matter.** For each bounded flow, name its peak (the moment of most value or most risk) and its end. The `aha-moment` skill walks through this.
3. **Give bounded flows a calm, clear end.** Say what was done, and stop. TurboTax's completion screen has no upsell. Don't end on an exit-intent "please don't go" pop-up or a prompt for a review ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)).
4. **Keep routine actions quiet.** People expect success; confirm only significant completions ([Apple, Feedback](https://developer.apple.com/design/human-interface-guidelines/feedback)). Celebration on every action stops meaning anything.
5. **Make celebration rare and switchable.** Asana's creatures appear only occasionally after a run of completed tasks and can be turned off ([Asana](https://blog.asana.com/2016/03/new-celebrations)).
6. **Treat outages and errors as endings.** Explain what happened, give an estimate, don't blame.

## Goal-gradient and endowed progress — good

- People speed up as a goal gets closer. Coffee-card customers bought more often as the free coffee neared, and the pace reset after each reward ([Kivetz, Urminsky & Zheng 2006](https://business.columbia.edu/sites/default/files-efs/pubfiles/1200/goalgradient.pdf)). The animal version goes back to Hull (1932).
- Framing a task as already partly done raises completion: a 12-stamp coffee card with 2 stamps given beat a plain 10-stamp card ([Kivetz et al.](https://business.columbia.edu/sites/default/files-efs/pubfiles/1200/goalgradient.pdf)), and a car-wash card of 8 stamps framed as 10 with 2 done was finished more often ([Nunes & Drèze 2006](https://ideas.repec.org/a/oup/jconrs/v32y2006i4p504-512.html)). The widely quoted 19% vs 34% is from a secondary post, unverified.
- How to apply: show real progress toward a done state (a setup checklist, running totals that update instantly). Count steps the person has genuinely completed, including ones done implicitly (account created). **Never fake progress**; it's a dark pattern and people notice.
- Progress bars that speed up toward the end feel faster than linear ones ([Harrison et al. 2007](https://www.chrisharrison.net/index.php/Research/ProgressBars)). Apple warns that very uneven reporting feels deceptive ([Apple](https://developer.apple.com/design/human-interface-guidelines/progress-indicators)).

## Aesthetic–usability effect — mixed

- Kurosu & Kashimura rated 26 ATM layouts: perceived ease of use tracked looks more than actual usability ([CHI '95](https://chi1995.chistatic.hosting.acm.org/proceedings/shortppr/mk_bdy.htm)). Tractinsky replicated it and later found the link survives real use ([Tractinsky, Katz & Ikar 2000](https://academic.oup.com/iwc/article/13/2/127/898608)).
- But Tuch et al. found the reverse with 80 people and an online shop: poor usability made the design look *worse*, and looks didn't change perceived usability ([Tuch et al. 2012](https://doi.org/10.1016/j.chb.2012.03.024)).
- How to apply: polish earns goodwill, and friction destroys it. In testing, polish can hide problems, because people praise the looks after struggling ([NN/g](https://www.nngroup.com/articles/aesthetic-usability-effect/)). Measure what people do, not only what they rate.

## Zeigarnik vs Ovsiankina — don't rely on Zeigarnik

- The claim that interrupted tasks are *remembered* better largely fails to replicate; the 2025 meta-analysis found no general effect. The **Ovsiankina effect**, the urge to go back and finish an interrupted task, does look general ([Ghibellini & Meier 2025](https://ideas.repec.org/a/pal/palcom/v12y2025i1d10.1057_s41599-025-05000-w.html)).
- How to apply: if you show an unfinished thing to pull people back ("Resume your draft"), the justification is Ovsiankina. Make resuming one tap, and don't nag.

## Onboarding — contextual help beats tutorials

- Up-front walkthroughs are "dramatically overused": people skip them, forget them, and do no better afterwards. Prefer help that appears at first use, can be dismissed and found again, and isn't spent on standard controls ([NN/g](https://www.nngroup.com/articles/onboarding-tutorials/)).
- Get people to the product's core value fast; the `aha-moment` skill covers finding it.

## Checking a new claim

1. Find the original study, not the summary. What did it actually measure, on whom, in what setting?
2. Has it replicated? Look for a meta-analysis.
3. Is the number in the source, or added later?
4. How far is the setting from yours (mainframes, cold water, ATMs vs a phone app)?
5. Say how strong the evidence is when you use it.

The source notes are in [references/sources.md](references/sources.md).

**Finish with `taste`.** Any screen or component you build or change with this skill gets the `taste` pass before you call it done.
