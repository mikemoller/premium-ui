# What makes an app feel premium: research notes

Researched 2026-11-03. Every claim links to its source. Where only a secondary write-up says something, the text says so.

## Summary

- **Speed you can feel is the biggest single factor, and it has the best evidence.** The response-time limits (0.1 s feels instant, 1 s keeps the flow of thought, 10 s loses attention) go back to [Miller 1968 and Card et al. 1991, as summarised by NN/g](https://www.nngroup.com/articles/response-times-3-important-limits/). Today's web measure is [INP: 200 ms or less is "good" at the 75th percentile](https://web.dev/articles/inp).
- **Motion should be short, eased, purposeful and optional.** NN/g puts most UI animation at [100–500 ms, simple feedback around 100 ms, and warns that 500 ms and up drags](https://www.nngroup.com/articles/animation-duration/). Material 3 publishes exact [duration tokens (50–1000 ms) and easing curves](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md). Apple says motion should never be the only way information is shown, and that [people should be able to cancel it](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/motion.json).
- **"Perceived performance" tricks are weaker than people say.** Skeleton screens are commonly recommended, but the one controlled test I found had [skeletons do worst of three loaders](https://viget.com/articles/a-bone-to-pick-with-skeleton-screens). Optimistic UI is sound when the action nearly always succeeds and can be rolled back ([TanStack Query](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates)).
- **Peak–end is real and well replicated.** A 2022 meta-analysis of 174 effect sizes [found strong support](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html). Most other UX "laws" are weaker. The Zeigarnik memory effect [largely fails to replicate](https://ideas.repec.org/a/pal/palcom/v12y2025i1d10.1057_s41599-025-05000-w.html). The aesthetic–usability effect [can run in reverse](https://doi.org/10.1016/j.chb.2012.03.024). The "Doherty threshold" of 400 ms rests on a 1982 IBM productivity report that is hard to get hold of.
- **The secondary sources we were given cite almost nothing.** The [HBOX Digital](https://www.hboxdigital.com/blog/what-makes-an-app-feel-premium-beyond-design) and [be-dev.pl](https://be-dev.pl/blog/eng/what-makes-a-mobile-app-feel-premium-to-users-unlocking-top-user-experiences) posts make no sourced claims. The [dev.to React patterns post](https://dev.to/rohith_kn/react-ui-patterns-that-make-apps-feel-instantly-premium-4gob) gives sensible patterns with no evidence. The [Medium Flutter post](https://medium.com/@ankii8946/how-i-made-my-flutter-app-feel-more-premium-541acc90ffcc) returned HTTP 403 and could not be read.

## 1. Speed and response time

### Response-time limits

- **0.1 s:** the user feels the system reacts at once, and for direct manipulation the result alone is enough feedback. **1 s:** the user notices the delay but keeps their train of thought. Delays over 1 s should show that work is going on. **10 s:** attention is lost. Longer waits need a percent-done indicator and a way to cancel. These limits come from [Miller (1968) and Card et al. (1991), summarised by Nielsen](https://www.nngroup.com/articles/response-times-3-important-limits/). Between 2 and 10 s a full progress bar can be overkill, and a busy indicator does ([same source](https://www.nngroup.com/articles/response-times-3-important-limits/)).
- **RAIL** set web budgets on the same 100 ms basis: handle input in 50 ms so a visible response lands within 100 ms, keep about 10 ms of work per frame for 60 fps, and break idle work into chunks of 50 ms or less. web.dev now says [Core Web Vitals replace RAIL for setting goals](https://web.dev/articles/rail).
- **INP** (Interaction to Next Paint) is the current Core Web Vital for responsiveness. It counts from input to the next painted frame (input delay, then processing, then presentation delay). It is "good" at 200 ms or less, "needs improvement" up to 500 ms and "poor" above 500 ms, taken at the 75th percentile of page loads ([web.dev](https://web.dev/articles/inp)).

### The "Doherty threshold" (400 ms)

[Laws of UX](https://lawsofux.com/doherty-threshold/) says productivity rises when the system answers in under 400 ms, citing Doherty and Thadani, IBM Systems Journal, 1982. The primary document is the IBM report *The Economic Value of Rapid Response Time* (GE20-0752). IBM hosts [only a fragment of it online](https://www.vm.ibm.com/devpages/jelliott/evrrt10.html?lnk=hmhmhm). A [Computer History Museum IBM document](https://archive.computerhistory.org/resources/access/text/2024/03/102751398-05-01-acc.pdf) describes the underlying finding: programmer transactions per hour rose steeply once response time fell below one second. This was a 1980s mainframe productivity study and says nothing about perception or "addiction". Treat 400 ms as a historical heuristic. The 0.1 s limit and INP's 200 ms are the better-founded numbers.

### Progress indicators

- Harrison et al. found that people's sense of a progress bar's duration depends on how it moves. A bar that speeds up near the end feels faster ([Rethinking the progress bar, UIST 2007](https://kilthub.cmu.edu/articles/journal_contribution/Rethinking_the_Progress_Bar/6470396/1); follow-up work [on visual augmentations, CHI 2010](https://chrisharrison.net/index.php/Research/ProgressBars)). The "11% faster" figure often quoted could not be traced to the paper in this pass.
- Apple: show status near the thing it describes, keep it unobtrusive, and on watchOS avoid indeterminate spinners that tie people to the screen ([HIG, Feedback](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/feedback.json)).

## 2. Perceived performance: optimistic UI, skeletons, instant feedback

- **Optimistic updates.** TanStack Query documents two ways to do them. One shows the pending mutation's variables in the UI, which needs no rollback. The other writes to the cache in `onMutate`, restores the snapshot in `onError` and refetches in `onSettled` ([TanStack Query docs](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates)). The [dev.to post](https://dev.to/rohith_kn/react-ui-patterns-that-make-apps-feel-instantly-premium-4gob) recommends the same pattern, with no evidence. Optimistic UI is a way to get under the 0.1 s limit above. It is honest only when failure is rare and visible.
- **Skeleton screens.** NN/g suggests nothing under 1 s, a skeleton or spinner up to 10 s (skeletons for full-page loads, spinners for one module), and a progress bar above 10 s. It also warns against skeletons that show only header and footer ([NN/g](https://www.nngroup.com/articles/skeleton-screens/)). NN/g cites one study ([Mejtoft et al. 2018](http://dx.doi.org/10.1145/3232078.3232086)) whose results I could not read. The one controlled test I could read, [Viget 2017](https://viget.com/articles/a-bone-to-pick-with-skeleton-screens), had 136 people view GIF loaders of equal length. Skeletons got the longest estimated wait (2.82 s, against 2.41 s for the spinner and 2.29 s for a blank screen) and the lowest "loaded quickly" score. That test was small and artificial. The claim that skeletons feel faster is **not established**.
- **Pressed and busy states.** A button that disables itself and says what it is doing ("Saving…") prevents double submits ([dev.to, unsourced](https://dev.to/rohith_kn/react-ui-patterns-that-make-apps-feel-instantly-premium-4gob)). This follows from the 0.1 s and 1 s limits above, which give it its evidence.

## 3. Motion and transitions

### Durations

| Source | Guidance |
|---|---|
| [NN/g](https://www.nngroup.com/articles/animation-duration/) | 100–500 ms overall. Toggles and checkboxes about 100 ms. Large changes such as a modal 200–300 ms. 400 ms only for big moves on big screens. 500 ms and up "feels like a drag". Entering slightly longer than exiting (for example 300 ms in, 200–250 ms out). The more often an animation is seen, the shorter it should be. Too slow is the more common mistake. |
| [Material 3 tokens](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md) | Short1–4: 50, 100, 150, 200 ms. Medium1–4: 250, 300, 350, 400 ms. Long1–4: 450–600 ms. Extra long1–4: 700–1000 ms. Longer distance or area means a longer duration. |
| [Apple HIG](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/motion.json) | No numbers for UI. Brief and precise feedback animation, never motion for its own sake, motion optional, people can cancel it, and no waiting for an animation to finish before acting. |

### Easing

- NN/g: avoid linear motion. Use ease-out for things entering and ease-in for things leaving. Ease-in-out on exit can feel sluggish ([NN/g](https://www.nngroup.com/articles/animation-duration/)).
- Material 3 curves ([source](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md)):
  - standard `cubic-bezier(0.2, 0, 0, 1)`, standard decelerate (enter) `(0, 0, 0, 1)` and standard accelerate (exit) `(0.3, 0, 1, 1)`
  - emphasized decelerate (enter) `(0.05, 0.7, 0.1, 1)` and emphasized accelerate (exit) `(0.3, 0, 0.8, 0.15)`
  - emphasized standard, a two-segment path rather than one cubic-bezier
  - linear `(0, 0, 1, 1)`
- Apple's fluid-interface guidance asks that animation be interruptible and that the UI follow new input mid-flight ([HIG Motion](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/motion.json); WWDC 2018 "Designing Fluid Interfaces", linked from [HIG Feedback](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/feedback.json)).

### View transitions on the web

The View Transitions API snapshots the old and new state and animates between them with CSS. Same-document transitions (`document.startViewTransition`) work in Chrome and Edge 111+, Firefox 144+ and Safari 18+. Cross-document ones (`@view-transition { navigation: auto }`) work in Chrome and Edge 126+ and Safari 18.2+, but not Firefox ([Chrome docs](https://developer.chrome.com/docs/web-platform/view-transitions)). That page does not cover reduced motion, so the general rule below applies.

### Reduced motion

- WCAG 2.2 **2.3.3 Animation from Interactions** (Level AAA): motion triggered by interaction can be turned off unless it is essential. The sufficient techniques are the CSS and JS `prefers-reduced-motion` query, or a setting in the app ([W3C Understanding 2.3.3](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html)).
- web.dev: `reduce` means removing non-essential motion. Parallax and scroll effects can trigger vestibular symptoms such as dizziness, nausea and migraine ([web.dev](https://web.dev/articles/prefers-reduced-motion)). The common advice to swap movement for a fade is not on that page. It is practice, not a cited rule.
- NN/g also says to avoid flashing, parallax and scroll-jacking ([NN/g](https://www.nngroup.com/articles/animation-duration/)).

## 4. Haptics

- Apple's rules ([HIG, Playing haptics](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/playing-haptics.json)):
  - Use the system patterns for their stated meaning.
  - Be consistent, so cause and effect stay clear.
  - Match intensity to the animation or sound the haptic goes with.
  - Avoid overuse ("tiresome" when frequent).
  - Prefer short haptics for discrete events.
  - Make haptics optional, so the app works without them.
  - Don't disturb the camera, gyroscope or microphone.
- Apple's iOS patterns are notification (success, warning, error), impact (light, medium, heavy, rigid, soft) and selection (a value changing) ([same](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/playing-haptics.json)).
- On the web, `navigator.vibrate()` has [limited availability and is not Baseline](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API). In practice that means Android only, with no iOS Safari. The Tauri haptics plugin offers `vibrate`, `impactFeedback`, `notificationFeedback` and `selectionFeedback` on Android and iOS, and notes that cheaper Android phones may not support them well ([Tauri docs](https://v2.tauri.app/plugin/haptics/)).
- Apple's feedback page: deliver feedback through several channels (colour, text, sound, haptics), because people silence devices, look away or use VoiceOver ([HIG Feedback](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/feedback.json)). A haptic should never be the only signal.

## 5. Feedback, empty, error and loading states

- **Feedback to match importance.** Routine status stays passive. Confirm significant completions only, since people expect most actions to work. Warn before unexpected, irreversible data loss but not when loss is the point. Keep alerts for critical things ([HIG Feedback](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/feedback.json)).
- **Confirmation vs undo.** Frequent confirmations train people to click through. Undo is the stronger safeguard. A confirmation should name the item and use outcome buttons ("Delete file" and "Keep file", not Yes and No) ([NN/g](https://www.nngroup.com/articles/confirmation-dialog/)).
- **Error messages.** Put them next to the cause, never in colour alone, and with prominence that matches severity. Use plain words for the specific problem, without blame and without jokes that wear thin. Keep the person's input, and offer a next step ([NN/g](https://www.nngroup.com/articles/error-message-guidelines/)).
- **Empty states.** They say what the state is (don't show "No records" while still loading), teach the feature in context and give a direct way to fill the space ([NN/g](https://www.nngroup.com/articles/empty-state-interface-design/)).
- **Loading.** See section 2. Don't show a loader for waits under about 1 s ([NN/g](https://www.nngroup.com/articles/skeleton-screens/)). A short delay before showing any indicator stops cached responses from flashing one. That is practice, not a cited rule.

## 6. Onboarding and delight

- **Tutorials vs contextual help.** NN/g finds up-front walkthroughs "dramatically overused": people skip them, forget them and do no better afterwards. NN/g prefers help that appears when needed, can be dismissed and found again, and isn't used for conventional controls ([NN/g](https://www.nngroup.com/articles/onboarding-tutorials/)). NN/g does not give sample sizes for these studies.
- **Delight and positive peaks.** NN/g's peak–end piece gives examples such as Duolingo's celebration and TurboTax's completion screen with no upsell. It warns against "please don't go" pop-ups, and says outage and error moments are negative peaks that need clear explanations ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)). These are illustrations, not tests.
- **Progress toward a goal.** See goal-gradient and endowed progress below. They support checklists that show real progress.

## 7. Psychology: what holds up

### Peak–end rule: strong

- **Origin.** In [Kahneman, Fredrickson, Schreiber & Redelmeier (1993), *Psychological Science* 4(6)](https://www.psychologicalscience.org/journals/psychological-science/j.1467-9280.1993.tb00589.x/), people held a hand in 14 °C water for 60 s. In a second trial, 30 s more were added while the water warmed to 15 °C. A significant majority chose to repeat the longer trial. The share is given differently in different places: [NN/g says 80%](https://www.nngroup.com/articles/peak-end-rule/) and other summaries say about 69%. I could not open the full text to check.
- **Field evidence.** In a randomised trial with 682 colonoscopy patients, adding a short, less painful interval at the end made the whole procedure remembered as less unpleasant. It also slightly raised the rate of returning for a repeat (odds ratio 1.41) ([Redelmeier, Katz & Kahneman 2003, *Pain* 104](https://pubmed.ncbi.nlm.nih.gov/12855328/)).
- **Replication.** Alaybek et al.'s meta-analysis of 174 effect sizes found peak and end effects large and robust across most moderators ([*OBHDP* 2022](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html); the dissertation version reports [r ≈ .57](https://psychology.gmu.edu/defenses/1259)).
- **Caveat for UX.** The studies are about remembered pain and pleasure in episodes. Transferring them to software sessions is plausible but untested at that scale. Negative peaks count as much as positive ones, and possibly more ([Laws of UX](https://lawsofux.com/peak-end-rule/)).

### Goal-gradient and endowed progress: good

- Hull (1932, 1934) found rats ran faster as they neared food ([Laws of UX origin list](https://lawsofux.com/goal-gradient-effect/)).
- In humans, Kivetz, Urminsky & Zheng (2006) found coffee-card customers bought faster as they neared the free coffee. A 12-stamp card with 2 stamps already given was finished faster than a plain 10-stamp card, and the pace reset after each reward ([*JMR* 43](https://business.columbia.edu/sites/default/files-efs/pubfiles/1200/goalgradient.pdf)).
- Nunes & Drèze (2006): framing 8 steps as 10 with 2 done raised completion and cut completion time ([*JCR* 32(4)](https://ideas.repec.org/a/oup/jconrs/v32y2006i4p504-512.html)). The often-quoted figures of 19% against 34% appear only in [a secondary post](https://performanceloyaltygroup.com/a-concept-that-could-double-your-loyalty-program-results/), and I could not check them against the paper.

### Aesthetic–usability effect: mixed

- Kurosu & Kashimura (1995) rated 26 ATM layouts and found perceived ease of use tracked aesthetics more than actual ease of use ([Laws of UX summary](https://lawsofux.com/aesthetic-usability-effect/)). The study did not measure actual use, and I did not find the paper online.
- [Tractinsky, Katz & Ikar (2000)](https://academic.oup.com/iwc/article-abstract/13/2/127/898608) found aesthetics shaped perceived usability before and after use.
- [Tuch et al. (2012)](https://doi.org/10.1016/j.chb.2012.03.024) found the reverse with 80 people and an online shop: usability changed perceived beauty, and aesthetics did not change perceived usability. Earlier studies also mixed the two up, because changing a layout's looks changes its usability too.
- Conclusion: polish does not cover for friction, and friction makes a product look worse.

### Zeigarnik effect: weak, do not rely on it

[Laws of UX](https://lawsofux.com/zeigarnik-effect/) cites no primary publication. A 2025 meta-analysis found the *memory* advantage for interrupted tasks lacks general validity and is hard to replicate. The *Ovsiankina* effect, the urge to go back to an interrupted task, does look general ([Ghibellini & Meier 2025](https://ideas.repec.org/a/pal/palcom/v12y2025i1d10.1057_s41599-025-05000-w.html); [dissertation](https://boristheses.unibe.ch/4796/)). If anything supports "show the unfinished thing", it is the Ovsiankina effect, not Zeigarnik.

## 8. Typography, spacing and touch targets

- **Target size.** WCAG 2.2 **2.5.8** (AA) asks for at least 24 × 24 CSS px, with exceptions for spacing, an equivalent control, inline links, user-agent controls and essential cases. **2.5.5** (AAA) is the stricter target for important controls ([W3C](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html)). 2.5.5 is 44 × 44 px, as is Apple's usual minimum of 44 pt. That figure was not re-checked on the page in this pass.
- **Consistency.** Haptics, animation and feedback should mean the same thing everywhere ([Apple haptics](https://developer.apple.com/tutorials/data/design/human-interface-guidelines/playing-haptics.json)). None of the sources gives a number for typographic polish.

## Weak or contested evidence

| Claim | Status |
|---|---|
| Skeleton screens feel faster than spinners | Not established. The [Viget test](https://viget.com/articles/a-bone-to-pick-with-skeleton-screens) found the opposite. NN/g's guidance is advice, not data ([NN/g](https://www.nngroup.com/articles/skeleton-screens/)). |
| Doherty threshold: under 400 ms makes apps "addicting" | Wording from [Laws of UX](https://lawsofux.com/doherty-threshold/). The source is a 1982 mainframe productivity report that is hard to obtain. Use 100 ms and INP's 200 ms instead. |
| Zeigarnik: unfinished tasks are remembered better | Fails to replicate in general ([meta-analysis 2025](https://ideas.repec.org/a/pal/palcom/v12y2025i1d10.1057_s41599-025-05000-w.html)). |
| Beautiful is usable | Two-way and confounded ([Tuch et al. 2012](https://doi.org/10.1016/j.chb.2012.03.024)). |
| Peak–end in software | Strong in lab and clinic ([meta-analysis](https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html)). Software uses rest on analogy and examples ([NN/g](https://www.nngroup.com/articles/peak-end-rule/)). |
| Specific numbers: 80% vs 69% (cold water), 19% vs 34% (car wash), "11% faster" progress bars | Differ between secondary sources or could not be traced to the paper. Don't quote them. |
| Swap motion for fades under reduced motion | Common practice, not in [web.dev](https://web.dev/articles/prefers-reduced-motion) or [WCAG 2.3.3](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html). It is fine as long as the fade itself isn't large movement. |
| The HBOX, be-dev.pl and dev.to "premium" claims | No sources ([HBOX](https://www.hboxdigital.com/blog/what-makes-an-app-feel-premium-beyond-design), [be-dev.pl](https://be-dev.pl/blog/eng/what-makes-a-mobile-app-feel-premium-to-users-unlocking-top-user-experiences), [dev.to](https://dev.to/rohith_kn/react-ui-patterns-that-make-apps-feel-instantly-premium-4gob)). Their points that speed and consistency matter more than visuals agree with the primary sources above. Their pattern lists are opinion. |
| Laws of UX as a whole | Useful index, but individual pages vary from well sourced (peak–end) to no primary citation at all (Zeigarnik). |

## Sources

**Primary research**
- Kahneman, Fredrickson, Schreiber & Redelmeier (1993). When more pain is preferred to less. *Psychological Science* 4(6). https://www.psychologicalscience.org/journals/psychological-science/j.1467-9280.1993.tb00589.x/
- Redelmeier, Katz & Kahneman (2003). Memories of colonoscopy: a randomized trial. *Pain* 104. https://pubmed.ncbi.nlm.nih.gov/12855328/
- Alaybek, Dalal et al. (2022). Meta-analysis of the peak-end rule and duration neglect. *OBHDP* 170. https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html (dissertation: https://psychology.gmu.edu/defenses/1259)
- Kivetz, Urminsky & Zheng (2006). The goal-gradient hypothesis resurrected. *JMR* 43. https://business.columbia.edu/sites/default/files-efs/pubfiles/1200/goalgradient.pdf
- Nunes & Drèze (2006). The endowed progress effect. *JCR* 32(4). https://ideas.repec.org/a/oup/jconrs/v32y2006i4p504-512.html
- Tractinsky, Katz & Ikar (2000). What is beautiful is usable. *Interacting with Computers* 13(2). https://academic.oup.com/iwc/article-abstract/13/2/127/898608
- Tuch et al. (2012). Is beautiful really usable? *Computers in Human Behavior* 28(5). https://doi.org/10.1016/j.chb.2012.03.024
- Ghibellini & Meier (2025). Zeigarnik and Ovsiankina effects meta-analysis. *Humanities and Social Sciences Communications* 12. https://ideas.repec.org/a/pal/palcom/v12y2025i1d10.1057_s41599-025-05000-w.html (dissertation: https://boristheses.unibe.ch/4796/)
- Harrison, Amento, Kuznetsov & Bell (2007). Rethinking the progress bar. UIST. https://kilthub.cmu.edu/articles/journal_contribution/Rethinking_the_Progress_Bar/6470396/1 ; Harrison et al. progress bar research: https://chrisharrison.net/index.php/Research/ProgressBars
- Mejtoft, Långström & Söderström (2018). The effect of skeleton screens. ECCE. http://dx.doi.org/10.1145/3232078.3232086 (not read)
- Doherty & Thadani (1982). The economic value of rapid response time. IBM GE20-0752. Fragment: https://www.vm.ibm.com/devpages/jelliott/evrrt10.html?lnk=hmhmhm ; context: https://archive.computerhistory.org/resources/access/text/2024/03/102751398-05-01-acc.pdf

**Guidelines and specs**
- NN/g, Response times: the 3 important limits. https://www.nngroup.com/articles/response-times-3-important-limits/
- NN/g, Executing UX animations: duration and motion characteristics. https://www.nngroup.com/articles/animation-duration/
- NN/g, Skeleton screens 101. https://www.nngroup.com/articles/skeleton-screens/
- NN/g, The peak–end rule. https://www.nngroup.com/articles/peak-end-rule/
- NN/g, Onboarding tutorials vs contextual help. https://www.nngroup.com/articles/onboarding-tutorials/
- NN/g, Confirmation dialogs. https://www.nngroup.com/articles/confirmation-dialog/
- NN/g, Error-message guidelines. https://www.nngroup.com/articles/error-message-guidelines/
- NN/g, Designing empty states. https://www.nngroup.com/articles/empty-state-interface-design/
- Apple HIG, Motion. https://developer.apple.com/design/human-interface-guidelines/motion
- Apple HIG, Playing haptics. https://developer.apple.com/design/human-interface-guidelines/playing-haptics
- Apple HIG, Feedback. https://developer.apple.com/design/human-interface-guidelines/feedback
- Material Design 3 motion tokens (material-components-android). https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md ; spec site: https://m3.material.io/styles/motion/easing-and-duration/tokens-specs
- web.dev, Interaction to Next Paint. https://web.dev/articles/inp
- web.dev, RAIL. https://web.dev/articles/rail
- web.dev, prefers-reduced-motion. https://web.dev/articles/prefers-reduced-motion
- Chrome for Developers, View Transitions. https://developer.chrome.com/docs/web-platform/view-transitions
- W3C, Understanding WCAG 2.2 SC 2.3.3. https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html
- W3C, Understanding WCAG 2.2 SC 2.5.8. https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- MDN, Vibration API. https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API
- Tauri, Haptics plugin. https://v2.tauri.app/plugin/haptics/
- TanStack Query, Optimistic updates. https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates

**Secondary**
- Laws of UX. https://lawsofux.com/ (peak-end, doherty-threshold, goal-gradient-effect, zeigarnik-effect, aesthetic-usability-effect pages)
- Viget (2017), A bone to pick with skeleton screens. https://viget.com/articles/a-bone-to-pick-with-skeleton-screens
- HBOX Digital. https://www.hboxdigital.com/blog/what-makes-an-app-feel-premium-beyond-design
- be-dev.pl. https://be-dev.pl/blog/eng/what-makes-a-mobile-app-feel-premium-to-users-unlocking-top-user-experiences
- dev.to, React UI patterns. https://dev.to/rohith_kn/react-ui-patterns-that-make-apps-feel-instantly-premium-4gob
- Medium, Flutter premium post. https://medium.com/@ankii8946/how-i-made-my-flutter-app-feel-more-premium-541acc90ffcc (HTTP 403, not read)
- Performance Loyalty Group (19%/34% figures, unverified). https://performanceloyaltygroup.com/a-concept-that-could-double-your-loyalty-program-results/

> Update: a full reproduction of the Doherty & Thadani report was found later; what it measured, and the fact that 400 ms does not appear in it, are in [perceived-performance/references/sources.md](../../perceived-performance/references/sources.md#doherty--thadani-1982-what-it-actually-measured).
