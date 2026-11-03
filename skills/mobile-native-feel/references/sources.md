# Native-feeling mobile web apps (Tauri WebView, PWA): research notes

Collected 2026-11-03. Primary sources: Apple HIG (read through its JSON data endpoints, `developer.apple.com/tutorials/data/design/human-interface-guidelines/<page>.json`), Android Developers, MDN, web.dev and developer.chrome.com, W3C WCAG, Tauri v2 docs, WebKit blog. NN/g is the one practitioner source and is labelled as such.
Tags: **[convention]** platform rule or house style, **[evidence]** backed by measurement or a study, **[mechanism]** browser/OS behaviour that you have to work with.

---

## 1. Touch targets

| Rule | Value | Source | Kind |
|---|---|---|---|
| iOS hit region | at least **44x44 pt** ("a button needs a hit region of at least 44x44 pt — in visionOS, 60x60 pt") | https://developer.apple.com/design/human-interface-guidelines/buttons | convention |
| iOS accessibility table | 44x44 pt default (iOS/iPadOS); 28x28 pt minimum is listed for other platforms; 66x66 pt (tvOS) | https://developer.apple.com/design/human-interface-guidelines/accessibility | convention |
| Android | "Ensure all touch targets are at least 48 dp, even if this extends past the UI element visual" | https://developer.android.com/design/ui/mobile/guides/foundations/accessibility | convention |
| WCAG 2.2 SC 2.5.8 (AA) | at least **24x24 CSS px**, or spaced so a 24px circle centred on each target overlaps no other target. Exceptions: spacing, equivalent, inline, user-agent control, essential | https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html | legal/accessibility floor |
| WCAG 2.5.5 (AAA) | 44x44 CSS px | https://www.w3.org/WAI/WCAG22/Understanding/target-size-enhanced.html | accessibility target |
| Physical size | at least 1cm x 1cm; fingertip 1.6–2cm wide, thumb contact about 2.5cm (MIT Touch Lab); based on Parhi, Karlson & Bederson 2006; roughly 2mm between targets; spacing can't make up for a target that is too small | https://www.nngroup.com/articles/touch-target-size/ (Harley, 2019) | **evidence** |
| Grow the hit area, not the visual | "Make tap targets bigger, not the controls themselves" (HIG, macOS touch section; same idea as Android's "even if this extends past the visual") | https://developer.apple.com/design/human-interface-guidelines/gestures | convention |

Takeaway: the 44pt and 48dp figures are platform conventions, the 1cm figure has study evidence behind it, and 24px is only the legal floor. For a premium feel, aim for 44–48 CSS px and use a padded or pseudo-element hit area for small icons.

## 2. Press feedback, tap delay, highlight

- **Every custom button needs a press state.** "Without a press state, a button can feel unresponsive" — https://developer.apple.com/design/human-interface-guidelines/buttons [convention]
- **The 300 ms delay (history):** browsers waited about 300–350 ms after `touchend` to rule out a double-tap zoom. Chrome 32 (2014) dropped it for `width=device-width` pages, and Firefox and IE/Edge followed — https://developer.chrome.com/blog/300ms-tap-delay-gone-away. WebKit (Dec 2015) made taps fast on `width=device-width` pages at initial scale, on unscalable viewports, and on elements with `touch-action: manipulation`, which works at any zoom level — https://webkit.org/blog/5610/more-responsive-tapping-on-ios/. The Chrome post dates the iOS fix to iOS 9.3 (Mar 2016). [mechanism] **Today:** keep `width=device-width` and you get no delay. `touch-action: manipulation` is a belt-and-braces addition for zoomed states. FastClick is obsolete.
- **`touch-action`:** `manipulation` = `pan-x pan-y pinch-zoom` (no double-tap zoom). The value is read when a gesture *starts*, so set it in advance. `none` can block zoom, which is an accessibility risk — https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action
- **`:active` on iOS:** "the `:active` pseudo state is triggered only when there is a touch event set on the HTML element", for example `ontouchstart=""` (Apple Safari Web Content Guide, archived) — https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/AdjustingtheTextSize/AdjustingtheTextSize.html. A common fix is one empty passive `touchstart` listener on `document`, or pointer-event-driven press classes. [mechanism; verify on current iOS]
- **`-webkit-tap-highlight-color`:** non-standard. It sets the grey/blue tap flash, and `transparent` removes it. Removing it takes away the browser's only feedback, so add your own `:active`/pressed style — https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-tap-highlight-color
- **`user-select: none` on UI chrome** (buttons, tabs, toolbars) stops a long-press from selecting text. Never apply it to content — https://web.dev/articles/app-like-pwas (Steiner, 2020)
- **`-webkit-text-size-adjust`:** iOS inflates text on rotation unless it is set. Default `auto` on iPhone, `none` on iPad — Safari Web Content Guide (same URL as above).
- **Input zoom:** iOS Safari zooms into a focused field whose computed font-size is below 16px and does not zoom back out. **There is no Apple primary source for this.** It is documented in secondary sources such as https://weblog.west-wind.com/posts/2023/Apr/17/Preventing-iOS-Textbox-Auto-Zooming-and-ViewPort-Sizing. Fix it with field font-size >= 16px, not `maximum-scale=1`/`user-scalable=no` (MDN: blocking zoom fails low-vision users; iOS 10+ ignores `user-scalable=no` by default — https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport). [mechanism, secondary]

## 3. Haptics

### Apple (HIG "Playing haptics", https://developer.apple.com/design/human-interface-guidelines/playing-haptics)
- "Use system-provided haptic patterns according to their documented meanings." Don't reuse a pattern to mean something else.
- "Use haptics consistently", building "a clear, causal relationship" between each haptic and its action.
- "Prefer using haptics to complement other feedback": match the haptic's intensity and sharpness to the animation.
- "Avoid overusing haptics … the best haptic experience is one that people may not be conscious of, but miss when it's turned off."
- In apps, "prefer playing short haptics that complement discrete events"; long-running haptics dilute meaning.
- "Make haptics optional": people should be able to mute them and still use the app.
- Make sure haptics don't disrupt the camera, gyroscope or microphone.
- iOS patterns and their meanings:
  - **Notification:** success (task completed), warning, error.
  - **Impact:** light, medium or heavy (size or weight of the colliding UI objects), rigid, soft ("a tap when a view snaps into place").
  - **Selection:** "a UI element's values are changing" (pickers, scrubbing through discrete values).
- Standard switches, sliders and pickers already play system haptics.
- HIG Accessibility: pair audio cues with matching haptics — https://developer.apple.com/design/human-interface-guidelines/accessibility

### Android (https://developer.android.com/develop/ui/views/haptics/haptics-principles)
- "Less is more." Use *clear* haptics (crisp, discrete) and *rich* haptics; avoid *buzzy* ones. "Given the choice of buzzy haptics or no haptics for touch feedback, choose no haptics."
- Prefer the action-oriented `HapticFeedbackConstants` over raw vibration. The platform gives consistent effects and fallbacks.
- "Correlate event importance and frequency with strength": very frequent events (scroll, text handle) should be very subtle. Refresh or submit can be stronger than a toggle.
- Avoid legacy `createOneShot`/`vibrate(long)` for feedback. "A good keyclick haptic feedback signal should last between 10 to 20 milliseconds", and actuators ring for another 20–50 ms. [evidence-ish engineering figure]
- Since API 33 only privileged apps can ignore the user's touch-feedback setting — https://developer.android.com/reference/android/view/HapticFeedbackConstants
- Constant → event (same reference): `CONFIRM`/`REJECT` (success/failure, API 30), `LONG_PRESS`, `CONTEXT_CLICK`, `DRAG_START` (picked up, API 34), `GESTURE_THRESHOLD_ACTIVATE`/`DEACTIVATE` (pull-to-refresh style threshold crossed or un-crossed, API 34), `SEGMENT_TICK` (list items or slider detents, API 34), `SEGMENT_FREQUENT_TICK` (dense choices, "very soft"; may do nothing), `CLOCK_TICK`, `TEXT_HANDLE_MOVE`, `TOGGLE_ON`/`TOGGLE_OFF` (API 34), `KEYBOARD_TAP`, `VIRTUAL_KEY`, `GESTURE_START`/`END`, `NO_HAPTICS`.

### Event-to-haptic mapping (derived from the two sources above)
| Event | iOS | Android | Tauri plugin call |
|---|---|---|---|
| Save/commit succeeded | notification success | CONFIRM | `notificationFeedback('success')` |
| Action refused/failed | notification error | REJECT | `notificationFeedback('error')` |
| Warning (destructive confirm shown, limit hit) | notification warning | (REJECT or none) | `notificationFeedback('warning')` |
| Toggle on/off | switch's system haptic | TOGGLE_ON/OFF | `selectionFeedback()` or `impactFeedback('light')` |
| Stepping through discrete values (key picker, transpose, tempo detent) | selection | SEGMENT_TICK | `selectionFeedback()` |
| Drag picked up / dropped into place | impact light/medium ("snaps into place") | DRAG_START / CONFIRM | `impactFeedback('light'\|'medium')` |
| Long-press opened a menu | (system context menu plays it) | LONG_PRESS | `impactFeedback('medium')` |
| Pull/swipe crossed the commit threshold | impact light | GESTURE_THRESHOLD_ACTIVATE | `impactFeedback('light')` |
| Plain button tap, scrolling, every keystroke | none (overuse) | none / very subtle | none |

### Web and Tauri
- **Vibration API** (`navigator.vibrate(ms|pattern)`) — https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API. **No support in iOS Safari at any version**; Firefox desktop dropped it from 129; Chrome Android supports it — https://caniuse.com/vibration. A web build therefore gets no haptics on iPhone. Chromium also requires prior user activation (MDN/Chromium behaviour; not stated on the MDN page I read). Android's own guidance warns that raw one-shot durations feel buzzy.
- **Tauri v2 haptics plugin** (`@tauri-apps/plugin-haptics`, Android and iOS only; register under `#[cfg(mobile)]`). Each command needs a capability: `haptics:allow-impact-feedback`, `-notification-feedback`, `-selection-feedback`, `-vibrate` — https://v2.tauri.app/plugin/haptics/. API: `impactFeedback('light'|'medium'|'heavy'|'soft'|'rigid')`, `notificationFeedback('success'|'warning'|'error')`, `selectionFeedback()`, `vibrate(ms)` — https://v2.tauri.app/reference/javascript/haptics/. These map onto the native iOS generators, which makes this the only route to real iPhone haptics for a WebView app. Feature-detect, and make every haptic optional (HIG).

## 4. Gestures and back navigation

- HIG: respond to standard gestures as people expect (tap activates). Don't give a familiar gesture an app-unique action, and don't invent a gesture for a standard action. Custom gestures must be discoverable, easy to perform, distinct, and "not the only way to perform an important action". Shortcut gestures supplement visible controls ("people expect to find a Back button … many apps also offer a shortcut gesture … while continuing to provide the Back button"). Avoid conflicts with system edge gestures. Show when a gesture isn't available. "Handle gestures as responsively as possible" with continuous feedback — https://developer.apple.com/design/human-interface-guidelines/gestures [convention]
- Android: "Don't rely on gestures to complete all actions" (e.g. swipe-to-delete also needs a delete button) — https://developer.android.com/design/ui/mobile/guides/foundations/accessibility
- **System gesture zones:** on Android 10+ the left and right edges are back-gesture insets and the bottom edge is home. Keep swipeable views (carousels, sheets) out of them — https://developer.android.com/develop/ui/views/layout/edge-to-edge
- **Predictive back (Android):** an animated preview of the back destination. Since Android 15 the system back-to-home, cross-task and cross-activity animations show by default for opted-in apps. Opt out with `android:enableOnBackInvokedCallback="false"`. An enabled `OnBackPressedCallback`/`OnBackInvokedCallback` stops the system animation, so enable a callback only when there is in-app back to handle (open sheet, unsaved form, history). "Don't create these callbacks to run business logic or to log." At the root, let the system take back to home — https://developer.android.com/guide/navigation/custom-back/predictive-back-gesture
- **In a WebView shell:** on iOS, `WKWebView.allowsBackForwardNavigationGestures` turns on the edge-swipe back/forward through web history and is off by default — https://developer.apple.com/documentation/webkit/wkwebview/allowsbackforwardnavigationgestures. On Android, Tauri's JS API gained `onBackButtonPress` in `@tauri-apps/api` 2.9.0 (PR https://github.com/tauri-apps/tauri/pull/14133; release https://v2.tauri.app/release/@tauri-apps/api/v2.9.0/). Native-feeling rule: back closes the topmost overlay (sheet, menu, dialog) first, then pops in-app history, then leaves the app at the root. Never trap back.

## 5. Sheets vs modals

- HIG Sheets (https://developer.apple.com/design/human-interface-guidelines/sheets):
  - A sheet is for a scoped task you return from.
  - For complex or long flows, use a full-screen modal instead.
  - "Display only one sheet at a time": close the first sheet before opening another.
  - Pair Done with Cancel (or Back); never show all three. On iOS, Cancel goes leading and Done trailing.
  - Detents are large and medium (about half height). Use medium for progressive disclosure, as the share sheet does. Compose sheets (Mail, Messages) stay full height.
  - "Include a grabber in a resizable sheet". Tapping it cycles through detents.
  - iOS sheets can be nonmodal, e.g. the Notes format sheet.
- HIG Modality: use modality "only when there's a clear benefit". Keep modal tasks short. Avoid "an app within your app"; a modal hierarchy should have one path — https://developer.apple.com/design/human-interface-guidelines/modality
- NN/g (Laubheimer, 2023, practitioner research) — https://www.nngroup.com/articles/bottom-sheet/:
  - Use sheets for short, contextual tasks; don't stack them; don't use them in place of page flows.
  - Back (button or gesture) must dismiss the sheet.
  - Add a visible Close (X), because people miss the grab handle and swipe-down can trigger system panels.
  - Bottom placement doesn't make a sheet easier to reach; the middle of the screen is the easiest area to tap.
- Web mechanics: put `overscroll-behavior: contain` (or `none`) on the sheet's scroller so the page behind doesn't scroll-chain or pull-to-refresh — https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior. Protect unsaved edits from swipe-dismiss by asking to confirm before discarding (HIG sheets; SwiftUI's `interactiveDismissDisabled` pattern).

## 6. Safe areas, edge-to-edge, status bar

- HIG: "Respecting the safe area is essential" so the Dynamic Island, home indicator and bars don't obstruct controls. Extend backgrounds full-bleed; keep controls inside the safe area — https://developer.apple.com/design/human-interface-guidelines/layout
- Web: `env(safe-area-inset-top|right|bottom|left, fallback)` — https://developer.mozilla.org/en-US/docs/Web/CSS/env. The insets are only meaningful with `<meta name="viewport" content="…, viewport-fit=cover">`; `cover` "scaled to fill the device display", use the insets so content isn't hidden — https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport. `safe-area-max-inset-*` are the static maxima.
- **Android 15 (target SDK 35) enforces edge-to-edge:** the window draws behind the status and nav bars. Pad tappable views by the system-bar insets and keep the last list item clear of the nav bar — https://developer.android.com/develop/ui/views/layout/edge-to-edge. For a WebView shell, the native side has to get the insets into CSS (the WebView's `env()` values or a CSS variable injected from native). Test this on device. [mechanism]
- **Chrome Android 135+ edge-to-edge:** with `viewport-fit=cover`, bottom-fixed UI should use `bottom: env(safe-area-inset-bottom)`, not padding (padding causes layout thrash and Chrome stops retracting the chin). Use the `safe-area-max-inset-bottom` calc pattern — https://developer.chrome.com/docs/css-ui/edge-to-edge
- **Status bar / chrome colour:** `<meta name="theme-color" content="…" media="(prefers-color-scheme: dark)">` with separate light and dark variants — https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/theme-color. Legacy iOS home-screen apps: `apple-mobile-web-app-capable`, `apple-mobile-web-app-status-bar-style` (only in standalone), `apple-touch-icon`, `apple-touch-startup-image` — https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html. In Tauri the native shell owns status-bar icon contrast. On Android, `isAppearanceLightStatusBars` must follow the app theme (edge-to-edge page above).

## 7. Viewport height and the keyboard

- `100vh` is too tall on mobile because it ignores dynamic toolbars. Use `svh` (toolbars shown, stable), `lvh` (toolbars hidden, stable) or `dvh` (tracks the toolbars). `dvh` updates are throttled, not 60fps, so use `svh`/`dvh` for app shells and avoid animating against `dvh`. Support: Chrome/Edge 108, Firefox 101, Safari 15.4 — https://web.dev/blog/viewport-units
- **Keyboard:** since Chrome 108 the on-screen keyboard resizes only the *visual* viewport, matching iOS. `vh` stays stable, and `position: fixed` stays put but can be covered. `interactive-widget=resizes-content` restores layout resizing (Chromium only; no effect in iOS) — https://developer.chrome.com/blog/viewport-resize-behavior. For a composer pinned above the keyboard, use the `visualViewport` resize/scroll events, or on Chromium the VirtualKeyboard API (`navigator.virtualKeyboard.overlaysContent = true`, `geometrychange`, `env(keyboard-inset-height)`; experimental, limited availability) — https://developer.mozilla.org/en-US/docs/Web/API/VirtualKeyboard_API
- **Keyboard type and enter key:** `inputmode` (numeric, decimal, tel, email, url, search) picks the keyboard; `enterkeyhint` (enter, done, go, next, previous, search, send) labels its action key — https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/enterkeyhint. Native apps always set these, and their absence is a "website" tell. [convention]

## 8. Scrolling

- **Momentum:** Safari 13 (iOS 13, Sept 2019) made accelerated one-finger scrolling the default for all frames and `overflow: scroll` areas, so `-webkit-overflow-scrolling: touch` is obsolete — https://developer.apple.com/documentation/safari-release-notes/safari-13-release-notes [mechanism]
- **Scroll chaining, bounce, pull-to-refresh:** `overscroll-behavior: contain` stops chaining; `none` also stops bounce and refresh. An `overflow: hidden` container is always at its edge, so `contain` there blocks chaining (overlay scroll lock) — https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior. An app shell usually sets `overscroll-behavior-y: none` (or `contain`) on the root to stop browser pull-to-refresh, then offers its own refresh where it means something — https://web.dev/articles/app-like-pwas
- **Passive listeners:** a non-passive `touchstart`/`touchmove`/`wheel` listener makes the browser wait for it before it can scroll. Declare `{ passive: true }` unless you call `preventDefault()` — https://developer.chrome.com/docs/lighthouse/best-practices/uses-passive-event-listeners
- **Scroll snap** (`scroll-snap-type`, `scroll-snap-align`) gives native-feeling carousels and paging without JS — https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type ; recommended by https://web.dev/articles/app-like-pwas

## 9. Hover-only affordances

- `@media (hover: none)` means the primary input can't hover conveniently ("many mobile devices emulate hover on an inconvenient long tap"); `(hover: hover)` means it can — https://developer.mozilla.org/en-US/docs/Web/CSS/@media/hover. Gate hover styles behind `(hover: hover)` so they don't "stick" after a tap. Never put a control or information *only* on hover. Use `pointer: coarse` to enlarge targets. [mechanism + convention]
- HIG macOS touch section: give touch equivalents for pointer-only actions, such as a "menu when the pointer moves over an element" — https://developer.apple.com/design/human-interface-guidelines/gestures

## 10. Appearance: dark mode, type, motion

- HIG Dark Mode: people "expect all apps and games to respect their preference". "Avoid offering an app-specific appearance setting". Rarely, a dark-only UI makes sense for media or immersive use (Stocks). Dark colours are not plain inversions. Test with Increase Contrast and Reduce Transparency — https://developer.apple.com/design/human-interface-guidelines/dark-mode [convention; note HIG discourages an in-app theme picker, which many cross-platform apps offer anyway]
- Web: `prefers-color-scheme`, `color-scheme`, and `font-family: system-ui` to match the platform font — https://web.dev/articles/app-like-pwas
- Text size: HIG asks for at least 200% enlargement (Dynamic Type) — https://developer.apple.com/design/human-interface-guidelines/accessibility. Android: use sp, body text no smaller than 12sp, contrast 4.5:1 for text and 3:1 for non-text — https://developer.android.com/design/ui/mobile/guides/foundations/accessibility
- Motion: honour `prefers-reduced-motion: reduce` by replacing scale and pan with fades (vestibular disorders) — https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion

## 11. Keeping the screen awake

- `navigator.wakeLock.request('screen')` → `WakeLockSentinel`. Requires HTTPS and the `screen-wake-lock` permissions policy (default `self`). The lock is released when the page is hidden, so re-request it on `visibilitychange`; a released sentinel can't be reused. It can be refused in low battery or power-save mode. Show users when the lock is on, let them turn it off, and release it as soon as the activity ends — https://developer.mozilla.org/en-US/docs/Web/API/Screen_Wake_Lock_API
- Support: iOS Safari 16.4+, Chrome Android, Firefox 126+ — https://caniuse.com/wake-lock. **Caveat:** MDN's compat data carries a note that it did not work in *standalone Home Screen web apps* on iOS 16.4–18.4, and the bug thread has user reports of failures (https://bugs.webkit.org/show_bug.cgi?id=254545). Whether 18.4+ fixed it isn't confirmed, so test installed-PWA mode on device. WKWebView inside a Tauri app is a different embedder again; a native `isIdleTimerDisabled` (iOS) / `FLAG_KEEP_SCREEN_ON` (Android) path is the reliable fallback. [unverified on device]
- web.dev cites lyrics display as a canonical wake-lock use case — https://web.dev/articles/app-like-pwas

## 12. Offline

- web.dev offline UX (Kurtuldu & Steiner, 2016) — https://web.dev/articles/offline-ux-design-guidelines:
  - Tell people the state as soon as it changes (e.g. a toast), and say whether the cause is their connection or your server.
  - Keep browsing working and disable only the actions that need the network.
  - Don't block with full-screen loaders.
  - Queue writes, show that they are "saved locally, will sync", and sync promptly.
  - Show last-updated times.
  - Use plain action words, and avoid the word "offline" and jargon.
  - Don't signal state by colour alone.
  - Use skeletons, not spinners.
- App-shell plus Cache Storage/IndexedDB; Persistent Storage to resist eviction; Background Sync for deferred writes — https://web.dev/articles/app-like-pwas
- `navigator.onLine` is unreliable: `true` doesn't mean internet, and the value updates lazily. Use it only for hints, never to disable features, and treat failed requests as the truth — https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine

## 13. Convention vs evidence

- **Evidence-backed:** a physical target size around 1cm (NN/g citing Parhi et al. 2006 and MIT Touch Lab); short 10–20 ms haptic clicks, because longer one-shots ring and feel buzzy (Android engineering guidance); the 24px WCAG floor (a standard built on research); reduced motion for vestibular disorders (MDN).
- **Platform convention (match it, because users learned it):** 44pt/48dp; Cancel leading and Done trailing; one sheet at a time; the grabber; back closes the top layer; edge-swipe back on iOS; predictive-back previews; a haptic's meaning tied to its pattern; following the system dark mode; system font.
- **Browser mechanism (not taste):** tap delay rules, `:active` on iOS, `dvh` throttling, keyboard viewport behaviour, iOS input zoom under 16px (secondary sources only), no Vibration API on iOS, wake-lock gaps in standalone mode, Android 15 edge-to-edge enforcement.
- **Gaps in these notes:** Material 3 pages (m3.material.io) render client-side and couldn't be read, so the Material rules come from developer.android.com instead. iOS input zoom has no Apple primary source. The Tauri Android default back behaviour without a handler wasn't confirmed in the docs. Nothing here was checked on a device.
