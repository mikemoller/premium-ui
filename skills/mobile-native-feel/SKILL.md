---
name: mobile-native-feel
description: Make a mobile web app, PWA or WebView shell (Tauri, Capacitor) feel native instead of "a website in a wrapper" — touch targets, press states, haptics per event (iOS, Android, Tauri plugin), gestures and the back button, sheets, safe areas and Android 15 edge-to-edge, viewport units and the on-screen keyboard, scrolling and pull-to-refresh, hover-free affordances, dark mode, keeping the screen awake, and offline. Traced to Apple HIG, Android Developers, MDN, web.dev, WCAG and Tauri docs. Use when building or reviewing anything that runs on a phone, or when a mobile build feels "webby".
---

# Mobile native feel

People don't compare your app with other websites; they compare it with the native apps on their phone. Most "webby" tells come from browser defaults left in place: grey tap flashes, text that selects on long-press, a page that bounces behind a sheet, a keyboard that covers the input, a back button that leaves the app.

Rules below are tagged where it matters: **convention** (match it because people learned it), **evidence** (measured), **mechanism** (how the browser or OS behaves).

## 1. Touch targets

- At least **44 × 44 pt** on iOS ([Apple, Buttons](https://developer.apple.com/design/human-interface-guidelines/buttons)) and **48 × 48 dp** on Android ([Android accessibility](https://developer.android.com/design/ui/mobile/guides/foundations/accessibility)). Convention.
- The measured basis is physical: about **1 cm × 1 cm**, with ~2 mm between targets. Spacing doesn't make up for a target that's too small ([NN/g](https://www.nngroup.com/articles/touch-target-size/), citing Parhi et al. 2006). Evidence.
- [WCAG 2.5.8](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) (AA) sets a 24 px floor. That's the legal minimum, not the goal.
- **Grow the hit area, not the visual**: padding or a pseudo-element around a small icon. Use `@media (pointer: coarse)` to enlarge targets on touch.

## 2. Press feedback

- Every custom button needs a visible pressed state, or it feels unresponsive ([Apple, Buttons](https://developer.apple.com/design/human-interface-guidelines/buttons)).
- If you set `-webkit-tap-highlight-color: transparent`, you've removed the browser's only feedback, so add your own `:active` style ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-tap-highlight-color)).
- iOS Safari historically applied `:active` only to elements with a touch listener. One empty passive `touchstart` listener on `document`, or pointer-event press classes, fixes it. Mechanism; check on a current device.
- The 300 ms tap delay is gone on pages with `width=device-width`; add `touch-action: manipulation` for zoomed states ([WebKit](https://webkit.org/blog/5610/more-responsive-tapping-on-ios/), [Chrome](https://developer.chrome.com/blog/300ms-tap-delay-gone-away)). FastClick is obsolete.
- `user-select: none` on interface chrome (buttons, tabs, toolbars) so a long-press doesn't select text. Never on content ([web.dev, app-like PWAs](https://web.dev/articles/app-like-pwas)).
- Set `-webkit-text-size-adjust: 100%` so iOS doesn't inflate text on rotation.

## 3. Haptics

Rules both platforms share ([Apple, Playing haptics](https://developer.apple.com/design/human-interface-guidelines/playing-haptics), [Android, Haptics principles](https://developer.android.com/develop/ui/views/haptics/haptics-principles)):

- Use the system patterns **only for their documented meaning**, and always for the same event.
- **Less is more.** The best haptics are ones people don't notice until they're gone. No haptic on plain button taps, scrolling or typing.
- Short and crisp. Android: a click should last 10–20 ms; "given the choice of buzzy haptics or no haptics, choose no haptics."
- Haptics **accompany** visual feedback; they never carry information alone. Let people turn them off.

| Event | iOS | Android | Tauri `@tauri-apps/plugin-haptics` |
| --- | --- | --- | --- |
| Save or commit succeeded | notification success | `CONFIRM` | `notificationFeedback('success')` |
| Action failed or refused | notification error | `REJECT` | `notificationFeedback('error')` |
| Warning (limit hit) | notification warning | — | `notificationFeedback('warning')` |
| Stepping through values (picker, stepper detent) | selection | `SEGMENT_TICK` | `selectionFeedback()` |
| Toggle | system switch | `TOGGLE_ON` / `TOGGLE_OFF` | `selectionFeedback()` |
| Dragged item picked up / snaps into place | impact light/medium | `DRAG_START` / `CONFIRM` | `impactFeedback('light' \| 'medium')` |
| Gesture crosses its commit point | impact light | `GESTURE_THRESHOLD_ACTIVATE` | `impactFeedback('light')` |

- **The web Vibration API doesn't exist on iOS**, in any version ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API), [caniuse](https://caniuse.com/vibration)). A plain web build gets no iPhone haptics; Android raw vibration feels buzzy.
- In a Tauri app, the [haptics plugin](https://v2.tauri.app/plugin/haptics/) calls the native generators on iOS and Android. Each command needs its capability (`haptics:allow-impact-feedback` etc.). Feature-detect and no-op on the web.

## 4. Gestures and back

- Standard gestures do standard things. Don't give a familiar gesture a new meaning, and every gesture has a visible alternative: swipe-to-delete also has a Delete button ([Apple, Gestures](https://developer.apple.com/design/human-interface-guidelines/gestures), [Android](https://developer.android.com/design/ui/mobile/guides/foundations/accessibility)).
- Keep swipeable things (carousels, sheets) away from the screen edges; those belong to system back and home ([Android, edge-to-edge](https://developer.android.com/develop/ui/views/layout/edge-to-edge)).
- **Back closes the topmost layer first** (menu, sheet, dialog), then goes back in the app's history, then leaves the app at the root. Never trap back.
- Android predictive back (from Android 15 the system back animations show by default for apps that opt in): register a back handler **only while there's something in the app to close or go back to**; otherwise the system can show its preview animation ([Android, Predictive back](https://developer.android.com/guide/navigation/custom-back/predictive-back-gesture)). Tauri exposes `onBackButtonPress` (Android) from `@tauri-apps/api` 2.9.0.
- iOS `WKWebView` edge-swipe back is off by default (`allowsBackForwardNavigationGestures`); turn it on in the shell or provide a visible Back.
- Gestures follow the finger and commit destructive actions only on release (see `ui-motion`).

## 5. Sheets

- For short, scoped tasks; long or complex flows get a full screen ([Apple, Sheets](https://developer.apple.com/design/human-interface-guidelines/sheets), [Modality](https://developer.apple.com/design/human-interface-guidelines/modality)).
- **One sheet at a time.** Close the first before opening another.
- Pair Done with Cancel; never Done, Cancel and Back together.
- A grabber on resizable sheets; medium (about half height) and large detents.
- **A visible Close button** too: people miss the grabber, and swiping down from the top can open the system panel ([NN/g, Bottom sheets](https://www.nngroup.com/articles/bottom-sheet/)). Back dismisses the sheet.
- `overscroll-behavior: contain` on the sheet's scroll area so the page behind doesn't scroll or pull-to-refresh ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior)).
- Ask before a swipe-dismiss throws away unsaved edits.

## 6. Safe areas and edge-to-edge

- `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">`, then pad with `env(safe-area-inset-*)` ([MDN, env()](https://developer.mozilla.org/en-US/docs/Web/CSS/env)). Backgrounds run full-bleed; controls stay inside the safe area ([Apple, Layout](https://developer.apple.com/design/human-interface-guidelines/layout)).
- **Android 15 forces edge-to-edge** for apps targeting SDK 35: the app draws behind the status and navigation bars ([Android](https://developer.android.com/develop/ui/views/layout/edge-to-edge)). In a WebView shell, make sure the system-bar insets reach CSS (through `env()` or a variable the native side sets). Test on a device.
- Chrome Android 135+: position bottom-fixed bars with `bottom: env(safe-area-inset-bottom)`, not padding ([Chrome, edge-to-edge](https://developer.chrome.com/docs/css-ui/edge-to-edge)).
- `<meta name="theme-color">` with light and dark variants (`media="(prefers-color-scheme: dark)"`) so the browser chrome matches ([MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/theme-color)). In a native shell, set status-bar icon contrast from the app theme.

## 7. Viewport and keyboard

- Don't use `100vh` for full-height layouts; it ignores the mobile toolbars. Use `100svh` (stable) or `100dvh` (tracks toolbars; updates are throttled, so don't animate against it) ([web.dev, viewport units](https://web.dev/blog/viewport-units)).
- The on-screen keyboard resizes only the **visual** viewport on iOS and Chrome 108+. Fixed elements stay put and can be covered ([Chrome](https://developer.chrome.com/blog/viewport-resize-behavior)). To keep a composer above the keyboard, follow `visualViewport` resize and scroll events.
- Set `inputmode` (numeric, decimal, email, tel, url, search) and `enterkeyhint` (done, next, go, search, send) on every field ([MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/enterkeyhint)). Missing them is a giveaway.
- Inputs at **16 px or more**, or iOS zooms in on focus and doesn't zoom back. (Widely documented, but no Apple primary source.) Never fix it by disabling zoom.

## 8. Scrolling

- Momentum scrolling has been the default since iOS 13; `-webkit-overflow-scrolling: touch` is obsolete ([Safari 13 notes](https://developer.apple.com/documentation/safari-release-notes/safari-13-release-notes)).
- `overscroll-behavior-y: none` (or `contain`) on the root stops the browser's pull-to-refresh and bounce; add your own refresh only where it means something ([web.dev](https://web.dev/articles/app-like-pwas)).
- Touch and wheel listeners are `{ passive: true }` unless they call `preventDefault()`; a non-passive one delays every scroll ([Chrome](https://developer.chrome.com/docs/lighthouse/best-practices/uses-passive-event-listeners)).
- CSS scroll snap for carousels and paging, no JS needed ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type)).

## 9. No hover-only affordances

- Gate hover styles behind `@media (hover: hover)` so they don't stick after a tap ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/hover)).
- Nothing lives only on hover: actions revealed on row hover need a visible alternative on touch (a menu button, a long-press menu, swipe actions plus a button).

## 10. Appearance

- Follow the system dark mode by default; people expect it ([Apple, Dark Mode](https://developer.apple.com/design/human-interface-guidelines/dark-mode)). Set `color-scheme` so form controls and scrollbars match.
- Support large text: iOS asks for 200% enlargement, Android for `sp` units and body text ≥ 12 sp. Contrast 4.5:1 for text, 3:1 for icons and controls.
- `font-family: system-ui` where you want the platform's own font.
- Honour reduced motion (see `ui-motion`).

## 11. Keep the screen awake

- For apps people read from without touching (recipes, lyrics, sheet music, timers): `navigator.wakeLock.request('screen')` ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Screen_Wake_Lock_API)).
- The lock drops when the page is hidden: **request it again on `visibilitychange`**. It can be refused in power-save mode. Show that it's on, let people turn it off, release it when done.
- Support is iOS Safari 16.4+, Chrome Android, Firefox 126+, but it reportedly didn't work in installed Home Screen web apps on iOS 16.4–18.4, and a later fix isn't confirmed; test the installed app on a device ([WebKit bug](https://bugs.webkit.org/show_bug.cgi?id=254545)). In a native shell, the native keep-awake (`isIdleTimerDisabled` on iOS, `FLAG_KEEP_SCREEN_ON` on Android) is the reliable route.

## 12. Offline

- Say what state you're in as soon as it changes, and whether it's the connection or the server ([web.dev, Offline UX](https://web.dev/articles/offline-ux-design-guidelines)).
- Keep everything that doesn't need the network working; disable only what does. No full-screen blocking loaders.
- Queue writes and say so ("Saved on this device — will sync"); show when data was last updated.
- `navigator.onLine` is a hint, not the truth; a failed request is the truth ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine)).

## Review checklist (test on a real phone)

- [ ] Targets ≥ 44 px (48 on Android); small icons have padded hit areas.
- [ ] Every control has a pressed state; no grey tap flash; chrome doesn't select on long-press.
- [ ] Haptics only on meaningful events, native in the shell, none on the web build for iOS.
- [ ] Back closes the top layer, then goes back, then leaves; never trapped.
- [ ] Sheets: one at a time, Close button, back dismisses, no scroll-through.
- [ ] Safe areas respected in portrait and landscape; Android 15 bars don't cover controls.
- [ ] No `100vh`; keyboard never covers the focused input; inputs ≥ 16 px with `inputmode`/`enterkeyhint`.
- [ ] No browser pull-to-refresh or bounce behind the app; listeners are passive.
- [ ] Nothing is hover-only; dark mode follows the system.
- [ ] Screens people read hands-free stay awake; offline state is stated and writes queue.

Full notes and every source: [references/sources.md](references/sources.md).

**Finish with `taste`.** Any screen or component you build or change with this skill gets the `taste` pass before you call it done.
