# Craft details and interface states: research notes

Compiled 2026-11-03. Every claim carries its source. "Unverified" marks figures I could not confirm against the primary text.

---

## 1. What craft teams say about their own practice

### Linear
- Quality is framed as a choice, not a scale problem: makers skip it because they don't care, optimise cost or take the easy path. Linear's practices: ask "does this improve quality?", weigh intuition and direct user feedback beside data, hire for taste via work trials, small teams (about 3 to 5 people), no handoffs, test incomplete work internally rather than ship it half-baked, zero-bugs policy with issues fixed within 7 days. https://linear.app/blog/why-is-quality-so-rare
- Zero-bugs policy: every reported bug is either fixed or explicitly marked "won't fix"; the rationale is that a fix costs the same now or later. They began by clearing about 175 open bugs in three weeks. https://linear.app/now/zero-bugs-policy (third-party summaries add "high priority within 48 h", which I could not find in Linear's own text, so it is unverified).
- Quality Wednesdays: each engineer finds and fixes one small imperfection a week (not necessarily a bug) and shows it at the Wednesday meeting. More than 1,000 fixes in about two years, most taking 30 minutes or less and none over about an hour, tracked with a "quality" label. Example fixes: making adjacent buttons the same size; stopping the issue composer from changing height when a line is added; making a button that darkened instantly fade over 150 ms like its siblings. The habit trains people to see flaws: a group notices what one maker can't, and over time fewer flaws ship. Tip: narrowing the browser window exposes mobile layout bugs. https://linear.app/now/quality-wednesdays

### Vercel Web Interface Guidelines (the most rule-dense primary source)
Source for all bullets: https://vercel.com/design/guidelines
- **Focus**: `:focus-visible` rings that nothing covers; `:focus-within` for grouped controls; sticky UI never hides focus; trap, move and return focus per ARIA patterns.
- **Targets**: at least 24 px (at least 44 px on mobile), with the hit area extended past small visuals; no dead zones ("anything that looks interactive is interactive"); a checkbox or radio shares one generous hit target with its label.
- **Inputs**: mobile inputs at 16 px or larger (no iOS zoom); never disable zoom; never block paste; hydration must not reset focus or value; correct `type`, `inputmode` and `autocomplete`; no spellcheck on emails or codes; trim trailing whitespace; placeholders end in "…" and show an example.
- **Forms**: Enter submits a lone text input; Cmd/Ctrl+Enter submits a textarea; don't disable submit in advance, and once sending starts disable it, show a spinner and send an idempotency key; errors sit beside their fields and focus moves to the first one on submit; warn before navigating away from unsaved data.
- **Loading**: a loading button keeps its label and adds an indicator; spinners and skeletons wait about 150 to 300 ms before showing, then stay at least 300 to 500 ms; skeletons mirror the final content exactly.
- **State**: the URL holds state (filters, tabs, pagination, expanded panels) so share, refresh and Back/Forward work; scroll position is restored on Back/Forward; navigation uses real links so modifier-click and middle-click work.
- **Mutations**: optimistic updates, rolled back or offered Undo on failure; "Confirm destructive actions or offer Undo within a safe window"; POST, PATCH and DELETE finish within 500 ms.
- **Motion**: honour `prefers-reduced-motion`; animate only `transform` and `opacity`; never `transition: all`; animations can be interrupted; set the transform origin where the motion starts; animate only to clarify cause and effect or for deliberate delight.
- **Layout**: optical adjustments of ±1 px where perception differs from geometry; every element aligns to something (grid, baseline, edge or optical centre); balance icon and text lockups; respect `env()` safe areas; test with scrollbars set to Always.
- **Content**: design empty, sparse, dense and error states; every screen offers a next step or a way to recover; layouts survive short, average and very long content; `tabular-nums` for numbers people compare; curly quotes and the real "…"; no widows or orphans; non-breaking spaces between numbers and units and inside shortcuts; locale-aware dates and numbers; never colour alone.
- **Visual**: layered shadows (two or more layers) mixed with semi-transparent borders; **nested radii: the child is no larger than the parent and the two are concentric**; hover, active and focus states have more contrast than rest; `color-scheme: dark` and a matching `theme-color`; tint borders and shadows toward the background hue.
- **Copy**: active voice; second person; error messages say how to fix the problem; specific labels ("Save API Key", not "Continue"); consistent nouns; numerals for counts. (Title Case and "&" are Vercel's house style, not universal rules.)
- **Tooltips**: delay the first tooltip in a group, then show its neighbours without delay; inline help first, tooltips last.

### Rauno Freiberg, "Invisible details of interaction design"
Source: https://rauno.me/craft/interaction-design
- Reuse metaphors so learning compounds; model behaviour on physics.
- **Interruptibility**: motion should be stoppable mid-flight, as in the real world; iOS Settings' back-swipe that waits for its slide-in to finish feels worse.
- **Gesture commit timing**: light actions (overlays, revealing search) fire mid-gesture; destructive ones (dismissing an app) fire only on release, so people can back out.
- **Responsiveness**: apply the delta right away (scale and position follow the finger), then animate past a threshold.
- **Frequency vs novelty**: drop animation from high-frequency keyboard and mouse actions (command menus, context menus, Cmd+Tab); removing motion from a frequently used bookmarking tool made it feel faster.
- Spatial consistency: things open from where they live and close back to it.
- Fitts's law: screen edges and corners are effectively infinite targets; radial menus put every option at the same distance.
- Implicit input: context counts as input (Wallet raising brightness for a pass, the app switcher blurring sensitive content).

### Stripe
- Accessible colour system: HSL lightness is perceptually uneven (yellows look light, blues dark), so they moved to CIELAB and matched lightness curves across hues. **Rule: any two colours at least 5 levels apart pass small-text contrast; at least 4 apart pass large text and icons.** Default text and icon colours pass on white and on every hue's lightest tint. They built a tool that shows out-of-gamut colours while iterating. https://stripe.com/blog/accessible-color-systems
- Front-end animation guidance: custom easing curves, animate only `transform` and `opacity`, keep most animations under 500 ms, `prefers-reduced-motion` turns off decorative motion in both CSS and JS, and an animation starts only once it is fully in view. https://stripe.com/blog/connect-front-end-experience

### Raycast
- Three principles, "fast, simple, and delightful", applied "from big features to tiny details". The redesign brought a larger search bar and larger result icons for scanning, and an action bar that lists every action in context **with its keyboard shortcut (it teaches shortcuts)**. Toasts moved inside the window. The icon set shares one stroke width and corner radius, and there is an optional Compact Mode. https://www.raycast.com/blog/a-fresh-look-and-feel

### Superhuman (command palette)
Source: https://blog.superhuman.com/how-to-build-a-remarkable-command-palette/
- One global shortcut opens it everywhere; the same shortcut closes it, and closing returns focus where it was.
- Every action is a command, alongside its button and shortcut. Command execution stays separate from the UI.
- Showing "Title (matching alias)" teaches the preferred term.
- Search ignores case, matches fuzzily with a threshold, includes synonyms and aliases, and ranks by default scores, context and usage history.
- Hide only commands that don't apply; boost or demote the rest by the current screen or selection.

### Things (Cultured Code)
Source: https://culturedcode.com/things/features/
- Magic Plus: tap to add, or drag to insert at an exact spot (drag to the margin for a heading, onto Inbox to file it).
- Type Travel: typing anywhere navigates, with no shortcut to open search first.
- Natural date parsing ("Tom" for tomorrow, "Wed 8pm").
- Pasting a bulleted list turns it into a checklist.
- Slim Mode; a custom animation toolkit.
- (No Cultured Code essay on its own craft turned up; this list comes from its features page.)

### Apple HIG
- **Concentricity** (WWDC25 "Get to know the new design system"): align radii and margins around a shared centre so shapes nest. **Inner radius = parent radius - padding**; capsules use half the height; a component that can be nested or stand alone uses a concentric shape with a fallback radius. Watch for corners that look "pinched or flared". Views are "mathematically centered when it makes sense, and subtly offset when it doesn't" (optical centring). https://developer.apple.com/videos/play/wwdc2025/356/
- **Undo and redo**: label the result ("Undo Typing"); show what an undo did, scrolling to and highlighting offscreen changes so people don't repeat it; allow many levels of undo back to a logical point such as open or save; consider reverting a batch at once. https://developer.apple.com/design/human-interface-guidelines/undo-and-redo
- **Alerts**: use them sparingly and only for actionable information; never at launch. "Don't alert on common, undoable actions, even destructive ones" (deleting an email or a file). Use an alert for uncommon destructive actions that can't be undone. Button titles are verbs naming the result ("Erase", "Delete"), with no Yes/No and OK only for information; Cancel is never the default; the destructive style goes only on a destructive action the person didn't deliberately choose. https://developer.apple.com/design/human-interface-guidelines/alerts
- **Feedback**: keep status inside the interface, near what it describes; warn only about data loss that is **unexpected** and irreversible ("Finder doesn't warn people every time they throw away a file"); confirm completion only for significant tasks, because people expect success; when a command can't run, say why. Give feedback in several forms (colour, text, sound, haptics). https://developer.apple.com/design/human-interface-guidelines/feedback
- **Restore state**: bring people back where they left off, down to the detail (scroll position, window state and location). The launch screen should be almost identical to the first screen, with no splash or advert. https://developer.apple.com/design/human-interface-guidelines/launching
- **Keyboard**: never repurpose standard shortcuts; don't add a modifier to a standard shortcut for an unrelated command; add custom shortcuts only for the most frequent commands; modifier order is Control, Option, Shift, Command; shortcut titles must make sense in a flat list. https://developer.apple.com/design/human-interface-guidelines/keyboards
- **Writing**: tone fits the moment (direct when serious, light when celebrating); keep a list of terms so language stays consistent; labels are verbs, "clear, not clever" ("Send", not "Let's do it!"); multistep flows run Get Started, then Continue or Next, then Done; one capitalisation style per element type; avoid "we" and sparing possessives ("Favorites", not "Your Favorites"). Empty states give a next step and a button and welcome people, but never hold crucial information, because they are temporary. Errors: prevent them first; show them near the problem; never blame; say what to do ("Choose a password with at least 8 characters", not "too short"); no "oops"; if many people hit the same error, redesign the interaction. https://developer.apple.com/design/human-interface-guidelines/writing

---

## 2. Nielsen Norman Group

- **Empty states**: never leave a container totally blank (people can't tell loading from failure from nothing). Don't show "No records" while still loading. State a no-result outcome plainly; show progress while working. Teach what the area is for ("Star your favorites to list them here"). Give a direct path, such as a Create button, and link instructions to the place where the action happens. Offer more than one way in where useful (set up for real, or explore demo data). https://www.nngroup.com/articles/empty-state-interface-design/
- **Error messages**: place them beside the cause; use redundant cues (never colour alone); scale the message to the severity (a modal only for a severe error); don't flag fields that were merely visited; plain language, codes hidden; say the specific problem and a concrete fix; no blame words ("invalid", "illegal"); **no humour (it wears thin on repeat)**; **preserve the input so people edit rather than retype**; offer fixes people can pick; prevent errors in advance (the missing-attachment warning). https://www.nngroup.com/articles/error-message-guidelines/
- **Confirmation dialogs vs undo**: confirm only serious actions (ones that destroy work, cost real money or can't be undone). Routine confirmations train people to click through, which weakens the dangerous ones. Be specific (name the item and the consequence); buttons name outcomes, not Yes/No; avoid a default Yes; save typing a word for the rarest high-risk cases. Undo complements confirmation, because confirmation can't catch every error. Weigh severity against frequency. https://www.nngroup.com/articles/confirmation-dialog/
- **Heuristics** (#3 user control and freedom: undo, redo, a clearly marked exit; #4 consistency and standards; #5 error prevention; #6 recognition over recall; #7 accelerators such as shortcuts; #9 recognise, diagnose, recover): https://www.nngroup.com/articles/ten-usability-heuristics/
- **Response-time limits**: 0.1 s feels instant; up to 1 s keeps the flow of thought (beyond it, show that work is happening); 10 s is the limit of attention (show percent done and a way to cancel). From Miller 1968 (AFIPS FJCC 33:267-277) and Card, Robertson & Mackinlay 1991 (CHI '91:181-188). https://www.nngroup.com/articles/response-times-3-important-limits/
- **Tone of voice**: four dimensions (formal/casual, serious/funny, respectful/irreverent, matter-of-fact/enthusiastic). In a study of 50 respondents, tone shifted perceived friendliness and formality significantly but modestly (about 0.5 to 1 point on a 5-point scale). https://www.nngroup.com/articles/tone-of-voice-dimensions/

---

## 3. Aesthetic-usability effect, traced to its sources

- NN/g definition: people perceive attractive products as more usable. It cites Kurosu & Kashimura (Hitachi Design Center): 26 ATM layouts, 252 participants, aesthetic ratings correlated more strongly with apparent ease of use than with inherent usability. Caveats: it forgives minor usability problems, not major ones; it can **hide problems in testing** (a participant struggled, then rated ease of use highly and praised the colours), so watch what people do, not what they say. https://www.nngroup.com/articles/aesthetic-usability-effect/
- Primary: Kurosu, M. & Kashimura, K. (1995). "Apparent usability vs. inherent usability: experimental analysis on the determinants of the apparent usability." CHI '95 Conference Companion, 292-293. doi:10.1145/223355.223680. The 26 layouts used the same components arranged differently. https://chi1995.chistatic.hosting.acm.org/proceedings/shortppr/mk_bdy.htm
- Replication: Tractinsky, N. (1997), CHI '97. Reran the study in Israel to test cultural bias and found a stronger aesthetics-to-apparent-usability correlation (r = 0.59 is reported), with mostly negligible correlation between apparent and inherent usability. He flagged a method bias: in the original, usability was judged before any use. https://chi1997.acm.org/proceedings/paper/nt.html
- Tractinsky, N., Katz, A. S. & Ikar, D. (2000). "What is beautiful is usable." *Interacting with Computers* 13(2):127-145. A simulated ATM, with perceptions measured **before and after real use**. The aesthetics-usability correlation held after use; post-use judgments of usability were driven by aesthetics, not by actual usability. The authors tie it to the "what is beautiful is good" stereotype from social psychology. https://academic.oup.com/iwc/article/13/2/127/898608 (PDF: https://mycourses.aalto.fi/pluginfile.php/1645430/mod_folder/content/0/Tractinsky%20%282000%29.pdf)
- Practical reading: polish raises perceived usability and goodwill, but it does not fix real usability problems and can hide them in testing. Measure tasks as well as ratings.

---

## 4. WCAG 2.2 on focus

- **2.4.7 Focus Visible (AA)**: "Any keyboard operable user interface has a mode of operation where the keyboard focus indicator is visible." The indicator stays visible as long as focus does; its contrast falls under 1.4.11 Non-text Contrast (3:1). https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- **2.4.11 Focus Not Obscured (Minimum) (AA, new in 2.2)**: "When a user interface component receives keyboard focus, the component is not entirely hidden due to author-created content." Partial covering (sticky headers, non-modal dialogs) passes but should be minimised. The best practice is no covering at all, which is 2.4.12 (Enhanced, AAA). Fix: `scroll-padding-top` or `scroll-margin` matching the sticky header height. https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html
- **2.4.13 Focus Appearance (AAA)**: the indicator's area is at least that of a 2 CSS px perimeter around the component, and those pixels change by at least 3:1 between unfocused and focused. The simplest pass is a solid 2 px outline (with an offset). Untouched browser defaults are exempt. https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance.html
- A practical target from all three: `:focus-visible { outline: 2px solid <3:1 colour>; outline-offset: 2px }`, with scroll padding under sticky bars.

---

## 5. Typography polish (MDN)

- `font-variant-numeric: tabular-nums` (OpenType `tnum`) gives every digit the same width, so columns, timers, counters and changing numbers don't jitter. `proportional-nums` (`pnum`) suits running text. Other values: `lining-nums`/`oldstyle-nums`, `slashed-zero` (tells 0 from O), `diagonal-fractions`/`stacked-fractions`, `ordinal`. Values from different groups combine. Baseline widely available since January 2020, but it depends on the font having the feature. https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric
- `text-wrap: balance` evens out line lengths; it applies only to blocks of up to 6 lines in Chromium and 10 in Firefox (cheap at that size), so use it on headings, captions and blockquotes. `text-wrap: pretty` runs a slower algorithm for better breaks (fewer orphans) and is meant for body paragraphs, not huge text. `stable` keeps earlier lines still while editing `contenteditable`. Unsupported values fall back to `wrap`, so these are safe progressive enhancements. https://developer.mozilla.org/en-US/docs/Web/CSS/text-wrap
- `font-optical-sizing: auto` (the default) uses a variable font's `opsz` axis: sturdier shapes at small sizes, finer contrast at display sizes. It does nothing without an `opsz` axis. https://developer.mozilla.org/en-US/docs/Web/CSS/font-optical-sizing
- Font smoothing: `font-smooth` is non-standard. `-webkit-font-smoothing: antialiased` and `-moz-osx-font-smoothing: grayscale` work **only on macOS** and make text look slightly lighter (most visible as light text on dark). MDN advises against production reliance and links critiques such as "Please Stop 'Fixing' Font Smoothing". Common practice is to apply it to dark surfaces only, or app-wide as a deliberate house look; it does nothing on Windows. https://developer.mozilla.org/en-US/docs/Web/CSS/font-smooth

---

## 6. Peak-end rule: primary sources, and where design use is extrapolation

- **Kahneman, Fredrickson, Schreiber & Redelmeier (1993)**, "When more pain is preferred to less: Adding a better end," *Psychological Science* 4(6):401-405. A short trial of 60 s with a hand in 14 °C water, and a long trial of the same 60 s plus 30 s while the water warmed to about 15 °C (still painful). Asked which to repeat, a significant majority chose the **longer** trial. https://www.psychologicalscience.org/journals/psychological-science/j.1467-9280.1993.tb00589.x/ (scan: https://ius.uzh.ch/dam/jcr:5ae9adc9-61ec-4174-b37c-4b752f36c23b/Kahnemann%20et%20al.%20-%20When%20More%20Pain%20is%20Preferred%20to%20Less%20(1993).pdf). The share differs between retellings: NN/g says 80% (https://www.nngroup.com/articles/peak-end-rule/), and other summaries say about 69%. I could not read the scanned paper, so treat any exact percentage as unverified.
- **Fredrickson & Kahneman (1993)**, "Duration neglect in retrospective evaluations of affective episodes," *JPSP* 65(1):45-55, doi:10.1037/0022-3514.65.1.45. Using film clips, they found duration barely moves remembered ratings; a few representative moments (peak and end) predict them. https://bear.warrington.ufl.edu/brenner/mar7588/Papers/fredr-kahneman-jpsp1993.pdf
- **Redelmeier & Kahneman (1996)**, "Patients' memories of painful medical treatments: real-time and retrospective evaluations of two minimally invasive procedures," *Pain* 66(1):3-8, doi:10.1016/0304-3959(96)02994-6. Colonoscopy and lithotripsy patients rated pain in real time; their remembered totals tracked the worst moment and the final moment, not the duration. https://en.wikipedia.org/wiki/Peak%E2%80%93end_rule
- **Redelmeier, Katz & Kahneman (2003)**, a randomised trial (often wrongly merged with the 1996 paper): leaving the scope in for 3 extra mildly uncomfortable minutes made the procedure remembered as less unpleasant, and those patients were more likely to return. https://en.wikipedia.org/wiki/Peak%E2%80%93end_rule
- **Meta-analysis**: Alaybek et al. (2022), "All's well that ends (and peaks) well?", *OBHDP* 170. 174 effect sizes; the peak-end effect on retrospective evaluations is large (r = 0.581) and holds across boundary conditions; duration's effect is about zero. https://ideas.repec.org/a/eee/jobhdp/v170y2022ics0749597822000334.html
- **Limits**: Kemp et al. (2008) found it "not an outstandingly good predictor" for holidays; day-long experiences such as hotel stays may not follow it (Miron-Shatz 2009); it needs clear start and end points; expectations and goals change the weights. https://en.wikipedia.org/wiki/Peak%E2%80%93end_rule
- **How designers apply it (practitioner inference)**: NN/g recommends creating positive peaks (Duolingo's celebrations), fixing negative peaks (confusion is remembered), ending on a high note (TurboTax's celebratory closing screen with no upsell), avoiding "please don't go" exit pop-ups, and treating outages and errors as endings that need explanation, an estimate and no blame. https://www.nngroup.com/articles/peak-end-rule/ It also cites Harrison et al. (2007), "Rethinking the progress bar" (UIST '07:115-118): bars that speed up toward the end feel faster than linear ones. https://www.chrisharrison.net/index.php/Research/ProgressBars
- A real product example: Asana's celebration creatures are deliberately occasional ("complete a few tasks in a row however, and one might just pop across your screen") and can be switched off. https://blog.asana.com/2016/03/new-celebrations
- **Where this is extrapolation**: the primary studies cover aversive physical sensations (cold water, medical procedures) and film-induced emotion. Remembered evaluation is measured once, after a bounded episode. None of them tests software completion screens, confetti or app retention. Carrying the findings over to UI "end moments" is a plausible inference, but it is unvalidated. It fits best for bounded flows with a clear end (checkout, onboarding, finishing a project, finishing a performance) and worst for ongoing, all-day tool use (compare the hotel-stay result). Apple's guidance pulls the other way on routine work: confirm completion only for significant tasks, because people expect success. https://developer.apple.com/design/human-interface-guidelines/feedback Rauno says to skip flourishes on frequent actions. https://rauno.me/craft/interaction-design So celebrate rare milestones, keep the routine quiet, and treat the worst moment (errors, lost work) as the most important peak to fix.

---

## 7. Consolidated rule list (each traced above)

**Typography**
- Use `tabular-nums` for any number that changes or sits in a column (timers, BPM, counts, durations).
- Use `text-wrap: balance` on headings and `pretty` on paragraphs.
- Use curly quotes and the real "…" character; put non-breaking spaces in "4 min" and "⌘ K".
- Rely on optical sizing in variable fonts; use font smoothing only as a deliberate macOS choice.

**Geometry**
- Use one radius scale; nested radius = outer radius - padding (concentric).
- Use one spacing scale.
- Optically centre icons in buttons (±1 px), and align every element to something.

**States**
- Rest, hover, active, focus-visible and disabled are all distinct.
- Interactive states have more contrast than rest.
- Focus shows as a 2 px outline at 3:1, never covered by sticky UI (2.4.7 and 2.4.11 AA; 2.4.13 AAA).
- Hit areas are at least 24 px, or 44 px on touch.

**Feedback timing**
- Under 100 ms needs nothing.
- Delay spinners about 150 to 300 ms, then show them for at least 300 to 500 ms.
- Past 1 s show progress; past 10 s show percent done and Cancel.
- Loading buttons keep their label.
- Skeletons match the final layout.

**Undo over confirm**
- Routine destructive actions (delete an item, remove it from a list) act at once and offer Undo; describe and show what the undo restored.
- Keep confirmation dialogs for rare, irreversible, unexpected loss. Name the item and the consequence, use verb buttons, never Yes/No, and give no default to the destructive button.

**Errors**
- Put them beside the cause and say what to do.
- No blame, no "oops", no humour.
- Keep the person's input.
- Focus moves to the first error.
- Explain outages and give an estimate.

**Empty states**
- Never blank, and never "none" while still loading.
- Say what will appear here, and give the primary action and, ideally, a sample or demo path.
- Put nothing crucial there, since empty states are temporary.

**Memory**
- Keep state in the URL (tabs, filters, sort).
- Restore scroll position on Back.
- Remember the last tab or place and reopen where the person left off.
- Never wipe a form on error or re-render.
- Warn before leaving unsaved work.

**Keyboard**
- One global palette shortcut that toggles and restores focus.
- Every action is a command; search is fuzzy, with synonyms, ranked by context.
- Show shortcuts next to actions so they are learned.
- Don't repurpose standard shortcuts.
- No animation on high-frequency keyboard actions.

**Motion**
- Animate only `transform` and `opacity`, at most about 500 ms in most cases, with custom easing.
- Motion can be interrupted, starts from its origin, and has a reduced-motion variant.
- Commit destructive gestures on release.

**Copy**
- Verb labels that are "clear, not clever"; specific labels ("Save draft", not "Continue").
- Consistent nouns, kept in a term list.
- One capitalisation style per element type.
- Tone follows the stakes.

**Consistency as practice**
- Hold a standing polish ritual (Linear: about 30-minute fixes, weekly, shown to peers).
- Fix or explicitly close every bug.
- Narrow the window to find layout breaks.

**Ends**
- Give bounded flows a clear, calm finish (no upsell).
- Keep celebration rare and switchable.
- Fix the worst moment first: the peak-end rule weighs the negative peak as heavily as the end.

---

### Not verified
- The exact share of participants choosing the long trial in the 1993 cold-water study (69% vs 80%): the scanned PDF wasn't text-readable.
- Linear's "48 hours" figure for high-priority bugs.
- Tractinsky 1997's r = 0.59 comes from search snippets, not the full text.
- Apple HIG pages were read through Apple's JSON data endpoints (`developer.apple.com/tutorials/data/design/human-interface-guidelines/*.json`) because the HTML renders client-side; the human-readable URLs are cited.
- No Cultured Code craft essay was found; Things details come from its features page only.
