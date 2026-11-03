---
name: perceived-performance
description: Make apps feel fast — human response-time limits, Core Web Vitals (INP, LCP, CLS), instant press feedback, when to show a spinner and for how long, skeletons vs spinners (and the weak evidence), keeping the old screen during navigation, optimistic updates with rollback, prefetch on intent, avoiding layout shift, undo and toasts, saving states and offline-first. Traced to Nielsen, web.dev, React, TanStack, Apple HIG and Android. Use when an app feels slow or janky, when adding loading states, mutations, navigation or caching, or when reviewing responsiveness.
---

# Perceived performance

Speed is the part of "premium" with the best evidence behind it. The goal is that every input is acknowledged within 100 ms and nobody stares at a blank screen.

## 1. The limits to design for

| Time | What people feel | What to show |
| --- | --- | --- |
| ≤ 0.1 s | Instant, like touching the thing itself | Just the result |
| ≤ 1 s | A noticeable delay, but thought isn't broken | Usually nothing; keep the old screen |
| 1–10 s | Waiting | A spinner or skeleton with text saying what's loading |
| > 10 s | Attention gone | Percent done, an estimate, and Cancel |

Source: [Nielsen, Response times](https://www.nngroup.com/articles/response-times-3-important-limits/) (from Miller 1968 and Card et al. 1991), [NN/g, Progress indicators](https://www.nngroup.com/articles/progress-indicators/).

**Field targets** at the 75th percentile, mobile and desktop separately ([web.dev, Core Web Vitals](https://web.dev/articles/vitals)):

- **INP ≤ 200 ms** (poor > 500): roughly the slowest tap, click or key to the next paint in a visit (one outlier per 50 interactions is ignored) ([INP](https://web.dev/articles/inp)).
- **LCP ≤ 2.5 s** (poor > 4 s).
- **CLS ≤ 0.1** (poor > 0.25). Shifts within 500 ms of a tap don't count, so expanding on tap is fine; data arriving later and pushing content down is not ([CLS](https://web.dev/articles/cls)).

Budgets from RAIL: handle input events within 50 ms, render a frame in ≤ 10 ms, do background work in chunks of ≤ 50 ms ([web.dev, RAIL](https://web.dev/articles/rail)).

**The "400 ms Doherty threshold" is not in Doherty & Thadani's 1982 IBM paper.** The paper measured mainframe terminal work (programmers did 180 transactions an hour at 3 s response and 371 at 0.3 s) and argued for sub-second response ([reproduction](https://jlelliotton.blogspot.com/p/the-economic-value-of-rapid-response.html)). Cite it for "faster response makes people disproportionately more productive", not for 400 ms.

## 2. Acknowledge every input at once

- A pressed state shows instantly (colour, `scale(0.97)`), before any network call. It also stops repeat taps ([NN/g, visibility of system status](https://www.nngroup.com/articles/visibility-system-status/)).
- Mark the control that's working: React's `useTransition` gives `isPending` for the button that was pressed while the rest of the screen stays put ([React](https://react.dev/reference/react/useTransition)).
- Keep the main thread free: break long work into chunks, defer what isn't needed for the next paint, and keep a controlled text input synchronous while deferring the expensive part (`useDeferredValue`).
- The 300 ms tap delay is gone on pages with `<meta name="viewport" content="width=device-width">`; `touch-action: manipulation` covers the rest ([Chrome](https://developer.chrome.com/blog/300ms-tap-delay-gone-away)).

## 3. Loading states

**Show nothing for fast loads, and don't let a spinner flash.** Wait before showing it, then keep it up for a minimum time:

| Source | Delay before showing | Minimum once shown |
| --- | --- | --- |
| TanStack Router `pendingMs` / `pendingMinMs` | 1000 ms | 500 ms |
| Android `ContentLoadingProgressBar` | 500 ms | 500 ms |
| Vercel Web Interface Guidelines | 150–300 ms | 300–500 ms |

Sources range from 150 ms (Vercel) to 1 s (TanStack) before showing, and 300–500 ms minimum once shown. Pick one pair of values per app and use it everywhere ([TanStack Router](https://tanstack.com/router/latest/docs/framework/react/guide/data-loading), [Android source](https://raw.githubusercontent.com/androidx/androidx/androidx-main/core/core/src/main/java/androidx/core/widget/ContentLoadingProgressBar.java), [Vercel](https://vercel.com/design/guidelines)). React reveals Suspense content at most once every 300 ms so boundaries pop in together ([React](https://react.dev/reference/react/Suspense)).

- **Keep the old screen during navigation** rather than swapping in a fallback. Wrap navigations in a transition; React calls the fallback swap "jarring". Dim stale results instead of blanking them.
- **Say what's loading** ("Loading comments…"), not just "Loading". Keep indicators moving; a still one looks frozen. Report progress evenly, not 90% in a second and the last 10% in a minute ([Apple, Progress indicators](https://developer.apple.com/design/human-interface-guidelines/progress-indicators)).
- **Show something as soon as possible.** A blank wait reads as broken ([Apple, Loading](https://developer.apple.com/design/human-interface-guidelines/loading)).
- **Long jobs** (uploads, imports): determinate progress, "3 of 50", Cancel, and let people keep working.

### Skeletons vs spinners: the evidence is weak

Skeleton screens came from a 2013 blog post with no data ([Wroblewski](https://www.lukew.com/ff/entry.asp?1797)). In Viget's 2017 test (136 people, mobile), the skeleton felt *slowest*: 2.82 s perceived vs 2.41 s spinner vs 2.29 s blank ([Viget](https://www.viget.com/articles/a-bone-to-pick-with-skeleton-screens)); one exploratory 2018 study (Mejtoft et al., not read in full) reportedly points the other way. There's no source for "skeletons feel 30% faster".

The case for skeletons that holds is **layout stability**: if a skeleton matches the final layout exactly, nothing jumps when content lands. Use skeletons for whole pages that load in under 10 s, spinners for one module, progress bars for long processes ([NN/g, Skeleton screens](https://www.nngroup.com/articles/skeleton-screens/)). A skeleton of just the header and footer is worse than nothing.

## 4. Don't wait at all: cache, prefetch, optimistic

- **Show cached data first, revalidate in the background.** TanStack Router's `staleTime` defaults to 0, so cached data renders immediately and refreshes behind it.
- **Prefetch on intent.** TanStack Router's `defaultPreload: 'intent'` starts loading after 50 ms of hover or focus and immediately on touch; preloaded data stays fresh 30 s. With TanStack Query, set `defaultPreloadStaleTime: 0` so Query decides freshness ([TanStack Router, Preloading](https://tanstack.com/router/latest/docs/framework/react/guide/preloading), [TanStack Query, Prefetching](https://tanstack.com/query/latest/docs/framework/react/guides/prefetching)).
- **Avoid waterfalls.** Await only the data the first paint needs; start the rest in parallel. Use data already in hand (`setQueryData`) instead of refetching.
- **Optimistic updates** for actions that rarely fail (reorder, favourite, toggle, rename):
  - React `useOptimistic` inside a transition. Rollback is automatic when the transition ends; use a reducer so a base value that changes meanwhile isn't overwritten ([React](https://react.dev/reference/react/useOptimistic)).
  - TanStack Query, one place on screen: render the mutation's `variables` while pending, show them in an error style with Retry on failure.
  - TanStack Query, many places: in `onMutate` cancel queries, snapshot, `setQueryData`; restore the snapshot in `onError`; invalidate in `onSettled` ([TanStack Query, Optimistic updates](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates)).
  - Don't be optimistic about things that often fail or matter a lot (payments, sending to other people).

## 5. Layout shift

- Give images and media `width`/`height` or `aspect-ratio`.
- Reserve space for late content with `min-height` or a skeleton of the real size; don't collapse it if nothing arrives.
- Insert new items (banners, "new posts") only on a user action, or overlay them, or reserve their space.
- Fonts: metric-matched fallbacks (`size-adjust`, `ascent-override`), preload the critical ones, `font-display: optional` where you can ([web.dev, Optimize CLS](https://web.dev/articles/optimize-cls)).
- Keep pages eligible for the back/forward cache so Back is instant.

## 6. Saving, undo and messages

- **Saving state lives next to what's being saved**: "Saving…" → "Saved", or "Updated just now" ([Apple, Feedback](https://developer.apple.com/design/human-interface-guidelines/feedback)). On failure, a persistent error with Retry, never a silent loss.
- **Undo instead of confirm** for routine destructive actions: do it, then offer Undo in a snackbar ([NN/g](https://www.nngroup.com/articles/confirmation-dialog/)). Keep deleted things recoverable for a while.
- **Don't confirm success for routine actions**; people expect success and need to hear only about failure. Confirm significant ones.
- **Toasts** carry one action at most (Undo, Retry). Android's lengths are 1.5 s and 2.75 s, extended for people who need more time ([Material source](https://raw.githubusercontent.com/material-components/material-components-android/master/lib/java/com/google/android/material/snackbar/SnackbarManager.java)). Giving toasts with an action more time and pausing the timer on hover or focus is common practice, with no primary source.
- **Never put errors in a toast that disappears.** One NN/g participant missed a 5-second error and waited 5 minutes ([NN/g](https://www.nngroup.com/articles/indicators-validations-notifications/)).
- Announce status messages ("Saved", result counts) to screen readers with `role="status"`, without moving focus ([WCAG 4.1.3](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html)).

## 7. Offline-first

- Serve the app shell and data from a cache first; update when the network answers ([Archibald, Offline Cookbook](https://web.dev/articles/offline-cookbook)).
- When fresh data arrives, don't replace what someone is reading: add it above and keep their scroll position.
- A slow network is worse than none. Don't make people wait for a request that may time out; show the cached version and update quietly.
- Say what state you're in in plain words ("Saved on this device — will sync"; web.dev advises avoiding the word "offline"), queue writes, and show when data was last updated. Don't trust `navigator.onLine` alone.

## Review checklist

- [ ] Every press shows feedback within 100 ms; INP ≤ 200 ms on a mid-range phone.
- [ ] Fast loads show no spinner; one app-wide delay (150 ms–1 s) and minimum (300–500 ms) for indicators.
- [ ] Navigation keeps the old screen and marks the pressed control as pending.
- [ ] Skeletons, if any, match the final layout exactly.
- [ ] Cached data renders first; links prefetch on intent; no request waterfalls.
- [ ] Rarely-failing mutations are optimistic with a visible rollback.
- [ ] Nothing shifts after load; media has dimensions.
- [ ] Routine deletes undo; errors never vanish in a toast; status messages use `role="status"`.

Full notes and every source: [references/sources.md](references/sources.md).
