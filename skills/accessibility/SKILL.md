---
name: accessibility
description: Audit and fix web and mobile UI against WCAG 2.2 AA — contrast, semantics and ARIA, accessible names, keyboard and focus, dialogs and menus, forms and errors, status messages, images and media, zoom, reflow and text spacing, reduced motion and forced colours, VoiceOver, TalkBack and Dynamic Type — with the automated-testing limits and the manual checks that close them. Rules traced to W3C (WCAG 2.2, WAI-ARIA, APG, accname), the HTML standard, Apple HIG, Android Developers and MDN. Use when building or reviewing any screen, component or form, when asked for an accessibility or WCAG audit, or before shipping UI.
---

# Accessibility

Target **WCAG 2.2 Level AA** ([W3C Rec](https://www.w3.org/TR/WCAG22/), also ISO/IEC 40500:2025). WCAG 3 is a working draft and not usable for conformance yet. Laws point at WCAG: the EU Accessibility Act has applied since 28 June 2025 (via EN 301 549), and the US ADA Title II rule requires WCAG 2.1 AA for public bodies (dates in [references](references/sources.md#10-legal-context)). Cite the success criterion (SC) in every finding.

Most sites fail the same few things. In the [WebAIM Million 2026](https://webaim.org/projects/million/), 95.9% of home pages had detectable failures: low-contrast text 83.9%, missing alt 53.1%, unlabelled inputs 51.0%, empty links 46.3%, empty buttons 30.6%, missing `lang` 13.5%. Check those first.

## 1. Semantics before ARIA

- **Use the native element.** `<button>`, `<a href>`, `<label>`, `<select>`, `<dialog>`, `<details>`. If HTML already has the role and behaviour, don't rebuild it with `div` + `role` ([Using ARIA, first rule](https://www.w3.org/TR/using-aria/)). "No ARIA is better than bad ARIA" ([APG](https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/)).
- A clickable `div` is a bug: no focus, no Enter/Space, no role. Links go somewhere, buttons do something.
- Landmarks (`header`, `nav`, `main`, `footer`) and real headings in order give structure ([SC 1.3.1](https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html), [2.4.6](https://www.w3.org/WAI/WCAG22/Understanding/headings-and-labels.html)). One `h1` with no skipped levels is a convention, not a WCAG rule.
- `<html lang>` set, and `lang` on passages in another language ([3.1.1](https://www.w3.org/WAI/WCAG22/Understanding/language-of-page.html)).

## 2. Every control has a name

- Icon buttons, links and inputs need an accessible name ([4.1.2](https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html)). Order of precedence: `aria-labelledby`, `aria-label`, native label (`<label>`, `alt`, `legend`), content, then `title`/placeholder as a last resort ([accname 1.2](https://www.w3.org/TR/accname-1.2/)).
- The visible label is contained in the accessible name, so voice users can say what they see ([2.5.3](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html)).
- Link text says where it goes; no "click here" out of context ([2.4.4](https://www.w3.org/WAI/WCAG22/Understanding/link-purpose-in-context.html)).
- Images: informative ones get `alt` that conveys the information, decorative ones `alt=""` ([1.1.1](https://www.w3.org/WAI/WCAG22/Understanding/non-text-content.html), [W3C alt decision tree](https://www.w3.org/WAI/tutorials/images/decision-tree/)).

## 3. Keyboard and focus

- Everything works from the keyboard ([2.1.1](https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html)) with no trap ([2.1.2](https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html)), in an order that matches meaning ([2.4.3](https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html)). No positive `tabindex`.
- Focus is always visible ([2.4.7](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html)): use `:focus-visible`, never `outline: none` without a replacement. Aim for a 2 px ring at 3:1 ([2.4.13](https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance.html), AAA).
- Sticky headers, cookie banners and toolbars never hide the focused element entirely ([2.4.11](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html), new in 2.2). Use `scroll-padding`.
- A skip link or landmarks let people bypass repeated blocks ([2.4.1](https://www.w3.org/WAI/WCAG22/Understanding/bypass-blocks.html)).
- Anything done by dragging also works with a single tap or click ([2.5.7](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html), new in 2.2).

## 4. Widgets: follow the APG pattern

- **Modal dialog**: prefer native `<dialog>` with `showModal()`, which makes the page inert, closes on Esc and sits in the top layer ([HTML](https://html.spec.whatwg.org/multipage/interactive-elements.html#the-dialog-element)). Focus moves in on open (first control, or the title for long content), Tab cycles inside, and focus returns to the opener on close ([APG dialog](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)).
- **Site navigation is a disclosure, not a menu.** `role="menu"` is for application menus with arrow-key behaviour ([APG disclosure nav](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/), [menubar](https://www.w3.org/WAI/ARIA/apg/patterns/menubar/)).
- Tabs, comboboxes, listboxes: copy the [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) roles and keys exactly, or use a native control.
- Tooltips and hover cards can be dismissed with Esc, hovered without vanishing, and stay until dismissed ([1.4.13](https://www.w3.org/WAI/WCAG22/Understanding/content-on-hover-or-focus.html)).

## 5. Forms

- Every input has a programmatic `<label>`; a placeholder is not a label ([1.3.1](https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html), [3.3.2](https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions.html)).
- Errors are identified in text beside the field and suggest the fix ([3.3.1](https://www.w3.org/WAI/WCAG22/Understanding/error-identification.html), [3.3.3](https://www.w3.org/WAI/WCAG22/Understanding/error-suggestion.html)). Wire them with `aria-invalid="true"` and `aria-describedby` ([ARIA21](https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA21), [ARIA1](https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA1)); move focus to the first error on submit.
- `autocomplete` tokens on personal data ([1.3.5](https://www.w3.org/WAI/WCAG22/Understanding/identify-input-purpose.html)). Don't ask again for what the user already gave in the same flow ([3.3.7](https://www.w3.org/WAI/WCAG22/Understanding/redundant-entry.html)).
- Login allows paste and password managers; no puzzle or memory test without an alternative ([3.3.8](https://www.w3.org/WAI/WCAG22/Understanding/accessible-authentication-minimum.html)).
- Results that appear without a focus change ("Saved", "3 results", "Added to cart") are announced with `role="status"` (polite) or `role="alert"` (urgent only) ([4.1.3](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html)). The live region must already be in the DOM before text is put in it (convention, widely reported).

## 6. Seeing it

- **Contrast** ([1.4.3](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html)): 4.5:1 for text, 3:1 for large text (≥24 px, or ≥18.66 px bold). **Non-text** ([1.4.11](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html)): 3:1 for input borders, control states, focus rings and meaningful icons and chart marks. Check placeholders, disabled-looking-but-active text, text on images, and dark mode separately.
- **Never colour alone** ([1.4.1](https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html)): errors, required fields, chart series and links in body text also get text, an icon, a pattern or an underline.
- **Zoom and reflow**: text resizes to 200% ([1.4.4](https://www.w3.org/WAI/WCAG22/Understanding/resize-text.html)); at 320 CSS px wide nothing needs horizontal scrolling except maps, tables and toolbars ([1.4.10](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html)). Size text in `rem`, never block pinch zoom.
- **Text spacing** ([1.4.12](https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html)): nothing clips at line-height 1.5, paragraph spacing 2×, letter spacing 0.12 em, word spacing 0.16 em. Avoid fixed heights on text boxes.
- **Targets** at least 24 × 24 CSS px or spaced so a 24 px circle doesn't overlap a neighbour ([2.5.8](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html)); 44 pt on iOS, 48 dp on Android.

## 7. Motion and user preferences

- Anything that moves by itself for more than 5 s can be paused ([2.2.2](https://www.w3.org/WAI/WCAG22/Understanding/pause-stop-hide.html), Level A); nothing flashes more than three times a second ([2.3.1](https://www.w3.org/WAI/WCAG22/Understanding/three-flashes-or-below-threshold.html)).
- Honour `prefers-reduced-motion` ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion)); the strict rule is [2.3.3](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html) (AAA). Details in `ui-motion`.
- **Forced colours** (Windows contrast themes): states drawn only with `box-shadow` or background images disappear. Use transparent borders and outlines, which get repainted, and system colours such as `CanvasText` and `Highlight` ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/forced-colors)). Use `forced-color-adjust: none` sparingly.

## 8. Native apps

- **iOS**: support Dynamic Type to at least 200%; controls 44 × 44 pt by default; WCAG AA contrast in both light and dark mode, more under Increase Contrast; Reduce Motion swaps slides and zooms for fades ([Apple HIG](https://developer.apple.com/design/human-interface-guidelines/accessibility)). `accessibilityLabel` is short and leaves out the control type, which VoiceOver adds from traits.
- **Android**: targets 48 × 48 dp; `contentDescription` on non-text elements without the role ("Submit", not "Submit button"), unique within lists, null for decoration; none on Text composables ([Android](https://developer.android.com/guide/topics/ui/accessibility/apps)).
- WCAG applies to native apps through [WCAG2ICT](https://www.w3.org/TR/wcag2ict-22/) (W3C Note).

## Testing: automation is a floor

Automated tools find only part of it. In the UK GDS test of 143 planted barriers, the best single tool found 37–41% and all ten together about 71% ([GDS 2017](https://accessibility.blog.gov.uk/2017/02/24/what-we-found-when-we-tested-tools-on-the-worlds-least-accessible-webpage/)); Deque's figure of 57% counts issue volume and is a vendor number ([Deque](https://www.deque.com/automated-accessibility-testing-coverage/)). A Lighthouse 100 is not conformance. So:

1. Run [axe-core](https://github.com/dequelabs/axe-core) (or the platform scanner) and fix everything; review its "incomplete" items by hand.
2. Unplug the mouse: Tab through every flow, open and close every dialog and menu, submit every form with errors.
3. Zoom to 200% and 400%, and narrow to 320 px. Apply the 1.4.12 text-spacing bookmarklet.
4. Turn on reduced motion, forced colours and dark mode.
5. Screen reader pass on the main flow: VoiceOver + Safari (macOS and iOS), NVDA + Chrome or Firefox, TalkBack + Chrome. JAWS + Chrome is the most used desktop pairing ([WebAIM survey #11](https://webaim.org/projects/screenreadersurvey11/)) but needs a licence.

Report anything you couldn't test (for example no real device or screen reader).

## Review output

| Severity | Where | Problem | Fix | SC |
| --- | --- | --- | --- | --- |
| High | `IconButton.tsx:12` | Delete button has no accessible name | `aria-label="Delete project"` | 4.1.2 (A) |

**High** blocks a task for some users (no keyboard access, no name, a trap, unlabelled form, contrast well under). **Medium** makes it hard (focus hidden, errors only in colour, reflow breaks). **Low** is AAA or best practice.

## Checklist

- [ ] Native elements first; no clickable `div`; `lang` set.
- [ ] Every button, link, input and informative image has a name; decorative images `alt=""`.
- [ ] Full keyboard path, visible focus, never hidden by sticky UI; skip link.
- [ ] Dialogs move, trap and return focus; nav is a disclosure, not `role="menu"`.
- [ ] Labels on every input; errors in text, wired with `aria-describedby`; paste allowed in login.
- [ ] Status messages announced via `role="status"`.
- [ ] Text 4.5:1, large text and UI parts 3:1, in light and dark; never colour alone.
- [ ] 200% zoom, 320 px reflow, text spacing all survive; targets ≥24 px.
- [ ] Reduced motion and forced colours work; no auto-moving content without pause.
- [ ] axe clean, then keyboard, zoom and one screen-reader pass done.

Full notes and every source: [references/sources.md](references/sources.md).

**Finish with `taste`.** Any screen or component you build or change with this skill gets the `taste` pass before you call it done.
