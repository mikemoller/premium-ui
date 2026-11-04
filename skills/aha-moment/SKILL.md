---
name: aha-moment
description: Walk a developer through finding their product's aha moment (the moment a new user first gets its core value) and designing its peak and end with the peak–end rule — sizing the evidence, the job to be done, candidate aha events, setup/aha/habit, checking candidates with retention data or interviews, instrumenting the activation event, mapping time-to-value and emotional lows, and designing the peak and the final screen. Works with or without analytics. Use when someone asks what their aha moment, activation metric or "magic number" is, why new users don't stick, how to improve onboarding or time-to-value, or how to apply the peak–end rule to their product.
---

# Find the aha moment

The **aha moment** is when a new user first gets the product's core value and notices it. **Activation** is measuring it: the early action that best predicts that someone stays ([PostHog](https://posthog.com/handbook/growth/growth-engineering/per-product-activation)). The **peak–end rule** says people remember an experience mostly by its best or worst moment and its end ([Kahneman et al. 1993](https://www.psychologicalscience.org/journals/psychological-science/j.1467-9280.1993.tb00589.x/)). Put together: the aha should be the positive peak of the first session, and what happens right after it is the end people remember when deciding whether to come back. That link is an inference, not a tested result (see [Evidence](#evidence)).

## How to run this

This is a conversation, not a lookup. Go through the steps in order with the developer. At each step, ask the questions, wait for answers, and **agree the output before moving on**. Read the codebase yourself where it answers a question (routes, events already tracked, the database schema, onboarding screens) instead of asking. Don't invent users, numbers or quotes: if the developer doesn't know, mark it as a gap and say how to find out.

At the end, write the result to a file (by default `docs/aha-moment.md`, or wherever the repo keeps product docs) using the [template](#output-template).

Keep these questions in mind throughout:

- Is this **value**, or is it **setup**?
- Is the unit a **person** or a **team**?
- Would a **motivated user do this anyway**? (Then it may be a symptom, not a cause.)
- Could **most new users** reach it in their first session or first few days?
- What does the user see **right before and right after**?
- What is the **last thing** they see?

## Step 0 — Size the evidence

Ask:

- How many signups a week or month? How long has the product been live?
- What data exists: product analytics events, server logs, the database itself?
- Is the user an individual or a team? Who decides to keep using it?

Decide the track:

- **Quantitative** if there are a few hundred signups or more per monthly cohort and 30–90 days of history.
- **Qualitative first** otherwise, using whatever data exists only as a hint. With 10 signups a month, 6 of 10 retained vs 3 of 10 is noise.

(The cut-off is a judgement call, not from a source.)

Output: the track, and the unit of analysis (person or team/account).

## Step 1 — The job

Ask:

- What were people doing before they used this? What did they use instead?
- What pushed them to look for something new, and what pulled them to this product?
- What almost stopped them (worry about switching, the old habit)?
- Name three real users and what each one said.

These are the four forces of a switch: push and pull against anxiety and habit ([Moesta & Spiek, jobstobedone.org](https://jobstobedone.org/)). If the developer can reach users, suggest 5–10 short switch interviews with recent, retained users that reconstruct the timeline from first thought to the moment it clicked ([Moesta on switch interviews](https://www.intercom.com/blog/podcasts/bob-moesta-on-unpacking-customer-motivations-with-jobs-to-be-done)). The idea of the "job" itself is Christensen's ([HBR 2016](https://hbr.org/2016/09/know-your-customers-jobs-to-be-done)).

Output: one sentence — "When [situation], I want to [progress], so I can [outcome]."

## Step 2 — The core value, in users' words

If you can get about 40 responses from active users (used it at least twice in the last two weeks), suggest the product–market fit survey ([Superhuman](https://blog.superhuman.com/how-superhuman-built-an-engine-to-find-product-market-fit/)):

1. How would you feel if you could no longer use [product]? (Very / somewhat / not disappointed)
2. What type of people do you think would benefit most?
3. What is the main benefit you get from it?
4. How can we improve it for you?

The "main benefit" answers from the very-disappointed group are the aha in the users' own words. Sean Ellis's benchmark is 40% very disappointed ([Ellis interview](https://venturehacks.com/sean-ellis-interview)); it comes from his experience across startups, not a study.

Without a survey, ask the developer what their most enthusiastic users say they'd miss.

Output: the main benefit, and the segment that names it.

## Step 3 — Candidate aha events

Ask:

- At what moment does a new user first *get* that benefit, not just get ready for it?
- What is on screen at that moment?
- What did people who stayed do early on that people who left didn't?

Write 3–8 candidates as **"[object] [verb]ed, N times, within D days"**, for example "invite accepted, 1, within 7 days" or "report viewed with real data, 3, within 14 days".

**Move setup out.** Creating an account, completing a profile, connecting an integration and creating an empty workspace are setup, not value ([Elena Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation)). A product can have more than one aha: one per persona, or one for people who arrive by invite and another for people who start on their own.

Output: the shortlist, with setup steps listed separately.

## Step 4 — Setup, aha, habit

Write the three stages:

- **Setup:** the minimum steps before the first candidate is possible.
- **Aha:** the value event.
- **Habit:** the aha repeated at the product's natural rhythm. Verna's thresholds are a reasonable default: daily products active 3 of 5 weekdays over two weeks; weekly products 3 of the last 4 weeks; monthly products 3 of the last 4 months. Some products follow an event, not the calendar: used around each trip, each event, each tax season.

Output: a three-line model.

## Step 5 — Check the candidates

**Quantitative track.** For each candidate, compare retention for users who did it inside the early window with those who didn't ([Amplitude Compass](https://amplitude.com/docs/analytics/charts/compass/compass-find-inflection-metrics), [PostHog](https://posthog.com/newsletter/wtf-is-activation.md)):

- Include **every signup** in the cohort and measure the action only inside a fixed early window (first 3, 7 or 14 days), then retention after it. This avoids survivorship bias.
- Report for each: reach (% who did it), retention with and without, the lift, and the sample size or confidence interval.
- In a 2×2 table, look at both **PPV** (of those who did it, how many stayed) and **sensitivity** (of those who stayed, how many did it). Amplitude says correlations of 0.2–0.4 are typically good.
- Reject candidates almost nobody reaches or almost everybody does (under ~5% or over ~95% is a judgement call). PostHog: if 1% activate and 100% of them retain, the definition is too narrow.
- For team products, measure at the team or account level.

**Qualitative track.**

- Ask 5–10 retained users when they first felt it was worth using, as multiple choice from the shortlist plus "Other". Clustering supports a candidate; a split suggests different ahas per segment ([Userpilot](https://userpilot.com/blog/aha-moment/)).
- Ask people who left what they never got to.
- Do a fresh-account walkthrough and watch 3–5 new users.

Ask the developer, whichever track:

- Could motivated users simply do more of everything? Then the event marks commitment rather than causing it.
- Are we only looking at people who survived long enough to do it?

Output: one activation definition, written down as a **hypothesis**.

## Step 6 — Instrument it

Ask:

- Where in the code does the aha **complete** (not start)?
- Can it be recorded on the server? Ad blockers drop client-side events ([PostHog](https://posthog.com/newsletter/wtf-is-activation.md)).
- Which ids does it need: person, team/account?

Look in the codebase for the analytics setup already in use and follow its naming. Without one, name events object then verb, past tense ("project created", "invite accepted") ([PostHog](https://posthog.com/docs/product-analytics/capture-events)). Make sure a signup event exists so cohorts have a start date. Collect only what the activation query needs, and check it against the product's privacy policy.

Output: the event names, where they fire, the activation query (signup → aha within D days → still active at the retention check), and a baseline number once data arrives.

## Step 7 — Map the first session

Walk through a fresh account together (sign up with a new email). List every step from signup to the aha with the time and clicks it takes, and rate how each step feels from −2 to +2. The scale is a convention; marking peaks, low points and the end follows [NN/g, Peak–end rule](https://www.nngroup.com/articles/peak-end-rule/).

Ask:

- Which steps can be removed, deferred, pre-filled, or replaced by sample data or a template?
- Where is the worst moment? A single confusing step can sour the whole experience.
- What appears on screen right after the aha?

Don't strip everything: a few onboarding questions that personalise the path don't necessarily cost users ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation)).

Output: the step list, cuts ranked by effort, and the negative peaks to fix.

## Step 8 — Design the peak and the end

**The peak (the aha).**

- Does the user notice they've got value? Name it on screen, in the words from step 2.
- Celebrate in proportion: the first time, once, and quietly. Never on every repeat.

**The end** of the first session and of each key flow (see `ux-psychology`):

- What's the last thing the user sees? It should confirm what they achieved, in their terms, and give a reason or cue to come back.
- Remove from the end: upsells, "please don't go" prompts, a survey before the value has landed, and weak extras. A mediocre add-on at the end lowers a good experience (Do, Rupert & Wolford 2008, *Psychonomic Bulletin & Review* 15(1); [summary](https://mdschulkind.people.amherst.edu/psych233/readingresponse/dorupert.html)).
- Design this flow's error, empty and offline endings too; they become the memory when things go wrong.
- Keep the middle good. In the meta-analysis, the average of the whole experience predicted memory about as well as peak and end did ([Alaybek et al. 2022](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html)).

Output: the changes to the peak and the end, plus the error and empty states to improve. Hand the screen work to `interface-craft`, `perceived-performance` and `ui-motion`.

## Step 9 — Test and iterate

- Ship the path changes. A/B test if traffic allows; otherwise compare cohorts before and after and call it weak evidence. Only an experiment shows causation ([Amplitude](https://amplitude.com/docs/analytics/charts/compass/compass-find-inflection-metrics)).
- Re-check activation and retention after one retention window. If retention didn't move, the candidate was correlation: go back to step 3.
- Set a date to review the definition.

## About the famous examples

Use them as illustrations, never as benchmarks:

- **Slack, 2,000 messages:** first-party. Stewart Butterfield said teams that reached it were still using Slack 93% of the time, based on "which companies stuck with us" — no sample size or causal check, and a survivorship setup ([First Round Review](https://review.firstround.com/from-0-to-1b-slacks-founder-shares-their-epic-launch-strategy/)).
- **Facebook, 7 friends in 10 days:** traced to Chamath Palihapitiya's 2012 talk, but the exact words are unverified; Andrew Chen said it could as well have been "10 friends in 12 days" ([Mixpanel](https://mixpanel.com/blog/magic-numbers-are-an-illusion/)).
- **Twitter, follow ~30:** a book retelling, but the best account of the full method: retention cohorts, then correlation, then a causal check (the follow-back ratio), then phone calls to users ([Hacking Growth excerpt](https://mattermark.com/putting-it-all-together-how-josh-elman-identified-a-growth-driver-at-twitter/)).
- **Dropbox, Zoom, Canva, Loom and others:** vendor-blog stories with no first-party source.

The point of a magic number is a simple goal the team can aim at, not a law ([Mixpanel](https://mixpanel.com/blog/magic-numbers-are-an-illusion/)).

## Evidence

- **Peak–end** is strong in the lab and the clinic: cold water (1993), colonoscopy memories (1996), and a randomised trial of 682 patients in which a gentler ending improved memory and made a return visit more likely (odds ratio 1.41) ([Redelmeier, Katz & Kahneman 2003](https://pubmed.ncbi.nlm.nih.gov/12855328/)). A 2022 meta-analysis of 174 effect sizes found r ≈ 0.58.
- **Applying it to software** is analogy. No study tests completion screens or app retention directly.
- **The link between aha and peak** is this skill's inference, not a source's.
- **Activation methods** come from analytics vendors' own docs (Amplitude, PostHog) and operators (Verna, Vohra). The setup/aha/habit model is attributed to Reforge (Brian Balfour, Casey Winters) through secondary write-ups only.

Full notes and every source: [references/sources.md](references/sources.md).

## Output template

```markdown
# Aha moment: <product>

Date: <YYYY-MM-DD> · Review by: <date> · Track: quantitative | qualitative · Unit: person | team

## Job
When <situation>, I want to <progress>, so I can <outcome>.

## Core value (users' words)
<main benefit> — named by <segment>. Source: <survey / interviews / developer's view>.

## Setup → aha → habit
- Setup: <minimum steps>
- Aha: <object verbed, N times, within D days>  ← hypothesis
- Habit: <rhythm and threshold>

## Evidence
<reach, retention with/without, lift, sample size — or interview clustering>. Known weaknesses: <...>

## Instrumentation
- Events: <names, where they fire, server or client>
- Activation query: signup → aha within D days → active at <retention check>
- Baseline: <number, or "pending data">

## First session
| Step | Time / clicks | Feel (−2..+2) | Change |
| --- | --- | --- | --- |

Worst moment: <...>

## Peak and end
- Peak: <how the aha is shown and named>
- End: <last screen; what's removed from it>
- Error, empty and offline endings: <...>

## Next
<experiment or cohort comparison, and when to re-check>
```

**Finish with `taste`.** Any screen or component you build or change with this skill gets the `taste` pass before you call it done.
