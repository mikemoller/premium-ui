# Motion, transitions and micro-interactions: primary-source notes

Fetched 2026-11-03. Each claim carries the URL it came from. "Spec" means a normative standard or platform guideline. "Practitioner" means one person's experience and taste, not measurement. Gaps are listed at the end.

## 1. Durations

| Source | Number | Kind |
|---|---|---|
| NN/g, Animation duration | 100–500 ms overall; ~100 ms for simple feedback (checkbox, toggle); 200–300 ms for big screen changes (a modal coming in); up to 400 ms only for large moves on large screens; above 500 ms "starts to feel like a real drag". Use "the shortest time that an animation can take without being jarring". 250 ms and 300 ms can feel very different. https://www.nngroup.com/articles/animation-duration/ | Guideline. The article says to experiment. It cites no study for these numbers. |
| Material 3 duration tokens | Short1–4 = 50/100/150/200 ms; Medium1–4 = 250/300/350/400; Long1–4 = 450/500/550/600; ExtraLong1–4 = 700/800/900/1000. "Duration should increase as the area or distance an animation covers increases." https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md | Design-system spec |
| Material 3 transition defaults (Android) | Container transform: 300 ms in, 250 ms out. Shared axis: 300 ms. Fade through: 300 ms. Fade (dialogs, menus, snackbars, FABs): 150 ms in, 75 ms out, linear easing. (same URL) | Spec |
| Emil Kowalski | UI animations "should generally stay under 300ms". A 180 ms dropdown or select feels more responsive than a 400 ms one. A 500 ms enter on a frequent action is too slow. Marketing sites can run longer than apps. https://emilkowal.ski/ui/you-dont-need-animations · https://emilkowal.ski/ui/7-practical-animation-tips · https://emilkowal.ski/ui/great-animations | Practitioner |
| Emil, tooltip example | `transform` + `opacity` over 0.125 s, ease-out, from `scale(0.97)`, opacity 0. https://emilkowal.ski/ui/7-practical-animation-tips | Practitioner |
| Apple HIG | No numbers. "Aim for brevity and precision in feedback animations." https://developer.apple.com/design/human-interface-guidelines/motion | Spec, qualitative only |

Takeaway: the sources agree on about 100 ms for small feedback, 150–300 ms for most UI, and 300–400 ms for whole-screen or long-distance moves, with 500 ms as the ceiling. M3's tokens go up to 1000 ms, but its own defaults for app transitions are 300 ms or less.

## 2. Enter vs exit asymmetry
- NN/g: entering takes "subtly longer" than exiting. A popup appears in ~300 ms and leaves in 200–250 ms. https://www.nngroup.com/articles/animation-duration/
- M3: container transform is 300 in / 250 out. Fade is 150 in / 75 out, so the exit is half the enter. https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md
- Emil: enter and exit **in the same direction** (spatial consistency, e.g. Sonner toasts, which makes swipe-to-dismiss feel natural). https://emilkowal.ski/ui/you-dont-need-animations
- Apple: dismiss the way the view arrived. A view that slid down should not be dismissed sideways. https://developer.apple.com/design/human-interface-guidelines/motion

## 3. Easing curves
Material 3 (spec): https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md
- Standard `cubic-bezier(0.2, 0, 0, 1)`: utility motion that starts and ends on screen
- Standard decelerate `cubic-bezier(0, 0, 0, 1)`: entering
- Standard accelerate `cubic-bezier(0.3, 0, 1, 1)`: exiting
- Emphasized: a two-segment path, `M 0,0 C 0.05,0,0.133333,0.06,0.166666,0.4 C 0.208333,0.82,0.25,1,1,1`. One cubic-bezier cannot express it, so on the web use `linear()` (the Chrome linear() generator has an "MD emphasized" preset: https://developer.chrome.com/docs/css-ui/css-linear-easing-function)
- Emphasized decelerate `cubic-bezier(0.05, 0.7, 0.1, 1)`: entering
- Emphasized accelerate `cubic-bezier(0.3, 0, 0.8, 0.15)`: exiting
- Linear `cubic-bezier(0, 0, 1, 1)`. The Fade pattern uses linear.
- Rule: standard for utility motion, emphasized for expressive motion; decelerate to enter, accelerate to exit.

**Conflict on exits.** M3 and NN/g use ease-in/accelerate for exits (https://www.nngroup.com/articles/animation-duration/). Emil uses **ease-out for both enter and exit** and says to avoid `ease-in` in UI because it speeds up at the end and feels sluggish. He also says the built-in CSS curves are too weak and you should use custom ones, without giving values (https://emilkowal.ski/ui/7-practical-animation-tips). He also uses `ease` for a calmer, "elegant" feel, as in Sonner (https://emilkowal.ski/ui/great-animations). Practical middle ground: an exit that leaves the screen (accelerate) is a different case from an exit that fades or scales out in place (short, with ease-out or linear, like M3's 75 ms linear Fade).

## 4. Springs
- **M3 spring tokens** (damping ratio / stiffness): FastSpatial 0.9/1400 and FastEffects 1/3800 for small parts (switches, buttons). DefaultSpatial 0.9/700 and DefaultEffects 1/1600 for partial-screen surfaces (bottom sheet, nav drawer). SlowSpatial 0.9/300 and SlowEffects 1/800 for full screen. *Spatial* (slightly underdamped, so a small overshoot) is for position. *Effects* (critically damped, no overshoot) is for colour and opacity, which "shouldn't overshoot". Pick speed by size or distance. https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md
- **Motion (motion.dev)** spring options and documented defaults: stiffness 1, damping 10, mass 1, bounce 0.25 (range −1..1, 0 = critically damped), duration 0.8 s for `spring()` (0.3 s for a tween), restSpeed 0.1, restDelta 0.01, velocity = the value's current velocity. `visualDuration` sets when the motion *visually* arrives, with the bounce settling after that. Setting any physics option overrides the time options. A negative bounce changes the shape of the curve but not the arrival time. https://motion.dev/docs/spring · https://motion.dev/docs/react-transitions. (Note: the documented stiffness default of 1 looks odd next to damping 10. Check it in the source before relying on it.)
- **Josh Comeau**: mass, tension (stiffness) and friction (damping). Use springs for motion, not for colour or opacity, which matches M3's Effects/Spatial split. Most good springs don't bounce. His sandbox example is mass 1.75 / tension 200 / friction 12, and he does not present it as a default. CSS has no native spring, and Safari's 2017 experiment never spread. https://www.joshwcomeau.com/animation/a-friendly-introduction-to-spring-physics/
- Emil: springs feel natural. Framer Motion runs on rAF on the main thread, so it can drop frames under load, while CSS and WAAPI transform/opacity animations run on the compositor. https://emilkowal.ski/ui/great-animations
- Rauno: lighter objects should bounce less. Carry the throw's momentum and angle instead of using fixed timing. https://rauno.me/craft/interaction-design

## 5. `linear()` easing (CSS springs and bounces)
- Piecewise-linear stops. Values may go beyond 0–1 (overshoot), and percentages position stops in time. Baseline Widely available since **Dec 2023**: Chrome/Edge 113, Firefox 112, Safari 17.2. https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function/linear · https://developer.chrome.com/docs/css-ui/css-linear-easing-function
- Chrome: a bounce needed ~30 stops, and doubling that looked smooth. Tools: Jake Archibald / Adam Argyle's generator, and the DevTools linear() editor (Chrome 114). https://developer.chrome.com/docs/css-ui/css-linear-easing-function
- Josh Comeau: an 11-point hand-made spring looked robotic, and 50 points was convincing. More than 100 points ran as smoothly as `linear(0,1)` on low-end hardware. Three ~75-point springs cost about 1.3 kB gzipped. Keep them as tokens in CSS variables with a duration variable, and note the stiffness and damping in a comment. Use `@supports (animation-timing-function: linear(0, 1))` with a cubic-bezier fallback, because a duplicate `transition` declaration doesn't act as a fallback when the value comes through `var()`. **Limits:** the duration is fixed, and an interrupted transition reverses abruptly instead of keeping its velocity the way a JS spring does. https://www.joshwcomeau.com/animation/linear-timing-function/

## 6. What to animate (performance)
- Animate `transform` (translate, rotate, scale) and `opacity`. "Avoid any property that triggers layout or paint unless it's absolutely necessary." In web.dev's demo, animating `top`/`left` dropped 50% of frames and `transform` dropped 1%. Blur and shadows are expensive to paint. https://web.dev/articles/animations-guide
- `will-change`: use it sparingly and only for a measured problem. For changes that happen now and then, set it from JS just before and remove it afterwards. Layers have their own costs. (same URL)
- Emil: animating padding or margin triggers layout, paint and composite. Target 60 fps. A ~2px `filter: blur()` during a crossfade hides imperfections, but blur costs paint (web.dev above). https://emilkowal.ski/ui/great-animations · https://emilkowal.ski/ui/7-practical-animation-tips
- Apple (games): a steady 30–60 fps. https://developer.apple.com/design/human-interface-guidelines/motion
- View transitions: animating width or height in a `::view-transition-group` runs layout every frame (not yet optimised). https://developer.chrome.com/docs/web-platform/view-transitions/same-document

## 7. When NOT to animate
- Apple: "In apps, generally avoid adding motion to UI interactions that occur frequently." Don't add motion "for the sake of adding motion". Don't use motion as the only carrier of information, so pair it with haptics or audio. https://developer.apple.com/design/human-interface-guidelines/motion
- Emil: **never animate keyboard-initiated actions**, including the highlight moving on arrow-key list navigation. Animation makes them feel delayed. A command menu opened hundreds of times a day gets no animation or a minimal one, and so do frequent hover effects. Raycast is his example. https://emilkowal.ski/ui/you-dont-need-animations · https://emilkowal.ski/ui/great-animations
- Rauno: he removed motion from a bookmarking tool's core keyboard actions and it felt faster. Keyboard and mouse input feels mechanical, so skipping animation there is fine (macOS context menus, app switcher). Animation becomes a burden at "hundreds/thousands of times a day", and he felt it after "a couple of days". https://rauno.me/craft/interaction-design
- NN/g: the more often an animation is seen, the shorter and subtler it should be. Time-filling transition animations frustrated usability-test participants. Many simultaneous animations cancel each other out. Decorative motion steals attention. https://www.nngroup.com/articles/animation-duration/ · https://www.nngroup.com/articles/animation-purpose-ux/
- Legitimate purposes (NN/g): feedback, state or mode change, system status, spatial navigation, preventing disorientation, signifiers. https://www.nngroup.com/articles/animation-purpose-ux/

## 8. Interruptibility
- Apple: "Let people cancel motion." Don't make people wait for an animation to finish, especially one they see repeatedly. https://developer.apple.com/design/human-interface-guidelines/motion
- Rauno: motion must respond to new input mid-flight. A slide-over that ignores a reversed swipe feels broken. Apply the gesture's delta at once and animate only after a threshold. Light, reversible actions may fire mid-swipe. Destructive ones fire only on release, and snapping happens on release so the user can peek and cancel. https://rauno.me/craft/interaction-design
- Emil: CSS **transitions** can be interrupted and retargeted, **keyframe animations** cannot. Prefer transitions (or JS springs) for UI state. https://emilkowal.ski/ui/great-animations
- CSS Transitions spec: when a transition reverses midway, its duration is scaled by the "reversing shortening factor" (the fraction of value distance already covered), so going back doesn't take the full duration. The spec admits this doesn't fully solve more than two states. https://www.w3.org/TR/css-transitions-1/
- Motion: springs start from the value's current velocity, so a retarget keeps its momentum. https://motion.dev/docs/react-transitions
- View transitions: a new transition makes the running one skip to its end, and `skipTransition()` skips only the animation, never the DOM update. https://developer.chrome.com/docs/web-platform/view-transitions/same-document

## 9. Origin-aware popovers and press feedback
- Scale popovers from their trigger, not from `center`: `transform-origin: var(--radix-dropdown-menu-content-transform-origin)` (Radix) or `var(--transform-origin)` (Base UI). Never enter from `scale(0)`; start at ~0.9 or more (his example uses 0.93). Button press: `scale(0.97)` on `:active`. https://emilkowal.ski/ui/7-practical-animation-tips
- Tooltips: a delay before the first one opens, then neighbours open instantly with no animation (Base UI's `data-instant` sets `transition-duration: 0ms`). (same URL) and https://emilkowal.ski/ui/you-dont-need-animations
- Rauno: show where content comes from (expand from the source icon, slide in from its side of the stack). A single-instance element doesn't need an origin cue. https://rauno.me/craft/interaction-design
- M3: container transform when a card or list item grows into a view. Shared axis (x/y/z) for spatial or navigational siblings, with paired screens moving the same way. Fade through for unrelated destinations (bottom-nav tabs). Fade for in-bounds surfaces. https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Motion.md

## 10. View Transitions API
- Support: same-document Chrome/Edge 111, Firefox 144, Safari 18. Cross-document Chrome/Edge 126, Safari 18.2, **no Firefox**. https://developer.chrome.com/docs/web-platform/view-transitions
- Same-document: `document.startViewTransition(cb)` or `{ update, types }`. Promises: `updateCallbackDone`, `ready` (run custom animations here) and `finished` (which fulfils even when the transition is skipped). Element-scoped `element.startViewTransition()` also exists. Skipped if the document is hidden. https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API/Using
- Default: the old snapshot fades 1→0 and the new one 0→1 (a crossfade). Groups animate position, width and height. Set the duration on `::view-transition-group(...)` so old and new stay in sync. Tree: `::view-transition` > `-group(name)` > `-image-pair` > `-old` (static image) / `-new` (live). `:root` is `root`. (same URL)
- `view-transition-name` must be unique among rendered elements, otherwise `ready` rejects and the transition is skipped (the DOM still updates). `match-element` gives names automatically. `view-transition-class` (Chrome 125) shares styles through `::view-transition-group(.cls)`. Types are matched with `:active-view-transition-type()` (Chrome 125, Firefox 144, Safari 18). Clear names set from script after `finished`, or they linger in bfcache. https://developer.chrome.com/docs/web-platform/view-transitions/same-document · https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API/Using
- The page is frozen during the update callback, so fetch data first and keep timeouts tight (~100 ms for fonts and images). Live video can look frozen in the old snapshot. https://developer.chrome.com/docs/web-platform/view-transitions/same-document
- Cross-document: both pages opt in with `@view-transition { navigation: auto; types: … }`. Same origin only (scheme, host and port), no cross-origin redirects, main frame only (Chrome 126). It does not run for address-bar, bookmark or reload navigations. `pageswap` (old page) and `pagereveal` (new page) give `e.viewTransition`, and types are not carried over between pages. `<link rel="expect" href="#id" blocking="render">` waits for the element to exist. Use render blocking only when you can measure the cost. **Chrome skips the transition if the navigation takes more than 4 s** (`TimeoutError`). Prerendering with Speculation Rules helps. https://developer.chrome.com/docs/web-platform/view-transitions/cross-document
- Reduced motion: Chrome suggests `animation: none` under the media query, but adds that reduced motion "doesn't mean the user wants no motion", so prefer a subtler crossfade. https://developer.chrome.com/docs/web-platform/view-transitions/same-document

## 11. Reduced motion and WCAG
- `prefers-reduced-motion: reduce | no-preference`. OS switches: iOS Accessibility > Motion; Android 9+ "Remove animations"; macOS "Reduce motion"; Windows 11 Visual Effects > "Animation Effects". MDN's example replaces a scaling pulse with an opacity "dissolve", which reduces motion without removing it. Large scaling and panning are the vestibular triggers. https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion
- Josh Comeau puts motion inside `@media (prefers-reduced-motion: no-preference)`, so motion is opt-in. https://www.joshwcomeau.com/animation/linear-timing-function/ · Emil swaps movement for a fade under `reduce`. https://emilkowal.ski/ui/great-animations
- Motion: `<MotionConfig reducedMotion="user">` turns off transform and layout animations but keeps `opacity` and `backgroundColor`. `"always"`/`"never"` let an in-app setting override the OS. The `useReducedMotion()` hook returns a boolean. https://motion.dev/docs/react-accessibility
- **WCAG 2.3.3 Animation from Interactions (AAA)**: motion animation triggered by interaction can be disabled unless it is essential. "Motion animation" means steps that give an illusion of movement. Colour and opacity changes that don't change perceived size, shape or position don't count, so fades are fine. An erratum says blur *should* now count. User scrolling is allowed, but parallax added to it is the usual example to address. Techniques: C39 (CSS media query) and SCR40 (JS). https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html
- **WCAG 2.2.2 Pause, Stop, Hide (Level A)**: content that moves, blinks or scrolls, starts automatically, lasts **more than 5 s** and runs beside other content needs pause, stop or hide. Auto-updating content needs the same with no 5 s allowance. A preloader that is the only content is exempt. Pausing only while the element has focus does not count. Best practice is one control that pauses everything. Auto-scrolling text falls under this (failure F16, technique SCR33). https://www.w3.org/WAI/WCAG22/Understanding/pause-stop-hide.html
- Apple visionOS: avoid sustained oscillation, especially around **0.2 Hz**. https://developer.apple.com/design/human-interface-guidelines/motion

## 12. Feedback (Apple)
- Feedback should be accessible across colour, text, sound and haptics. Put status next to the item it describes. Confirm only significant actions. Warn only about unexpected, irreversible data loss. watchOS: avoid indeterminate spinners. https://developer.apple.com/design/human-interface-guidelines/feedback

## 13. Conflicts and evidence quality
- **Exit easing:** M3 and NN/g accelerate (ease-in) out. Emil uses ease-out for everything and avoids ease-in. See §3.
- **Duration ceiling:** NN/g allows up to 400–500 ms. Emil keeps UI under 300 ms. M3 tokens go to 1000 ms, but its defaults stay at 300 ms or less.
- **Reduced motion:** remove motion (Chrome's `animation: none`) vs replace it with a fade (MDN, Chrome's own follow-up, Motion, Emil). WCAG 2.3.3 only requires that *motion* can be disabled, and fades are allowed.
- **Springs vs CSS:** JS springs keep velocity and can be interrupted, but run on the main thread. `linear()` springs run on the compositor, but have a fixed duration and reverse abruptly.
- **Evidence:** none of the duration or easing numbers above come from a controlled study. NN/g calls its ranges guidelines and cites only Pratt et al. 2010 (motion captures attention) for the attention claim. M3 and Apple are platform conventions. Emil and Rauno are practitioner taste. The only measured numbers are performance figures: web.dev's 50% vs 1% dropped frames, and Josh's >100-point `linear()` running smoothly with ~1.3 kB for three springs.

## Gaps (not verified from a primary source)
- **Stagger**: none of the fetched sources gives a stagger interval. Treat any number (e.g. 20–50 ms per item) as unsourced.
- Material 2 "Speed" (device multipliers such as tablet +30%, wearable −30%, desktop faster) could not be fetched (m2.material.io renders client-side). The m3.material.io token pages and Apple's HIG HTML also render client-side. M3 came from the material-components-android Motion.md, Apple from its JSON data endpoint.
- The default view-transition duration is not stated on the MDN or Chrome pages I read.
- Apple HIG gives no millisecond numbers for Motion.
- Motion's documented stiffness default (1) was not checked against its source code.
