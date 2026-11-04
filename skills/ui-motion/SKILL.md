---
name: ui-motion
description: Rules for UI motion on web and mobile — durations, easing curves, springs, enter/exit asymmetry, interruptible transitions, origin-aware popovers, press feedback, View Transitions, and reduced motion — each traced to Material 3, Apple HIG, NN/g, MDN, WCAG or named practitioners. Use when adding, tuning or reviewing any animation, transition, micro-interaction, page transition, popover, toast, sheet or drawer, or when motion feels slow, janky, floaty or "too much".
---

# UI motion

Motion is there to explain something: where a thing came from, what changed, that the app heard you. If it explains nothing, cut it. Every number below is a guideline or practitioner taste, not a lab result (see [Evidence](#evidence)), so treat ranges as starting points and tune by eye at full speed and at 10% speed in the browser's Animations panel.

Express every change in the project's existing styling system (Tailwind utilities in a Tailwind project, CSS variables or the CSS-in-JS in use). Don't add an animation library for something a CSS transition does.

## 1. Decide whether to animate at all

Animate when the motion carries information:

- feedback that an input landed (press, toggle, drop)
- a change of state or mode
- system status (saving, syncing)
- spatial navigation (where a screen went, where a sheet came from)
- keeping people oriented while layout changes

Don't animate:

- **Keyboard-initiated actions**, including the highlight moving through a list with arrow keys. Motion makes them feel laggy.
- **Things done hundreds of times a day**: a command palette, a frequent hover, a list row. Give them no motion or a very short fade.
- **Decoration** that competes with the content, and several things moving at once.
- **Information carried by motion alone.** Pair it with text, colour or a haptic.

Sources: [Apple HIG, Motion](https://developer.apple.com/design/human-interface-guidelines/motion), [NN/g, purpose of animation](https://www.nngroup.com/articles/animation-purpose-ux/), [Emil Kowalski, You don't need animations](https://emilkowal.ski/ui/you-dont-need-animations), [Rauno Freiberg, Invisible details](https://rauno.me/craft/interaction-design).

## 2. Durations

| What moves | Duration |
| --- | --- |
| Small feedback: press, toggle, checkbox, tooltip | 100–150 ms |
| Menus, popovers, dropdowns, small panels | 150–250 ms |
| Sheets, dialogs, drawers, a card growing into a view | 250–300 ms |
| Whole-screen or long-distance moves on large screens | 300–400 ms |
| Ceiling for UI | 500 ms ("starts to feel like a drag") |

- Duration grows with the distance or area covered ([Material 3 motion](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md)). M3's tokens: short 50–200, medium 250–400, long 450–600, extra long 700–1000 ms.
- The more often people see it, the shorter it should be ([NN/g, animation duration](https://www.nngroup.com/articles/animation-duration/)). App UI stays under about 300 ms; marketing pages can run longer ([Emil Kowalski](https://emilkowal.ski/ui/7-practical-animation-tips)).
- Use the shortest duration that isn't jarring. 250 ms and 300 ms feel noticeably different, so tune in small steps.

## 3. Enter and exit

- **Exits are shorter than enters.** NN/g: in around 300 ms, out 200–250 ms. M3 container transform 300/250 ms; M3 fade 150/75 ms.
- **Leave the way you came.** A sheet that slid up goes back down. A toast that came in from the right swipes out to the right ([Apple HIG](https://developer.apple.com/design/human-interface-guidelines/motion), [Emil](https://emilkowal.ski/ui/you-dont-need-animations)).
- **Pick the pattern by relationship** (M3): a card or row that becomes a page uses a container transform. Sibling screens in a sequence share an axis and move the same way. Unrelated destinations, such as bottom tabs, fade through. Surfaces that stay within their bounds, such as menus and dialogs, fade.

## 4. Easing

Use custom curves; the CSS keywords are weak.

```css
:root {
  /* Material 3 */
  --ease-standard: cubic-bezier(0.2, 0, 0, 1);          /* moves within the screen */
  --ease-decelerate: cubic-bezier(0, 0, 0, 1);          /* enters */
  --ease-accelerate: cubic-bezier(0.3, 0, 1, 1);        /* leaves the screen */
  --ease-emphasized-in: cubic-bezier(0.05, 0.7, 0.1, 1);
  --ease-emphasized-out: cubic-bezier(0.3, 0, 0.8, 0.15);
}
```

- **Entering: decelerate** (fast start, soft landing). Ease-out is the default for anything appearing or responding to input.
- **Exiting: it depends.** Something that *leaves the screen* can accelerate out (M3, NN/g). Something that *fades or shrinks in place* should be short and ease-out or linear. Practitioners such as Emil Kowalski avoid ease-in in UI altogether because it feels sluggish at the start. When in doubt, use a short ease-out.
- **Opacity-only fades** can be linear (M3's fade pattern is).
- Material's full "emphasized" curve has two segments and can't be written as one `cubic-bezier`; use `linear()` (Chrome's [linear() generator](https://developer.chrome.com/docs/css-ui/css-linear-easing-function) has a preset).

## 5. Springs

- Use springs for **position, size and rotation**. Don't use them for colour or opacity, which must not overshoot ([M3](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md), [Josh Comeau](https://www.joshwcomeau.com/animation/a-friendly-introduction-to-spring-physics/)).
- M3 spring tokens (damping ratio / stiffness): fast 0.9 / 1400 for small parts, default 0.9 / 700 for partial-screen surfaces, slow 0.9 / 300 for full screen. Effects (colour, opacity) use damping 1, no bounce.
- Most good UI springs barely bounce. Small, light things should bounce least ([Rauno](https://rauno.me/craft/interaction-design)).
- A JS spring (Motion, React Spring) keeps its velocity when retargeted, which is why gestures feel physical. It runs on the main thread, so it can drop frames under load ([Emil](https://emilkowal.ski/ui/great-animations)).
- CSS has no native spring. A `linear()` curve with 30–50+ points looks like one and runs on the compositor ([Chrome](https://developer.chrome.com/docs/css-ui/css-linear-easing-function), [Josh Comeau](https://www.joshwcomeau.com/animation/linear-timing-function/)). It has a fixed duration and reverses abruptly when interrupted. Use it for enter animations, not for gestures. Supported in all major browsers since December 2023 ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function/linear)). Wrap it in `@supports (transition-timing-function: linear(0, 1))` with a `cubic-bezier` fallback, because a value set through `var()` doesn't fall back by itself.

## 6. Interruptible by default

- People must never wait for an animation to finish ([Apple HIG](https://developer.apple.com/design/human-interface-guidelines/motion)).
- **Use CSS transitions for interactive state** (open/closed, hover, pressed, selected). A transition retargets from where it is, and reversing halfway takes only the time needed to go back ([CSS Transitions spec](https://www.w3.org/TR/css-transitions-1/)). Keyframe animations restart from the beginning. Keep `@keyframes` for one-off sequences such as a loading shimmer or a first-run reveal.
- **Gestures follow the finger.** Apply the drag delta immediately and animate only on release. Light, reversible actions may commit mid-gesture; destructive ones commit only on release, so people can peek and back out ([Rauno](https://rauno.me/craft/interaction-design)). Carry the release velocity into the settle animation.

## 7. Micro-interaction recipes

- **Press feedback:** `scale(0.97)` on `:active` ([Emil](https://emilkowal.ski/ui/7-practical-animation-tips)), over about 100 ms with ease-out (NN/g's small-feedback range). It confirms the press before anything else happens.
- **Popovers grow from their trigger.** Set `transform-origin` to the trigger side. Radix exposes `--radix-<component>-content-transform-origin`, Base UI `--transform-origin`. Start from `scale(0.9)`–`scale(0.97)` plus opacity 0, never `scale(0)`, which looks like something exploding out of a point.
- **Show where things come from.** Expand from the icon that opened it, slide in from the side it lives on. Something that only ever appears in one place needs no origin cue ([Rauno](https://rauno.me/craft/interaction-design)).
- **Tooltips:** a short delay before the first one opens, then neighbours open instantly with no animation while the pointer moves along a toolbar.
- **Stagger** lists sparingly and keep the total short. No primary source gives an interval; any number you see (20–50 ms per item is common) is taste.

## 8. Performance

- Animate only `transform` and `opacity`. In web.dev's test, animating `top`/`left` dropped 50% of frames; `transform` dropped 1% ([web.dev animations guide](https://web.dev/articles/animations-guide)).
- `width`, `height`, `padding`, `margin`, `top`/`left` trigger layout every frame. Blur and large shadows are expensive to paint.
- Transition named properties (`transition-property: transform, opacity`), never `transition: all`, which also animates properties you didn't mean to, including layout ones ([Vercel guidelines](https://vercel.com/design/guidelines)).
- `will-change` only for a measured problem, and remove it when the animation ends. Every promoted layer costs memory.
- To animate height (an accordion), common practice is `grid-template-rows: 0fr → 1fr` on a grid wrapper, or `interpolate-size: allow-keywords` where supported. Both still animate layout, so keep them short.

## 9. View Transitions

- Same-document (`document.startViewTransition`): Chrome/Edge 111, Firefox 144, Safari 18. Cross-document (`@view-transition { navigation: auto; }` on both pages): Chrome/Edge 126, Safari 18.2, not Firefox ([Chrome docs](https://developer.chrome.com/docs/web-platform/view-transitions)). Always treat it as progressive enhancement.
- Default is a crossfade of the whole page; give elements that should morph a `view-transition-name`.
- Names must be **unique** among rendered elements, otherwise the transition is skipped (the DOM still updates). Clear names set from script after `finished`.
- The page is **frozen during the update callback**: fetch data first, then start the transition. Chrome skips cross-document transitions on navigations over 4 s.
- Set durations on `::view-transition-group(name)` so the old and new snapshots stay in sync.
- If your framework or router wraps this API, use its wrapper rather than calling it by hand (check its docs).

## 10. Reduced motion

- Respect `prefers-reduced-motion: reduce`. **Reduce, don't remove:** replace movement (slides, scales, parallax, zooms) with a short opacity fade. Fades, colour changes and instant state changes are fine ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion), [Chrome](https://developer.chrome.com/docs/web-platform/view-transitions/same-document)).
- Large scaling and panning are the vestibular triggers. Blur now counts as motion too.
- In Motion, `<MotionConfig reducedMotion="user">` turns off transform and layout animations and keeps opacity and colour ([Motion docs](https://motion.dev/docs/react-accessibility)).
- [WCAG 2.3.3](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html) (AAA): motion triggered by interaction can be turned off unless it's essential.
- [WCAG 2.2.2](https://www.w3.org/WAI/WCAG22/Understanding/pause-stop-hide.html) (Level A, required): anything that moves or scrolls by itself for more than 5 s alongside other content needs a pause or stop control. This includes auto-scrolling text, such as tickers and teleprompters.
- Apple's visionOS guidance: avoid sustained oscillation, especially around 0.2 Hz ([Apple HIG](https://developer.apple.com/design/human-interface-guidelines/motion)).

```css
@media (prefers-reduced-motion: reduce) {
  .sheet[data-state="open"] { animation: fade-in 150ms linear; }
}
```

## Review checklist

- [ ] Every animation explains something; keyboard actions and very frequent actions have none.
- [ ] Durations: feedback ≈100–150 ms, UI ≤300 ms, nothing over 500 ms.
- [ ] Exits shorter than enters, and leave the way they came.
- [ ] Ease-out (decelerate) on enter; no `ease-in` on things that respond to input.
- [ ] Interactive states use transitions, not keyframes; gestures follow the finger and keep velocity.
- [ ] Popovers scale from their trigger, starting at ≥0.9, never 0.
- [ ] Only `transform` and `opacity` animate; no `transition: all`.
- [ ] Reduced motion swaps movement for a fade; auto-moving content over 5 s can be paused.

## Evidence

None of the durations or easing values here come from a controlled study. NN/g calls its ranges guidelines; Material and Apple are platform conventions; Emil Kowalski, Rauno Freiberg and Josh Comeau are practitioners writing from experience. The measured figures are the performance ones (web.dev's dropped-frame test). Sources disagree on exit easing (accelerate vs ease-out) and on the ceiling (Emil ≤300 ms, NN/g up to 400–500 ms). Full notes and every source: [references/sources.md](references/sources.md).

**Finish with `taste`.** Any screen or component you build or change with this skill gets the `taste` pass before you call it done.
