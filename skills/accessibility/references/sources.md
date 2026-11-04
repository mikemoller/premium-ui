# Accessibility: primary-source notes

Compiled 2026-11-04. Key: **[N]** normative (W3C Recommendation, WHATWG standard or law), **[I]** informative (W3C Note, APG, platform guideline), **[Data]** empirical study, **[Vendor]** a tool maker's own figure. "Unverified" marks what I could not confirm against the primary text.

---

## 1. Which standard

- **WCAG 2.2** is the current W3C Recommendation: first published 5 Oct 2023, current edition 12 Dec 2024. https://www.w3.org/TR/WCAG22/ [N]. Content that meets 2.2 also meets 2.1 and 2.0.
- **ISO/IEC 40500:2025 = WCAG 2.2.** W3C announcement, 21 Oct 2025: https://www.w3.org/WAI/news/2025-10-21/wcag22-iso [N]
- **New in 2.2** (Understanding docs at `https://www.w3.org/WAI/WCAG22/Understanding/<slug>`):
  - 2.4.11 Focus Not Obscured (Minimum), AA: the focused element is not *entirely* hidden by author content. 2.4.12 (AAA) hides no part of it.
  - 2.4.13 Focus Appearance, AAA: indicator area at least a 2 CSS px perimeter, 3:1 between focused and unfocused states.
  - 2.5.7 Dragging Movements, AA: a single-pointer alternative to any drag, unless dragging is essential.
  - 2.5.8 Target Size (Minimum), AA: 24 × 24 CSS px. Exceptions: spacing (a 24 px circle centred on the target doesn't overlap another target or its circle), equivalent control elsewhere, inline in text, user-agent controlled, essential. 2.5.5 (AAA) is 44 × 44.
  - 3.2.6 Consistent Help, A. 3.3.7 Redundant Entry, A.
  - 3.3.8 Accessible Authentication (Minimum), AA: no cognitive function test unless there's an alternative or a mechanism to help; allowing paste and password managers satisfies it. 3.3.9 is the AAA version.
- **4.1.1 Parsing is obsolete and removed** in 2.2; errata to 2.0 and 2.1 say to treat it as always satisfied. Don't audit against it.
- **WCAG 3.0** is a Working Draft (10 Sep 2026) that calls itself inappropriate to cite except as work in progress. https://www.w3.org/TR/wcag-3.0/

## 2. What fails most often

**WebAIM Million 2026** (home pages crawled Feb 2026, published 30 Mar 2026; automated WAVE detection, so a floor): 95.9% of pages had detected WCAG 2 failures, 56.1 errors per page on average. https://webaim.org/projects/million/ [Data]

| Failure | Pages | Maps to |
| --- | --- | --- |
| Low-contrast text | 83.9% | 1.4.3 |
| Missing alt text | 53.1% | 1.1.1 |
| Missing form labels | 51.0% | 1.3.1, 4.1.2 |
| Empty links | 46.3% | 2.4.4, 4.1.2 |
| Empty buttons | 30.6% | 4.1.2 |
| Missing document language | 13.5% | 3.1.1 |

## 3. Semantics and ARIA

- "No ARIA is better than Bad ARIA": APG Read Me First, https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/ [I]
- First rule of ARIA use: use a native element or attribute that already has the semantics and behaviour. W3C "Using ARIA" Note, https://www.w3.org/TR/using-aria/ [I]. Normative backing: ARIA in HTML https://www.w3.org/TR/html-aria/ and WAI-ARIA 1.2 https://www.w3.org/TR/wai-aria-1.2/ [N]
- Accessible name computation, precedence: `aria-labelledby` → `aria-label` → native labelling (`<label>`, `alt`, `<caption>`, `<legend>`) → name from content where the role allows → `title`/`placeholder` last. https://www.w3.org/TR/accname-1.2/ [N]. Backs SC 4.1.2 Name, Role, Value (A).
- Landmarks and headings: SC 1.3.1 (A), 2.4.6 (AA). APG landmarks: https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/ [I]. "One h1, no skipped levels" is convention, not a WCAG requirement.
- **Modal dialog** (APG, https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/ [I]): Tab and Shift+Tab cycle inside; Esc closes; initial focus on the first focusable element, or on the title (`tabindex="-1"`) for long content, or on the least destructive action for irreversible steps; on close, focus returns to the opener. `role="dialog"`, `aria-modal="true"`, named via `aria-labelledby`.
- **Native `<dialog>` and `inert`** (WHATWG HTML [N]): https://html.spec.whatwg.org/multipage/interactive-elements.html#the-dialog-element, https://html.spec.whatwg.org/multipage/interaction.html#the-inert-attribute. `showModal()` makes the rest of the document inert, uses the top layer and closes on Esc. Focus return on close: in the current spec and browsers, but check your targets (unverified this session).
- **Menu vs disclosure**: APG menubar https://www.w3.org/WAI/ARIA/apg/patterns/menubar/ is for application menus with arrow-key behaviour. Site nav uses the disclosure pattern https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/ [I].
- Tabs https://www.w3.org/WAI/ARIA/apg/patterns/tabs/ (tablist/tab/tabpanel, `aria-selected`, arrow keys, roving tabindex). Combobox https://www.w3.org/WAI/ARIA/apg/patterns/combobox/ (ARIA 1.2 model; prefer native `<select>`/`<datalist>` where enough) [I].

## 4. Keyboard

All [N]: 2.1.1 Keyboard (A); 2.1.2 No Keyboard Trap (A), and a modal focus loop that Esc exits is not a trap; 2.4.3 Focus Order (A), where positive `tabindex` is a common failure; 2.4.7 Focus Visible (AA), where `outline: none` without replacement fails (`:focus-visible`, https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible); 2.4.1 Bypass Blocks (A), met by a skip link, landmarks or headings.

## 5. Forms and status messages

- 1.3.1 and 3.3.2 Labels or Instructions (A): programmatic labels; placeholder isn't a label. Technique H44 https://www.w3.org/WAI/WCAG22/Techniques/html/H44, failure F68.
- 2.5.3 Label in Name (A).
- 3.3.1 Error Identification (A), 3.3.3 Error Suggestion (AA).
- 1.3.5 Identify Input Purpose (AA): `autocomplete` tokens, https://www.w3.org/TR/WCAG22/#input-purposes and https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofill
- Techniques [I]: ARIA21 `aria-invalid` https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA21; ARIA1 `aria-describedby` https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA1
- 4.1.3 Status Messages (AA): techniques ARIA22 (`role="status"`) and ARIA19 (`role="alert"`). `status` is implicitly polite, `alert` assertive (WAI-ARIA 1.2). "The live region must exist before content is injected" is practice, widely reported, not normative text.

## 6. Images and media

- 1.1.1 Non-text Content (A). Decorative: `alt=""` or CSS background. W3C decision tree https://www.w3.org/WAI/tutorials/images/decision-tree/ [I]; HTML spec alt guidance https://html.spec.whatwg.org/multipage/images.html#alt [N].
- Media: 1.2.1–1.2.3 (A), 1.2.4 live captions and 1.2.5 audio description (AA); 1.2.6–1.2.9 AAA. https://www.w3.org/WAI/media/av/ [I]
- 1.4.2 Audio Control (A); 2.2.2 Pause, Stop, Hide (A) for anything moving more than 5 s.

## 7. Visual

All [N] unless noted:
- 1.4.1 Use of Color (A). Links in body text need a non-colour cue, or 3:1 against surrounding text plus a cue on hover/focus (technique G183).
- 1.4.3 Contrast (AA): 4.5:1; large text 3:1, large = 18 pt (≈24 CSS px) or 14 pt bold (≈18.66 px). Logos and inactive controls exempt. Don't round: 4.499:1 fails. 1.4.6 (AAA) 7:1 / 4.5:1. https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- 1.4.11 Non-text Contrast (AA): 3:1 for component boundaries, states, focus indicators and meaningful graphics.
- 1.4.4 Resize Text (AA) 200%. 1.4.10 Reflow (AA): 320 CSS px wide / 256 px high without two-direction scrolling (= 1280 px at 400%); maps, data tables and toolbars excepted.
- 1.4.12 Text Spacing (AA): a robustness test at line-height 1.5, paragraph 2×, letter 0.12 em, word 0.16 em. It is not a styling rule. The styling rule (≥1.5 line spacing, ≤80 characters) is 1.4.8, AAA.
- 1.4.13 Content on Hover or Focus (AA): dismissible, hoverable, persistent.
- 2.3.1 Three Flashes (A). 2.3.3 Animation from Interactions is **AAA**. `prefers-reduced-motion`: https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion
- `forced-colors`: https://developer.mozilla.org/en-US/docs/Web/CSS/@media/forced-colors; spec CSS Color Adjustment 1 https://www.w3.org/TR/css-color-adjust-1/ (Candidate Recommendation; current status unverified). Use system colours; `box-shadow` and background images as the only state indicator vanish.
- `prefers-contrast`: https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-contrast (Media Queries 5, Working Draft; weaker support).

## 8. Native apps

- **Apple HIG Accessibility** https://developer.apple.com/design/human-interface-guidelines/accessibility [I]: control size default/minimum iOS 44/28 pt, macOS 28/20, visionOS 60/28, tvOS 66/56. Contrast per WCAG AA (4.5:1 up to 17 pt, 3:1 at 18 pt or bold), check light and dark, offer more under Increase Contrast. Text scales to at least 200% (watchOS 140%) via Dynamic Type; iOS default/minimum text 17/11 pt. Reduce Motion: cut automatic and repetitive motion, swap axis transitions for fades.
- iOS label APIs: UIKit `accessibilityLabel`, SwiftUI `accessibilityLabel(_:)`. The label omits the control type, which VoiceOver adds from traits. (Apple doc slugs not verified.)
- **Android** https://developer.android.com/guide/topics/ui/accessibility/apps [I]: targets 48 × 48 dp; contrast 4.5:1 under 18 sp (14 sp bold), 3:1 above; `contentDescription` on non-text elements without the role, unique in lists, null for decoration; none on Text composables. Testing: Accessibility Scanner, Compose testing, Lint, https://developer.android.com/develop/ui/compose/accessibility/testing
- **WCAG2ICT** (applying WCAG 2.2 to non-web ICT, W3C Note): https://www.w3.org/TR/wcag2ict-22/ (slug assumed from the 2.2 edition; verify).

## 9. Testing

- **UK GDS, 24 Feb 2017** [Data, primary, small and dated]: 143 planted barriers in 19 categories, 10 tools. Best single tool 37–41%, lowest 17%; all combined about 71%; 29% missed by every tool. https://accessibility.blog.gov.uk/2017/02/24/what-we-found-when-we-tested-tools-on-the-worlds-least-accessible-webpage/
- **Deque, 57.38%** [Vendor]: share of issues found automatically across about 13,000 pages and 294,958 issues from first-time audits. Weighted by issue volume, where repetitive issues such as contrast dominate, not by share of success criteria. https://www.deque.com/automated-accessibility-testing-coverage/
- **axe-core** https://github.com/dequelabs/axe-core: WCAG 2.0–2.2 rules; undecidable items reported as "incomplete" for manual review.
- **Lighthouse** runs a subset of axe; a score of 100 is not conformance. https://developer.chrome.com/docs/lighthouse/accessibility/scoring (page content unverified).
- W3C: tools can't determine conformance alone, https://www.w3.org/WAI/test-evaluate/tools/selecting/; Easy Checks https://www.w3.org/WAI/test-evaluate/preliminary/; WCAG-EM https://www.w3.org/TR/WCAG-EM/ [I]
- **WebAIM Screen Reader Survey #11** (Jul–Aug 2026, n = 1,780, self-selected) https://webaim.org/projects/screenreadersurvey11/ [Data]: primary desktop reader JAWS 55.0%, NVDA 32.9%, VoiceOver 6.5%. Top combinations: JAWS+Chrome 31.2%, NVDA+Chrome 17.3%, JAWS+Edge 17.0%, NVDA+Firefox 9.1%. 91.8% also use a mobile reader: VoiceOver 72.2%, TalkBack 29.5%. Survey #10 (2023–24) had JAWS 40.5% / NVDA 37.7%; the lead has swapped over the years, so don't claim a stable winner.

## 10. Legal context

Date-stamped 2026-11-04; both are moving.
- **European Accessibility Act**, Directive (EU) 2019/882, https://eur-lex.europa.eu/eli/dir/2019/882/oj [N]: applies from 28 June 2025, with transitional periods (to 2030 for some services, longer for self-service terminals) and an exemption for microenterprises providing services. Article numbers not re-checked.
- **EN 301 549**: V3.2.1 (2021, WCAG 2.1 AA) is the harmonised standard. V4.1.1, aligned to WCAG 2.2, was reportedly published 2 Sep 2026 (secondary sources: NDA Ireland https://nda.ie/news/en301549-published). Whether it is cited in the Official Journal yet is **unverified**.
- **US ADA Title II rule** (DOJ, 24 Apr 2024): WCAG 2.1 AA for state and local government web content and mobile apps. https://www.ada.gov/resources/2024-03-08-web-rule/. An interim final rule (20 Apr 2026) pushed compliance to 26 Apr 2027 (population ≥50,000) and 26 Apr 2028 (smaller entities and special districts). https://www.govinfo.gov/content/pkg/FR-2026-04-20/html/2026-07663.htm

## Gaps

Not verified: Apple API doc slugs, the WCAG2ICT-22 slug, Lighthouse page content, EAA article numbers, CSS Color Adjust and MQ5 status, EN 301 549 V4.1.1 Official Journal citation, and native `<dialog>` focus return across all target browsers.
