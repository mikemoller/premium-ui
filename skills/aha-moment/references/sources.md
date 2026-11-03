# Research notes: finding a product's aha moment, and the peak–end rule

Compiled 2026-11-03 for an agent skill that walks a developer through finding their aha moment. Every claim carries a source. Labels:

- **[primary]**: the person, company or paper itself.
- **[secondary]**: a write-up of someone else's work.
- **[unverified]**: I could not trace it to a first-party source.
- **[my reasoning]**: my own inference, not from a source.

---

## 1. Definitions

- **Aha moment.** The point at which a new user first gets the product's core value. The vendor articles define it loosely. Userpilot: the moment "a user sees why the product is worth using for their specific problem" ([Userpilot](https://userpilot.com/blog/aha-moment/), [secondary]). PostHog: the term is "vague and subjective", so measure it as activation ([PostHog newsletter](https://posthog.com/newsletter/wtf-is-activation.md), [primary for PostHog's method]).
- **Activation.** Reforge's definition, as Elena Verna quotes it: "Activation is taking a user from signing up to establishing a habit around your core value prop" ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation), [secondary for Reforge, primary for Verna]). PostHog: activation is the combination of early actions that best predicts long-term retention, and you check it against data rather than assume it ([PostHog handbook](https://posthog.com/handbook/growth/growth-engineering/per-product-activation), [primary]).
- **Setup, aha, habit.** This is Reforge's three-part model. Secondary sources credit it to Brian Balfour and Casey Winters ([June activation playbook](https://www.june.so/blog/activation-playbook), [Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation); [secondary]).
  - **Setup.** The steps a user needs before they can get value. Setup is not the value itself. Verna: "Setup is only the first step, no matter how innovative it is."
  - **Aha.** The user gets the core value and recognises it. It has to be measurable. A login or a page view alone does not count.
  - **Habit.** The user repeats the aha at the product's natural rhythm. Verna's thresholds: daily products, active 3 of 5 weekdays over the last 2 weeks; weekly products, 3 of the last 4 weeks; monthly products, 3 of the last 4 months.
  - **[unverified]** I found no Balfour essay that sets out this model in his own words. Reforge's material is paywalled, and every source I found is a summary.
- **Balfour on retention.** He is quoted as saying "Retention is fundamentally an output. The three core inputs into retention are activation, engagement, and resurrection" ([koji.so summary](https://www.koji.so/docs/aha-moment-research), [secondary]; [unverified] against the original).
- **More than one aha.** A product can have several. Userpilot lists retention, conversion and virality ahas. Its Zoom example: joining a call through an invite link is the entry or viral aha, and hosting your own first meeting is the retention aha ([Userpilot](https://userpilot.com/blog/aha-moment/), [secondary]; Zoom itself has not said this, [unverified]).
- **Per persona.** Userpilot says the aha differs by persona and use case, and that Slack's sales teams and engineering teams reach value through different setups ([Userpilot](https://userpilot.com/blog/aha-moment/), [secondary]).

## 2. The famous examples, traced

| Claim | Best source found | Status |
|---|---|---|
| Slack: 2,000 messages, after which 93% of teams were still using Slack | Stewart Butterfield in First Round Review, 26 Feb 2015, ["From 0 to $1B"](https://review.firstround.com/from-0-to-1b-slacks-founder-shares-their-epic-launch-strategy/). He says the number came from "which companies stuck with us and which didn't", and that any team that "has exchanged 2,000 messages in its history has tried Slack — really tried it". He puts that at about 10 hours of messages for a 50-person team, or about a week for a 10-person team. | **[primary]**. Butterfield is quoted directly. The article gives no sample size, time window, definition of "still using", or note on causation. |
| Facebook: 7 friends in 10 days | The usual source is Chamath Palihapitiya's Growth Hackers Conference talk (late 2012; TechCrunch covered it on 13 Jan 2013 and said the video was on Udemy) ([TechCrunch](https://techcrunch.com/2013/01/13/chamath-palihapitiya-on-growth-hacking-and-how-to-create-a-sustainable-user-acquisition-engine)). TechCrunch's write-up mentions neither "7 friends" nor "10 days". It only says the team found new users needed to find friends fast, which led to People You May Know. Mixpanel (24 May 2016) quotes Chamath calling it the "keystone" ([Mixpanel](https://mixpanel.com/blog/magic-numbers-are-an-illusion/)). Amplitude's docs repeat the claim ([Amplitude Compass](https://amplitude.com/docs/analytics/charts/compass/compass-aha-moment)). | **Partly verified.** Palihapitiya is the source, through his 2012/2013 talk. I could not get a transcript or video to check the exact words. The fetched coverage contradicts itself on the "10 days" window. Mixpanel says Andrew Chen pointed out it "could have been '10 friends in 12 days' or even 'five friends in one day'", so the exact numbers are a slogan rather than a law. |
| Facebook: the Alex Schultz version | Alex Schultz gave Lecture 6 (Growth) of the Stanford CS183B course "How to Start a Startup" (2014) ([thinkapps summary](https://thinkapps.com/blog/post-launch/how-start-startup-growth/)). Recaps say he discusses the 7-friends rule. | **[unverified]**. The Genius transcript page could not be fetched. |
| Twitter: follow about 30 accounts | Sean Ellis and Morgan Brown, *Hacking Growth* (2017), excerpted on [Mattermark](https://mattermark.com/putting-it-all-together-how-josh-elman-identified-a-growth-driver-at-twitter/). The method: they grouped users into cohorts by days visited per month and found that 7 or more visits a month meant 90–100% retention the next month. They then looked for what that retained group had in common: following about 30 users. The book says outright "correlation does not equal causation". The team kept going and found that the follow-back ratio mattered (about 1/3 to 2/3 is the sweet spot), then phoned users to understand why. | **[secondary]**. A book retelling of Josh Elman's work at Twitter, with no quote from Elman. It is still the best written account of how a team actually did this: cohorts, then correlation, then a causal check, then interviews. |
| Dropbox: one file in one folder on one device | No first-party source found. | **[unverified]**. Probably a growth-blog story. Elena Verna, who worked at Dropbox, gives a different activation: editing, viewing or inviting others on a shared file or folder ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation), [primary for Verna]). |
| Zoom, Canva, Loom, Calendly, Figma, Notion, Grammarly, Mailchimp | Vendor blogs ([Orbix](https://www.orbix.studio/blogs/aha-moment-saas), [Userpilot](https://userpilot.com/blog/aha-moment/)). Orbix itself says these are "patterns", not official metrics, and cites no sources for any company numbers. | **[unverified]**. Userpilot says Loom's cohort analysis found that sharing a video, not recording one, had the strongest retention lift. Userpilot gives no source for this. |
| SurveyMonkey: viewing 5 or more responses. Miro: collaborating with 2 or more people on a board | Elena Verna, from her own time at these companies ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation)) | **[primary]**, by an operator who was there. |
| PostHog's own activation definitions, for example product analytics: first team event ingested, 1 dashboard and 3 insights saved within 30 days; experiments: 1 experiment launched within 14 days | [PostHog newsletter](https://posthog.com/newsletter/wtf-is-activation.md) | **[primary]** |
| VSCO: 8 edits, 10 publishes, 16 collects, at a 90% retention target | [Mixpanel](https://mixpanel.com/blog/magic-numbers-are-an-illusion/) | **[secondary]**. A vendor relaying a customer's figures. |

**Takeaway for the skill.** Almost every famous number is a story told afterwards to rally a team. Mixpanel says the point of a magic number is a "simple, memorable goal", not precision ([Mixpanel](https://mixpanel.com/blog/magic-numbers-are-an-illusion/)). Only Slack's figure has a first-party quote. None of them comes with published method, sample or causal test.

## 3. How teams actually find it: the quantitative method

### 3.1 Correlate early actions with retention

1. **Retained vs churned paths.** Compare the first-week events of retained users with those of users who churned early, split by persona. Userpilot step 1 ([Userpilot](https://userpilot.com/blog/aha-moment/)). Orbix suggests comparing users retained 90 days or more with early churners on their events in the first 7 days ([Orbix](https://www.orbix.studio/blogs/aha-moment-saas); method uncited).
2. **Cohort validation.** For each candidate, compare 30-day retention between users who did it in their first 3 days and users who did not. Userpilot step 2. Orbix's rule of thumb is that a real aha shows about 2x retention or more ([Orbix](https://www.orbix.studio/blogs/aha-moment-saas), [unverified]).
3. **Amplitude Compass**, a first-party tool ([Compass docs](https://amplitude.com/docs/analytics/charts/compass/compass-aha-moment), [inflection metrics](https://amplitude.com/docs/analytics/charts/compass/compass-find-inflection-metrics)):
   - Choose a base cohort (new users) and a target cohort (for example, retained into week 2).
   - The event must happen within N days of first use. The default is 7.
   - Base-cohort users must have been new for longer than the retention window.
   - Good correlations usually fall between 0.2 and 0.4.
   - To pick a threshold, use a 2x2 table: **PPV** (of those who reached it, how many retained), **sensitivity** (of those retained, how many reached it), **NPV** and **specificity**. You want all of them high.
   - Check what share of users reach the threshold. If almost nobody does, the sample is too thin. If almost everybody does, there is no room to improve.
   - If most users never fire any event, NPV and specificity will look misleadingly high. Narrow the base cohort to real users, for example people who logged in.
   - The docs warn: "The only way to prove a causal relationship is to run an A/B or split test."
4. **The PostHog way** (SQL, [newsletter](https://posthog.com/newsletter/wtf-is-activation.md), [handbook](https://posthog.com/handbook/growth/growth-engineering/per-product-activation)):
   1. Collect candidate events from usage data and from the people who talk to customers.
   2. Bundle them into 5–10 candidate definitions of 3–5 events each, including repeat counts (five replays vs one).
   3. Funnel: product intent, then activation within a window (14 or 30 days), then still using about 3 months later.
   4. Compare each definition's retention with the product's average retention.
   5. "The one with the best retention rate (and a large enough sample size) wins."
   6. The warning on narrow definitions: "If you pick an activation definition where only 1% activate, and 100% of those 1% retain, your activation metric is too narrow."
   7. Measure at the group level (company or team) when the product is collaborative.
5. **Turn it into a magic number.** Find the threshold of the action and the window (N times in D days). Treat the round number as a goal for the team to aim at, not as a fact ([Mixpanel](https://mixpanel.com/blog/magic-numbers-are-an-illusion/)).

### 3.2 Then prove it

- **An experiment.** Build onboarding around the candidate and A/B test it against the current onboarding, on both activation rate and 30-day retention. If the lift is marginal, the candidate was correlation, not cause: go back to the shortlist (Userpilot step 5; [Amplitude](https://amplitude.com/docs/analytics/charts/compass/compass-find-inflection-metrics)).
- **Explain it with interviews.** Twitter's team phoned users to find out why the follow-back ratio mattered ([Mattermark/Hacking Growth](https://mattermark.com/putting-it-all-together-how-josh-elman-identified-a-growth-driver-at-twitter/)).

### 3.3 Pitfalls (tell the developer about these)

- **Correlation vs causation, and reverse causation.** Users who were already motivated do more of everything. "2,000 messages" may mark a team that was already committed rather than make it commit ([my reasoning]; Amplitude's docs say the same: a high correlation "can also mean that each of those events is highly correlated with another, as-yet-unidentified event" ([Compass search snippet](https://amplitude.com/docs/analytics/charts/compass/compass-interpret-1.md))).
- **Survivorship bias.** Slack's figure comes from looking at "which companies stuck with us" ([First Round](https://review.firstround.com/from-0-to-1b-slacks-founder-shares-their-epic-launch-strategy/)). You only see the behaviour of users who survived long enough to produce it. Fix: measure candidate actions only within a fixed early window (for example the first 3 or 7 days), measure retention after that window, and include every signup ([my reasoning], in line with Userpilot's 3-day window and Amplitude's N-day window).
- **Stories told afterwards.** The famous numbers are slogans with no published method (section 2).
- **Setup counted as value.** Profile completion, connecting an integration, or creating an empty workspace is setup ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation), [Userpilot](https://userpilot.com/blog/aha-moment/)). Userpilot's own cautionary case: a team chose "published first flow" without checking it, and retention did not move.
- **Activating users instead of teams.** In collaborative products, activate the account or team. At Miro, an account counted as activated only after collaboration ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation)).
- **Too narrow or too broad** ([PostHog](https://posthog.com/handbook/growth/growth-engineering/per-product-activation), [Amplitude](https://amplitude.com/docs/analytics/charts/compass/compass-find-inflection-metrics)).
- **Bad instrumentation.**
  - Decide whether an event fires on start or on completion. PostHog found its recording-analysed event was not firing as expected.
  - Ad blockers drop client-side events. Capture activation events on the server ([PostHog](https://posthog.com/newsletter/wtf-is-activation.md)).
- **Small samples.**
  - Amplitude: check the 95% confidence interval and the sample size ([inflection metrics](https://amplitude.com/docs/analytics/charts/compass/compass-find-inflection-metrics)).
  - [my reasoning] With tens of signups a month, a 30-day retention comparison has very wide confidence intervals. For example, 6 of 10 against 3 of 10 is not a reliable difference. Below a few hundred signups per cohort, use the correlation only as a hint and lean on the qualitative method below.

## 4. The qualitative method (for small products)

### 4.1 Sean Ellis's "very disappointed" test

- **The question.** "How would you feel if you could no longer use [product]?" Answers: very disappointed, somewhat disappointed, not disappointed.
- **The threshold.** Ellis: "achieving product/market fit requires at least 40% of users saying they would be 'very disappointed' without your product." This is from his blog post "The Startup Pyramid". He says it is from 2009. The Substack republication is dated 10 Feb 2025 and paywalled ([Substack](https://seanellis.substack.com/p/the-startup-pyramid)). The quote appears in [MIT Orbit](https://orbit-kb.mit.edu/hc/en-us/articles/205194123-What-is-Product-Market-Fit), [secondary]. He gives the same framing in an interview: "If you're above 40%... you've found product/market fit" ([Venture Hacks](https://venturehacks.com/sean-ellis-interview), [primary, interview]).
- **Where 40% came from.** It is a benchmark from his experience across startups. The count varies by source ("~100", "hundreds"), so it is **[unverified]** as a statistic.
- **Superhuman's version** (Rahul Vohra, [Superhuman blog](https://blog.superhuman.com/how-superhuman-built-an-engine-to-find-product-market-fit/), [primary]; also on First Round Review, 2018).
  - Four questions: how disappointed would you be, what type of people would benefit most, what is the main benefit you get, how can we improve it for you.
  - Survey active users only (used the product at least twice in the past two weeks). About 40 responses is directionally useful.
  - Score went from 22%, to 33% after narrowing to the right personas, to 58% after three quarters of product work.
  - Use the main-benefit answers of the very-disappointed group to name the aha. For Superhuman that was speed. Act on the somewhat-disappointed users who name the same benefit.
- **Why this matters for the aha.** The very-disappointed group's "main benefit" answer is the aha in the users' own words.

### 4.2 Jobs-to-be-done and switch interviews

- **Christensen.** Christensen, Hall, Dillon and Duncan, "Know Your Customers' 'Jobs to Be Done'", *HBR*, Sept 2016 ([HBR](https://hbr.org/2016/09/know-your-customers-jobs-to-be-done)); also the book *Competing Against Luck* (2016). Customers "hire" a product to make progress in a particular situation. The milkshake example: morning commuters hired a milkshake for a long, boring drive. Secondary accounts give conflicting details ([secondary summaries](https://echai.ventures/startingup/r/know-your-customers-jobs-to-be-done--bc-r083)).
- **Moesta and Spiek.**
  - **Switch interviews.** Bob Moesta and Chris Spiek (The Re-Wired Group) reconstruct the timeline from first thought, through looking around, to deciding to buy and using the product ([Intercom podcast](https://www.intercom.com/blog/podcasts/bob-moesta-on-unpacking-customer-motivations-with-jobs-to-be-done), [jobstobedone.org](https://jobstobedone.org/)).
  - **Four forces.** Push of the current situation and pull of the new solution, against anxiety about the new and habit of the present. A switch happens when push and pull outweigh anxiety and habit ([jobstobedone.org](https://jobstobedone.org/)).
  - The book is Moesta and Engle, *Demand-Side Sales 101* (2020).
  - [unverified] "Interview within 30 days of the switch" comes from a vendor guide ([koji.so](https://www.koji.so/docs/switch-interviews-jtbd-method)), not from Moesta.
- **Use for the aha.** Interview recently retained users. Ask "when did you know this was going to work for you?" and get them to walk through the session where it clicked. Their answers give the candidate aha events.

### 4.3 Other qualitative inputs

- **Power-user survey.** Ask retained users when they first felt the product was worth using regularly. Make it multiple choice from your shortlist, plus "Other". If answers cluster, that confirms the aha. If they split, you probably have a different aha per segment ([Userpilot](https://userpilot.com/blog/aha-moment/) step 3).
- **Churned-user survey.** In the cancellation flow, or by email for trial users, ask what they never reached. Pair the answers with session data: did they try the event and fail, or never reach it (Userpilot step 4)?
- **Fresh-account walkthrough.** Sign up with a new email and count the clicks to first real value ([Orbix](https://www.orbix.studio/blogs/aha-moment-saas)).
- **Session recordings** of new users ([Orbix](https://www.orbix.studio/blogs/aha-moment-saas), [Userpilot](https://userpilot.com/blog/aha-moment/)).

## 5. The peak–end rule: primary evidence

- **Kahneman, Fredrickson, Schreiber & Redelmeier (1993).** "When More Pain Is Preferred to Less: Adding a Better End", *Psychological Science* 4(6):401–405, [doi:10.1111/j.1467-9280.1993.tb00589.x](https://www.psychologicalscience.org/journals/psychological-science/j.1467-9280.1993.tb00589.x/).
  - Design: a short trial of 60 s in 14 °C water, and a long trial of the same 60 s plus 30 s while the water warmed to 15 °C.
  - Result: "A significant majority chose to repeat the long trial."
  - Conclusion: duration plays a small role in retrospective evaluations, which are dominated by the worst and the final moments. (The abstract I fetched does not give N or the exact percentage; I could not render the PDF.)
- **Fredrickson & Kahneman (1993).** "Duration Neglect in Retrospective Evaluations of Affective Episodes", *JPSP* 65(1):45–55, doi:10.1037/0022-3514.65.1.45 ([PDF](https://bear.warrington.ufl.edu/brenner/mar7588/Papers/fredr-kahneman-jpsp1993.pdf)).
  - Participants rated pleasant and aversive film clips.
  - Duration had only a small effect on retrospective ratings. Retrospective judgements behave like a weighted average of "snapshots" ([secondary summary of the PDF]).
- **Redelmeier & Kahneman (1996).** "Patients' memories of painful medical treatments…", *Pain* 66(1):3–8, [PMID 8857625](https://pubmed.ncbi.nlm.nih.gov/8857625/).
  - 154 colonoscopy patients and 133 lithotripsy patients, with real-time pain ratings.
  - Retrospective ratings of total pain tracked the worst moment (P<0.005) and the last 3 minutes (P<0.005).
  - "Lengthy procedures were not remembered as particularly aversive."
- **Redelmeier, Katz & Kahneman (2003).** "Memories of colonoscopy: a randomized trial", *Pain* 104(1–2):187–194, [PMID 12855328](https://pubmed.ncbi.nlm.nih.gov/?term=Memories+of+colonoscopy%3A+a+randomized+trial+Redelmeier).
  - 682 patients were randomised. In the extended group, a short interval was added at the end with the scope left in the rectum.
  - The extended group rated the final moments 1.7 vs 2.5, overall unpleasantness 4.4 vs 4.9, and ranked the procedure 4.1 vs 4.6 against other unpleasant experiences. All were significant.
  - **They were also more likely to come back for a repeat colonoscopy** (OR 1.41, P=0.038, median follow-up 5.3 years).
  - This is the closest primary evidence that **a better end changes later behaviour**, not just memory. It matters for a retention argument.
- **Do, Rupert & Wolford (2008).** "Evaluations of pleasurable experiences: The peak-end rule", *Psychonomic Bulletin & Review* 15(1):96–98 ([Wolford faculty page](https://faculty-directory.dartmouth.edu/node/1724)).
  - The effect holds for pleasant experiences too.
  - Children given a chocolate bar and then a less-liked extra (gum) rated the whole less positively than children given the chocolate alone ([secondary summary](https://mdschulkind.people.amherst.edu/psych233/readingresponse/dorupert.html)).
  - Lesson: a mediocre add-on at the end can lower an otherwise good experience.
- **Alaybek, Dalal, Fyffe et al. (2022).** "All's well that ends (and peaks) well? A meta-analysis of the peak-end rule and duration neglect", *Organizational Behavior and Human Decision Processes* 170:104149 ([RePEc](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html)). Note: the journal is OBHDP, not *Psychological Science*.
  - 174 effect sizes.
  - The peak–end effect on retrospective evaluations is large: r = 0.581, 95% CI 0.487–0.661.
  - It is **comparable to the overall average** of the experience.
  - It is stronger than trend, variability, beginning or trough.
  - The effect of duration is essentially nil.
  - It is robust across boundary conditions.
  - **Caveat for the skill:** peak and end matter, but the whole average matters about as much. "Design a good ending" does not excuse a bad middle.
- **Popular secondary sources on applying it to UX.**
  - NN/g, Lexie Kane, 30 Dec 2018, ["The Peak–End Rule"](https://www.nngroup.com/articles/peak-end-rule/):
    - Find and strengthen the rewarding peaks.
    - Remove negative peaks, which leave strong impressions. In one test, a single hard dropdown soured a participant on a whole brand.
    - End on a high note. Their example is TurboTax's relief-themed final screen with no upsell.
    - Avoid "please don't go" pop-ups.
    - Design error and outage states, because they become memorable bad endings.
    - Progress bars that speed up near the end are judged faster (Harrison et al., UIST 2007).
  - positivepsychology.com could not be fetched (HTTP 403).

**Linking the aha and the peak.** [my reasoning, no source makes this exact link] The aha is usually the positive peak of a new user's first session. The end is where the first session (or a key flow) stops. Someone deciding whether to return remembers mostly those two points, and the evidence suggests the overall average about as much (Alaybek 2022). The 2003 colonoscopy trial shows a better-remembered experience raised the chance of coming back. So:

- (a) get users to the aha inside the first session, so the session has a positive peak;
- (b) make that peak clear: show the value, name it, celebrate it in proportion;
- (c) design what happens right after: a clean ending that confirms what was achieved and points to the next time, not a pile of setup prompts or an upsell;
- (d) find and remove negative peaks (errors, confusing steps) before the aha.

## 6. Practical methods for a small team or solo developer

### 6.1 Emotional journey map

List each step from signup to the second session. Rate how each step feels (−2 to +2), from your own fresh-account walkthrough and from watching 3–5 new users. Mark the peak, the worst trough and the ending.

- Source: NN/g's peak/negative-peak/end guidance ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)). The fresh-account click count is from [Orbix](https://www.orbix.studio/blogs/aha-moment-saas).
- The −2..+2 scale is [my reasoning]. It mirrors the real-time ratings in Redelmeier & Kahneman 1996.

### 6.2 JTBD interviews

Run 5–10 interviews with users who stayed, and some with users who left.

- Reconstruct the timeline: first thought, looking around, choosing, first use, the moment it clicked.
- Map the four forces ([jobstobedone.org](https://jobstobedone.org/)).

### 6.3 Candidate aha events

Write each candidate as "the user [verb]s [object] and sees [value]". Reject anything that is setup ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation)). Each candidate must be:

1. **tied to core value;**
2. **reachable by most users in the first session or first few days** (an event only 5% reach is not a viable target);
3. **correlated with retention;**
4. **checked per persona** ([Userpilot](https://userpilot.com/blog/aha-moment/)).

### 6.4 Setup, aha, habit

Write down each stage for your product:

- **Setup:** the minimum steps before value.
- **Aha:** the value event.
- **Habit:** the aha repeated at the product's natural rhythm, using Verna's frequency thresholds.

Measure activation as the share of setup completers who reach the aha ([June](https://www.june.so/blog/activation-playbook), [secondary]).

### 6.5 Activation event and window

- One event, or a small bundle of 2–3, with a count and a window: "X within D days of signup". Typical windows are 3 days ([Userpilot](https://userpilot.com/blog/aha-moment/)), 7 days ([Amplitude](https://amplitude.com/docs/analytics/charts/compass/compass-aha-moment)) and 14–30 days ([PostHog](https://posthog.com/newsletter/wtf-is-activation.md)).
- Retention check: still active at 30 days or about 3 months.
- Unit: the user, or the team/account for collaborative products.

### 6.6 Instrumentation

- Name events as object then verb, in the past tense. Examples: "project created", "invite sent" ([PostHog capture docs](https://posthog.com/docs/product-analytics/capture-events)). PostHog's best-practice guide adds an optional `category:object_action` pattern in lowercase ([best practices](https://posthog.com/docs/product-analytics/best-practices.md)).
- Fire on completion, not on start.
- Capture on the server.
- Include the ids of the person and the account or group ([PostHog](https://posthog.com/newsletter/wtf-is-activation.md)).
- Also capture the signup event so cohorts have a start date ([my reasoning]).

### 6.7 Path to the aha (time-to-value)

- Count the steps and minutes from signup to the aha. Benchmarks seen:
  - Userpilot's median time to value is about 1 day 12 hours across 547 SaaS companies (via [Orbix](https://www.orbix.studio/blogs/aha-moment-saas), vendor data).
  - Orbix's "under 5 minutes" target is [unverified].
- Moves:
  - Cut or defer setup steps that do not lead to the aha.
  - Use sample data or templates.
  - Show one next step at a time.
  - Make empty states useful ([NN/g via Orbix](https://www.orbix.studio/blogs/aha-moment-saas)).
  - End walkthroughs at the value event, not at setup.
  - Offer contextual help when a user stalls ([Userpilot](https://userpilot.com/blog/aha-moment/)).
- Don't over-simplify: Verna says the idea that more onboarding questions always hurt is a myth. In her example, a 3-screen, 9-question flow lost no more than 10% of users ([Verna](https://www.elenaverna.com/p/hey-b2b-i-bet-you-are-measuring-activation)).
- Evidence about pushing users to the activation event: Appcues' model (via Orbix) says a 25% activation lift grew MRR by about 34% over 12 months, against 25% for the same lift in signups. This is a vendor model, not a study ([Orbix](https://www.orbix.studio/blogs/aha-moment-saas)).

### 6.8 The end of key sessions and flows (peak–end)

For the first session and each key flow, decide:

1. **The final screen.** It confirms what was achieved, in the user's terms.
2. **What the user takes away,** and the cue for next time.
3. **What to remove from the end:** upsells, "please don't go" prompts, a survey before the value has landed ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)), and weak add-ons that dilute it ([Do et al. 2008](https://mdschulkind.people.amherst.edu/psych233/readingresponse/dorupert.html)).
4. **The error and offline ending** of that flow ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)).

Keep the middle good as well: the average counts about as much as the peak and the end ([Alaybek 2022](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html)).

---

## 7. Step-by-step method (for the skill to follow with a developer)

Each step: the goal, the questions to ask, and what to produce. Stop and agree on the output before moving on.

**Step 0: Size the evidence.**
- Ask:
  - How many signups a week or month?
  - How long has the product been live?
  - What analytics exist (events, server logs, the database)?
  - Is the buyer or user an individual or a team?
- Decide the track:
  - **Quantitative** if there are a few hundred or more signups per monthly cohort with at least 30–90 days of history.
  - Otherwise **qualitative first**, with the data as a hint.
  - [my reasoning on the threshold]
- Output: the track and the unit of analysis (user or account).

**Step 1: The job.**
- Ask:
  - What were people doing before your product?
  - What pushed them to look? What pulled them to you?
  - What almost stopped them?
  - Ask for 3 real users by name and what each one said.
- Optionally run 5–10 switch interviews (section 4.2).
- Output: one sentence, "When [situation], I want to [progress], so I can [outcome]."

**Step 2: The core value, in the user's words.**
- If there are 30–40 or more active users, run the Ellis/Vohra survey: the four questions, sent to active users only.
- Ask the developer:
  - Which users would be very disappointed?
  - What benefit do they name?
- Output: the main benefit, and the segment that names it.

**Step 3: List candidate aha events.**
- Ask:
  - At what moment does a new user first get that benefit, not just get ready for it?
  - What do they see on screen?
  - What did retained users do in their first session that churned users did not?
- Write 3–8 candidates as "[object] [verb] (count) within [window]".
- Remove anything that is setup.
- Output: the shortlist, with setup steps listed separately.

**Step 4: Setup, aha, habit.**
- Ask:
  - What is the minimum setup before the first candidate is possible?
  - How often should a healthy user come back (daily, weekly, monthly, per event)?
- Output: a three-line model, with the habit threshold from Verna's frequencies or the product's natural rhythm.

**Step 5: Check the candidates.**
- **Quantitative track:**
  - For each candidate, compare the retention of users who did it inside the early window with those who did not, including every signup.
  - Report the reach (% who did it), retention for those who did and didn't, the lift, PPV, sensitivity and the confidence interval.
  - Reject candidates reached by under about 5% or over about 95% of users.
  - Prefer the highest lift that still has meaningful reach.
- **Qualitative track:**
  - Ask 5–10 retained users when they knew it was worth it, multiple choice from the shortlist plus "Other".
  - Ask churned users what they never got to.
  - Look for clustering.
- Ask the developer:
  - Could motivated users simply do more of everything?
  - Is this measured only on survivors?
- Output: one chosen activation definition, marked as a hypothesis.

**Step 6: Instrument it.**
- Ask:
  - Where in the code does the aha complete?
  - Can it be captured on the server?
  - What ids does it need (person, account)?
- Output:
  - The event names (object-verb, past tense), fired on completion. The signup event is included.
  - The activation query: signup, then the aha within D days, then active at the retention check.
  - A baseline number.

**Step 7: Map the first session (time-to-value and peak–end).**
- Walk through a fresh account together. List every step, with time and clicks to the aha. Rate each step −2..+2.
- Ask:
  - Which steps can be removed, deferred, pre-filled or given sample data?
  - Where is the worst moment?
  - What happens on screen right after the aha?
- Output: the step list, cuts ranked by effort, and the negative peaks to fix.

**Step 8: Design the peak and the end.**
- For the aha moment, ask:
  - Does the user notice they have got value?
  - How does the screen say it, in the words from step 2?
- For the end of the first session and of each key flow, ask:
  - What is the last thing the user sees?
  - Does it confirm what they achieved and give a reason to come back?
  - What can be removed from it (upsell, survey, prompt)?
  - What does this flow's error or offline ending look like?
- Output: changes to the peak and the end, plus the error and empty states to improve.

**Step 9: Test causation and iterate.**
- Ship the path changes. Then:
  - **A/B test** if the traffic allows it.
  - **Otherwise** compare cohorts before and after, and treat the result as weak evidence.
- Re-check activation and retention after one retention window.
- If retention does not move, go back to step 3.
- Output: a decision record covering the definition, the evidence, the date and when to review it.

**Questions for the skill to keep in mind throughout:**
- Is this value or is this setup?
- Is this a user or a team?
- Would a motivated user do this anyway?
- Could most new users reach this within the first session?
- What does the user see right before and right after?
- What is the last thing they see?
