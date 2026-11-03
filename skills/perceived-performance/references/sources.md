# Perceived performance and feedback: research notes

Collected 2026-11-03. Primary sources only; where only a secondary source exists, it is marked **(secondary)** or **(unverified)**. Every claim carries its URL.

---

## 1. Human response-time limits

### Nielsen (1993), "Response Times: The 3 Important Limits"
Source: https://www.nngroup.com/articles/response-times-3-important-limits/ (excerpt of *Usability Engineering*, 1993, with a 2014 update). Nielsen's own sources: Miller 1968 (AFIPS Fall Joint Computer Conference vol. 33, pp. 267–277) and Card, Robertson & Mackinlay 1991 (CHI '91, pp. 181–188).

| Limit | What the user perceives | Feedback needed |
|---|---|---|
| **0.1 s** | System reacts "instantaneously"; feels like direct manipulation | None beyond the result itself |
| **1 s** | Flow of thought uninterrupted, but the delay is noticed; lose the sense of operating directly on the data | Usually none; 2014 update: change the cursor for delays > 1 s |
| **10 s** | Limit for keeping attention on the dialogue | Percent-done indicator for operations > ~10 s; a clearly signposted way to interrupt; expect users to need to reorient |

- 2–10 s: busy cursor/indicator; avoid rapid flashing (same URL).

### Doherty & Thadani (1982), what it actually measured
Primary text: "The Economic Value of Rapid Response Time", W. J. Doherty (IBM T. J. Watson Research Center) & A. J. Thadani (IBM GPD San Jose), November 1982, reproduced with IBM permission at https://jlelliotton.blogspot.com/p/the-economic-value-of-rapid-response.html
- Setting: 1970s–80s interactive terminal work on large IBM systems (programming, engineering graphics, card wiring, parts forecasting, NIH word processing). Not web or mobile UI.
- Measured: system response time (computer + communication), user "think time", transactions per hour, task duration, programmer output, cost.
- Key figures: programmer transactions/hour **180 at 3.0 s vs 371 at 0.3 s** (+106%); forecasters 99 → 336 transactions/hour; Portsmouth team response 2.3 s → 0.84 s with programmer time 39% under estimate; NIH system at 4 s average response with 390 users, task time 32 → 48 min.
- Claim: user think time is **not** fixed. Slower responses break the user's mental sequence, so each second saved at the system saves more user time ("2.7 s" less system response saved "10.3 s" of user time).
- Recommendation: sub-second response ("ultimately reaching sub-second values").
- **The "400 ms Doherty Threshold" is not in the paper.** The text uses 0.3, 0.25, 0.5, 0.6 and 0.84 s and never 0.4 s / 400 ms (same URL). The 400 ms figure is a later popularisation (secondary sites such as https://typenorm.com/school/library/laws/the-doherty-threshold also flag it as such). Cite Doherty & Thadani for "sub-second response raises productivity more than proportionally", not for 400 ms.
- Method: mostly controlled tests in which response time was varied (card wiring, forecasters), plus baseline comparisons (Portsmouth) and operational data (NIH). This is a company publication, not a peer-reviewed paper (same URL). Thadhani's underlying study is cited as IBM Systems Journal 1981 **(unverified: I could not reach the abstract)**.

### Google RAIL model
Source: https://web.dev/articles/rail
- **Response:** complete an input-triggered transition within **100 ms**; process input events within **50 ms** (leaves headroom for other queued work). Scrolls and drags are excluded.
- **Animation:** produce each frame in **≤ 10 ms** (16 ms budget at 60 fps, minus ~6 ms browser render work).
- **Idle:** do deferred work in chunks of **≤ 50 ms**.
- **Load:** interactive in **≤ 5 s** on a mid-range phone over slow 3G for the first load; **≤ 2 s** on repeat loads.
- Perception table: 0–16 ms smooth motion; 0–100 ms immediate; 100–1000 ms "natural and continuous"; ≥ 1000 ms user loses focus; ≥ 10 000 ms frustrated, likely to abandon.
- Status: the page itself says Core Web Vitals are now "the recommended approach for defining performance goals over RAIL". Use RAIL for the reasoning and budgets; use CWV for targets.

### Core Web Vitals (field metrics, 75th percentile, segmented by mobile/desktop)
Sources: https://web.dev/articles/vitals, https://web.dev/articles/lcp, https://web.dev/articles/inp, https://web.dev/articles/cls

| Metric | Good | Needs improvement | Poor |
|---|---|---|---|
| LCP (loading) | ≤ 2.5 s | 2.5–4.0 s | > 4.0 s |
| INP (responsiveness) | ≤ 200 ms | 200–500 ms | > 500 ms |
| CLS (visual stability) | ≤ 0.1 | 0.1–0.25 | > 0.25 |

- A page passes only if all three meet "good" at p75 (https://web.dev/articles/vitals).
- INP = the longest click/tap/key interaction of the visit, ignoring one outlier per 50 interactions (https://web.dev/articles/inp). It replaced FID: announced May 2023 for March 2024; FID deprecation deadline 9 Sep 2024 (https://web.dev/blog/inp-cwv-launch).
- CLS uses **session windows**: shifts < 1 s apart, window ≤ 5 s, worst window counts. Shifts within **500 ms of a discrete input** (tap, click, key) get `hadRecentInput` and are excluded; scrolls, drags and pinches don't count as input (https://web.dev/articles/cls). Consequence: content that moves *because the user just tapped* is fine if it happens within 500 ms; a loading result that lands later and pushes content is a shift.

---

## 2. Spinner vs nothing: delay before showing, minimum display time

| Source | Delay before showing | Minimum shown once visible |
|---|---|---|
| TanStack Router pending component (https://tanstack.com/router/latest/docs/framework/react/guide/data-loading) | `pendingMs` default **1000 ms** (`defaultPendingMs` router-wide) | `pendingMinMs` default **500 ms** (`defaultPendingMinMs`) |
| Android `ContentLoadingProgressBar` (https://raw.githubusercontent.com/androidx/androidx/androidx-main/core/core/src/main/java/androidx/core/widget/ContentLoadingProgressBar.java) | `MIN_DELAY_MS = 500` | `MIN_SHOW_TIME_MS = 500` |
| Material Components progress indicator (https://raw.githubusercontent.com/material-components/material-components-android/master/docs/components/ProgressIndicator.md) | `app:showDelay` (default 0, configurable) | `app:minHideDelay` (default 0, configurable) |
| React Suspense (https://react.dev/reference/react/Suspense) | Suspended content is revealed **at most once every 300 ms**, measured from the last reveal; boundaries ready inside that window reveal together | — |
| NN/g, Sherwin 2014 (https://www.nngroup.com/articles/progress-indicators/) | No indicator under ~1 s ("distracting" for actions < 1 s) | — |

Rationale stated by TanStack: the delay means fast loaders never show the pending UI; the minimum keeps a spinner that appears just past the threshold from flashing on and off (same URL). Pattern: **delay ≈ 0.5–1 s, then minimum ≈ 0.5 s.**

Prefer keeping the old screen over showing a fallback:
- React: wrapping a navigation in `startTransition` keeps already revealed content on screen instead of swapping in the nearest Suspense fallback; `isPending` lets you mark the control that was pressed (https://react.dev/reference/react/useTransition). React calls the fallback swap "a jarring user experience" (same URL). Transitions don't wait for *new* nested boundaries, which still show their fallback immediately (https://react.dev/reference/react/Suspense).
- `useDeferredValue` keeps stale results visible (e.g. dimmed to opacity 0.5) while new ones load (https://react.dev/reference/react/Suspense).
- Caveat: a controlled text input can't be updated in a transition; keep the input synchronous and defer the rest (https://react.dev/reference/react/useTransition).
- TanStack Router: navigation `staleTime` defaults to 0, so cached data renders right away and revalidates in the background (https://tanstack.com/router/latest/docs/framework/react/guide/data-loading).

---

## 3. Progress indicators: which kind, when

- **< 1 s:** nothing (https://www.nngroup.com/articles/progress-indicators/).
- **~1–10 s:** looped spinner plus text saying what is loading ("Loading comments…") (same URL).
- **≥ 10 s:** percent-done indicator, an estimate ("About 3 minutes remaining"), a way to stop; for batches, "3 of 50" (same URL; Nielsen https://www.nngroup.com/articles/response-times-3-important-limits/).
- Avoid static "Loading…" text and "don't click again" warnings (https://www.nngroup.com/articles/progress-indicators/).
- Evidence cited by NN/g: a University of Nebraska–Lincoln study where people with an animated progress bar were more satisfied and willed to wait about **3× longer** than people with no indicator (https://www.nngroup.com/articles/progress-indicators/; the original study isn't named there, so **secondary**). Myers 1985 (CHI '85, pp. 11–17) is the classic primary on percent-done indicators (cited at https://www.nngroup.com/articles/response-times-3-important-limits/); I didn't read it.
- Apple HIG (https://developer.apple.com/design/human-interface-guidelines/progress-indicators):
  - Prefer determinate when possible; switch indeterminate → determinate once the duration is known.
  - Report progress evenly: "90 percent in five seconds and the last 10 percent in 5 minutes" can feel deceptive.
  - Keep indicators moving; a stationary one reads as frozen. If a process stalls, say why and what to do.
  - Don't morph a spinner into a bar.
  - Avoid vague labels like "loading" or "authenticating".
  - Keep the indicator in a consistent location.
  - Offer Cancel (and Pause if cancelling loses work).
  - Refresh controls: also refresh automatically; don't make people start every update.
- Material: determinate when the wait time is known, indeterminate when it isn't; use one indicator type per kind of activity across the app (https://raw.githubusercontent.com/material-components/material-components-android/master/docs/components/ProgressIndicator.md).
- Perceived duration can be changed by how the bar moves. Harrison et al., "Rethinking the Progress Bar", UIST 2007, classifies behaviours (acceleration, deceleration, pauses) that make a process feel faster or slower (https://www.chrisharrison.net/index.php/Research/ProgressBars). Harrison, Yeo & Hudson, CHI 2010, DOI 10.1145/1753326.1753556 (https://chrisharrison.net/index.php/Research/ProgressBars2): visual effects (ripples, pulsing) shorten perceived duration. Press coverage reports a 5.61 s rippling bar felt equal to a 5 s plain bar, about 10% (**secondary**, https://www.sbs.com.au/news/article/making-downloads-faster-with-trickery/tjsw1rlli). NN/g: start slow, finish fast; avoid stalling at the end (https://www.nngroup.com/articles/progress-indicators/).

---

## 4. Skeleton screens vs spinners: the evidence is weak and mixed

- **Origin:** Luke Wroblewski, "Mobile Design Details: Avoid The Spinner", 17 Sep 2013 (https://www.lukew.com/ff/entry.asp?1797). Polar's spinners "call attention to the fact that someone is waiting". Skeleton screens (a blank version of the page that fills in progressively) keep the focus on content. **No metrics**: the evidence is one user's complaint about "waiting around for pages to refresh and load".
- **Viget, Faulkner & Olvera, 19 Oct 2017** (https://www.viget.com/articles/a-bone-to-pick-with-skeleton-screens):
  - Method: 136 participants, mobile only, between-subjects; spinner n=39, skeleton n=39, blank n=58; same wait length for every condition.
  - Results:
    - Skeleton did **worst**: perceived wait 2.82 s vs spinner 2.41 s vs blank 2.29 s.
    - 59% of skeleton participants agreed it "loaded quickly" vs 74% for the spinner.
    - Post-load task time 10.54 s for skeleton vs ~9.5 s for the others.
  - Caveats: no significance tests reported; possible novelty effect; authors say "more testing is needed".
- **Mejtoft, Långström & Söderström (2018)**, "The effect of skeleton screens", ECCE '18, DOI 10.1145/3232078.3232086 (cited at https://www.nngroup.com/articles/skeleton-screens/). An exploratory study: per summaries, skeleton screens were perceived as faster and easier to navigate (**secondary; full text not read; sample size and effect size unverified**).
- **NN/g, Tankala 2023** (https://www.nngroup.com/articles/skeleton-screens/):
  - Under 1 s: no skeleton or spinner.
  - Skeletons: for full-page loads under 10 s.
  - Spinners: for a single module (a card, a video).
  - Progress bars: for over 10 s and for process tasks (uploads, conversions), where a skeleton confuses.
  - Avoid frame-only skeletons (header and footer only).
  - This is expert guidance; the page cites only Mejtoft 2018.
- **Apple HIG:** "Show something as soon as possible": placeholder text, graphics or animations replaced as content arrives; a blank wait reads as a broken app (https://developer.apple.com/design/human-interface-guidelines/loading).
- "Skeletons are 30% faster" and similar figures: **no primary source found**; don't repeat them.
- **Verdict:** no robust controlled evidence that skeletons beat spinners in perceived time. The defensible case for skeletons is layout stability (no shift when content lands; see §6) and showing structure. They must match the final layout.

---

## 5. Instant feedback on press

- Feedback within **0.1 s** keeps the feeling of direct manipulation (https://www.nngroup.com/articles/response-times-3-important-limits/). RAIL: acknowledge input within 100 ms, handle events within 50 ms (https://web.dev/articles/rail). INP "good" is ≤ 200 ms to the next paint (https://web.dev/articles/inp).
- Show that a press registered (colour change, checkmark). This also prevents repeat taps (https://www.nngroup.com/articles/visibility-system-status/).
- 300 ms tap delay: mobile browsers waited ~300–350 ms to detect a double-tap zoom. It is removed by `<meta name="viewport" content="width=device-width">` (Chrome 32, 2014; iOS 9.3, March 2016) or `touch-action: manipulation` (https://developer.chrome.com/blog/300ms-tap-delay-gone-away).
- Pending state on the control itself: `useTransition`'s `isPending` styles the button that was pressed while the old UI stays put (https://react.dev/reference/react/useTransition).
- Apple: give feedback through several channels (colour, text, sound, haptics); put status feedback next to the item it describes (https://developer.apple.com/design/human-interface-guidelines/feedback).
- (Unverified) Older iOS Safari applied `:active` on touch only when a touch listener was present. MDN's page doesn't say so (https://developer.mozilla.org/en-US/docs/Web/CSS/:active); test on device.

---

## 6. Avoiding layout shift

From https://web.dev/articles/optimize-cls:
- Media: set `width`/`height` attributes or `aspect-ratio`.
- Late content: reserve space with `min-height`/`aspect-ratio`, or use placeholders sized to the expected content. Don't collapse the space when nothing arrives.
- New content (banners, "load more", feeds): reserve its space, overlay it, or load it only on a user action. If it takes > 500 ms, reserve the space within that window.
- Animation: animate `transform`, not `top`/`left` or other layout properties.
- Fonts: `font-display: optional`; metric-matched fallback via `size-adjust` and the `ascent-override`, `descent-override` and `line-gap-override` descriptors; preload critical fonts.
- Navigation: keep pages bfcache-eligible.
- Shifts within 500 ms of a tap are excluded from CLS (https://web.dev/articles/cls), so an expand-on-tap is fine; data that arrives later and pushes content is not.
- Apple: use placeholders that match the final layout (https://developer.apple.com/design/human-interface-guidelines/loading).

---

## 7. Optimistic updates and rollback

### React `useOptimistic` (https://react.dev/reference/react/useOptimistic)
- `const [optimistic, setOptimistic] = useOptimistic(value, reducer?)`. It shows a temporary value while an Action/Transition is pending.
- The setter must run inside `startTransition` or an Action prop. Outside one, React warns and the value reverts at once.
- **Rollback is implicit:** when the Transition ends, React renders the real `value`. On failure the parent never updated `value`, so the UI returns to the old state. Catch the error to show a message.
- Use a reducer (relative update), not a fixed value, so a base value that changes mid-flight isn't clobbered. `optimistic !== value` signals pending.

### TanStack Query (https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates)
- **Via the UI:** render `variables` while `isPending` (e.g. faded). On `isError`, keep it in an error style with Retry → `mutate(variables)`. Return the `invalidateQueries` promise from `onSettled` so the mutation stays pending until the refetch lands. Across components: `mutationKey` + `useMutationState({ filters: { mutationKey, status: 'pending' } })`.
- **Via the cache:**
  - `onMutate`: `cancelQueries` (stops in-flight refetches overwriting the change), `getQueryData` snapshot, `setQueryData`, return the snapshot.
  - `onError`: restore the snapshot.
  - `onSettled`: `invalidateQueries`.
- When to use which: UI approach when the change shows in one place (less code, no rollback); cache approach when several parts of the screen must reflect it.

---

## 8. Prefetch on intent

- **TanStack Router** (https://tanstack.com/router/latest/docs/framework/react/guide/preloading):
  - Strategies: `preload: 'intent'` (hover or touchstart on a `<Link>`), `'viewport'` (IntersectionObserver) or `'render'`; set router-wide with `defaultPreload: 'intent'`.
  - Delay: `defaultPreloadDelay` default **50 ms** for hover/focus/viewport, cancelled if the pointer leaves; **touch preloads start immediately**.
  - Retention: `preloadStaleTime` default **30 s**, `preloadGcTime` **5 min**.
  - With TanStack Query: set `defaultPreloadStaleTime: 0` so Query's `staleTime` decides freshness.
- **TanStack Query** (https://tanstack.com/query/latest/docs/framework/react/guides/prefetching):
  - Prefetch in `onMouseEnter`/`onFocus`, with a `staleTime` so it only fires for old data.
  - Current docs: `queryClient.query()` replaces the deprecated `prefetchQuery`/`ensureQueryData`.
  - Non-critical prefetch: `void …catch(noop)`.
  - In loaders: await critical data, fire secondary data without awaiting; `staleTime: 'static'` for critical cached data so the loader doesn't block.
  - If data is already in hand, `setQueryData` instead of fetching.
  - Hoist or prefetch to avoid request waterfalls.

---

## 9. Undo instead of confirm; toasts; saving states

- **Confirmation dialogs** (Nielsen, 18 Feb 2018, https://www.nngroup.com/articles/confirmation-dialog/):
  - Confirm only serious, irreversible actions; frequent confirmations train people to click Yes without reading.
  - Make each confirmation specific, with outcome-labelled buttons ("Delete file" / "Keep file") and no default Yes.
  - Provide undo wherever possible, e.g. keep a deleted item recoverable for a while instead of declaring the action irreversible.
- **Apple:** "don't warn people when data loss is the expected result of their action" (Finder doesn't confirm moving a file to the Trash). Warn only when data loss is unexpected and irreversible (https://developer.apple.com/design/human-interface-guidelines/feedback).
- **Success confirmation:** Apple reserves explicit success confirmation for important tasks: people "only need to know when it doesn't" succeed (https://developer.apple.com/design/human-interface-guidelines/feedback).
- **Toasts / snackbars:**
  - Material: brief, bottom of screen, no input needed to dismiss; one action such as Undo or Retry (https://raw.githubusercontent.com/material-components/material-components-android/master/docs/components/Snackbar.md, https://developer.android.com/develop/ui/compose/components/snackbar).
  - Android timings: `LENGTH_SHORT` = **1500 ms**, `LENGTH_LONG` = **2750 ms** (`SnackbarManager.java`, https://raw.githubusercontent.com/material-components/material-components-android/master/lib/java/com/google/android/material/snackbar/SnackbarManager.java).
  - On Android 10+ the duration goes through `AccessibilityManager.getRecommendedTimeoutMillis`, flagged for controls when the snackbar has an action, so users who need more time get it (https://raw.githubusercontent.com/material-components/material-components-android/master/lib/java/com/google/android/material/snackbar/Snackbar.java).
  - NN/g: never put errors in transient toasts. One participant missed an error that faded after 5 s and waited 5 minutes. Place confirmations near the control they confirm (https://www.nngroup.com/articles/indicators-validations-notifications/).
- **Accessibility (WCAG 2.2 SC 4.1.3 Status Messages, AA)** (https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html):
  - "Saved", result counts, progress and busy states must be announced without moving focus.
  - Use `role="status"` for these, `role="alert"` only for important, time-sensitive messages.
- **Saving states:** NN/g's status heuristic asks for timely feedback and no silent changes, but gives no save-state spec (https://www.nngroup.com/articles/visibility-system-status/). Apple: integrate status passively near the item it describes (Mail's "Updated just now" in the toolbar) (https://developer.apple.com/design/human-interface-guidelines/feedback). Combined with the TanStack "error style + Retry" pattern (§7), that gives "Saving… → Saved" inline, with a persistent error and Retry on failure.

---

## 10. Offline-first

- **Strategies** (Jake Archibald, Offline Cookbook, https://web.dev/articles/offline-cookbook):

  | Strategy | Use for |
  |---|---|
  | cache only | versioned static assets cached at install |
  | network only | non-GET requests, analytics |
  | cache falling back to network | the offline-first default |
  | network falling back to cache | fresh-when-online content |
  | stale-while-revalidate | content where latest isn't essential |
  | cache then network | show cached data at once, update when the network answers |

- **UX rules** (same URL):
  - Show cached content first.
  - When new content arrives, don't replace what the user is reading; insert above and keep the scroll position.
  - Prefer cache-then-network to network-then-cache, which makes users wait through slow or flaky connections ("slow failures").
- Apple: load in the background and let people do other things while they wait (https://developer.apple.com/design/human-interface-guidelines/loading).
- RAIL repeat-load target ≤ 2 s (https://web.dev/articles/rail); a cache-first shell is the usual way to hit it.

---

## Not found or not verified
- Material 3 site pages (m3.material.io) render client-side and couldn't be read. Material guidance above comes from the Material Components Android docs and source.
- Did not read the full Mejtoft 2018, Myers 1985, Harrison 2007/2010 or Thadhani 1981 papers; only abstracts and reproductions.
- No primary source for "400 ms Doherty threshold" or "skeletons feel 30% faster".
