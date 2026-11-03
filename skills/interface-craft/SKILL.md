---
name: interface-craft
description: The small details that make an interface feel carefully made — typography (tabular numbers, balanced wrapping), geometry (concentric radii, optical alignment, shadows vs borders), every interactive state, focus, hit areas, icons, empty and error states, undo instead of confirm, remembering where people were, keyboard and command palettes, and microcopy. Rules traced to Apple HIG, NN/g, WCAG 2.2, MDN, Vercel's Web Interface Guidelines and teams known for craft. Use when building or reviewing components and screens, polishing UI, or when something "feels off", cheap or unfinished.
---

# Interface craft

Polish is a pile of small, boring decisions made the same way everywhere. Work in the project's existing styling system and design tokens; never add a second one to apply a fix.

To review, walk every state of every element (rest, hover, pressed, focus, disabled, loading, empty, error, very long content) and replay motion slowly. Narrow the window to find layout breaks ([Linear, Quality Wednesdays](https://linear.app/now/quality-wednesdays)).

## 1. Typography

- **`tabular-nums`** on every number that changes or lines up: timers, counters, durations, prices, table columns. Proportional digits make changing values jitter ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric)). It only works if the font has the `tnum` feature, so check.
- **`text-wrap: balance`** on headings, captions and short blocks (Chromium balances up to 6 lines, Firefox 10). **`text-wrap: pretty`** on paragraphs to avoid orphans. Both fall back safely ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/text-wrap)).
- Real characters: curly quotes, `…`, `×`, en dash for ranges. A non-breaking space between a number and its unit (`4 min`) and inside shortcuts (`⌘ K`) ([Vercel guidelines](https://vercel.com/design/guidelines)).
- Variable fonts with an `opsz` axis size themselves optically by default; leave `font-optical-sizing: auto` alone ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/font-optical-sizing)).
- `-webkit-font-smoothing: antialiased` only affects macOS and makes text lighter. Use it deliberately (often on light-on-dark text), not as a reflex ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/font-smooth)).
- Inputs on mobile at 16 px or more, or iOS zooms the page on focus.

## 2. Geometry

- **Concentric radii.** A rounded element inside a rounded container: inner radius = outer radius − padding. Equal radii on parent and child look pinched at the corners ([Apple, WWDC25 "Get to know the new design system"](https://developer.apple.com/videos/play/wwdc2025/356/), [Vercel](https://vercel.com/design/guidelines)). Pills use half their height.
- **One radius scale, one spacing scale**, used everywhere. Two buttons side by side are the same height.
- **Optical over geometric alignment.** Play triangles, arrows and icon-plus-label buttons often need a 1 px nudge to look centred. Everything aligns to something: a grid, a baseline, an edge or an optical centre ([Vercel](https://vercel.com/design/guidelines)).
- **Depth from layered shadows.** Vercel uses layered shadows (two or more layers) combined with semi-transparent borders, both tinted toward the background hue rather than pure black ([Vercel](https://vercel.com/design/guidelines)). Keep real borders for dividers, inputs and selected or focused states.
- **Image edges** (practice, unsourced): a 1 px inset outline at low opacity (`outline: 1px solid rgb(0 0 0 / 0.1); outline-offset: -1px`) stops images bleeding into a same-colour background.

## 3. States

- Every interactive element has distinct **rest, hover, pressed, focus-visible and disabled** states, and the interactive ones have more contrast than rest.
- Hover styles go behind `@media (hover: hover)` so touch devices don't get stuck hover states. Nothing is reachable by hover alone.
- **Pressed feedback is immediate** (a colour shift or `scale(0.97)`), so a tap is acknowledged before any work starts.
- **Focus:** use `:focus-visible`, a 2 px outline with an offset, at least 3:1 against what's around it ([WCAG 2.4.7 AA](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html), [2.4.13 AAA](https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance.html)). Sticky headers and bars must not hide the focused element entirely ([2.4.11 AA](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html)); aim for no covering at all (2.4.12 AAA). Use `scroll-padding-top` equal to the header height.
- **Hit areas** at least 24 × 24 px ([WCAG 2.5.8](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html)), 44 px on touch. Enlarge the hit area with padding or a pseudo-element, not the visible control. A checkbox and its label share one target. No dead gaps between items in a list ([Vercel](https://vercel.com/design/guidelines)).
- **Loading buttons keep their label** and add an indicator ([Vercel](https://vercel.com/design/guidelines)). Keeping the width fixed too avoids a layout jump.
- Don't disable a submit button in advance. Validate on submit, then disable it while sending.

## 4. Icons

The first rule is sourced ([Raycast](https://www.raycast.com/blog/a-fresh-look-and-feel)); the rest is common practice.

- One icon set, one stroke width and corner style, sized to the text beside it. A hairline icon next to bold text looks wrong; match stroke to text weight.
- One SVG using `currentColor`, recoloured per state in CSS, rather than a separate file per state.
- Outline icons by default; a filled variant marks the active or selected state (a tab bar's current tab).
- Mirror directional icons (back, forward, reply) in right-to-left layouts; don't mirror icons of real objects.

## 5. Empty states

- Never blank, and never "No items" while the data is still loading: people can't tell empty from broken from slow ([NN/g](https://www.nngroup.com/articles/empty-state-interface-design/)).
- Say what will appear here and offer the action that fills it ("Add your first project"). Where it helps, offer a sample or demo to explore.
- Keep anything people must know out of the empty state; it disappears once there's content ([Apple, Writing](https://developer.apple.com/design/human-interface-guidelines/writing)).
- Design the sparse, normal, dense and very-long-content versions too.

## 6. Errors

- Prevent first: constrain inputs, use the right `type`, `inputmode` and `autocomplete`, and warn about the likely mistake before it happens ([NN/g](https://www.nngroup.com/articles/error-message-guidelines/)).
- Put the message **next to the cause**, in plain words, saying **what to do** ("Use at least 8 characters", not "Invalid password").
- **Keep what the person typed.** Never clear a form on error or on re-render. Move focus to the first error on submit.
- No blame ("invalid", "illegal"), no "Oops", no jokes; humour wears thin on the tenth failure.
- Don't put errors in toasts that vanish. Match the weight to the severity: inline for a field, a banner for a page, a dialog only for something serious.
- If many people hit the same error, change the design, not the message ([Apple](https://developer.apple.com/design/human-interface-guidelines/writing)).

## 7. Undo over confirm

- **Routine destructive actions happen at once and offer Undo** (delete a row, remove an item, archive). Apple: don't alert on common, undoable actions, even destructive ones ([Apple, Alerts](https://developer.apple.com/design/human-interface-guidelines/alerts)).
- Confirmation dialogs are for **rare, irreversible, unexpected** loss. Routine confirmations train people to click through, which makes the dangerous ones useless ([NN/g](https://www.nngroup.com/articles/confirmation-dialog/)).
- When you do confirm: name the item and the consequence, use verb buttons ("Delete project", never Yes/No), and never make the destructive button the default.
- After an undo, show what came back; scroll to it and highlight it if it was off screen ([Apple, Undo](https://developer.apple.com/design/human-interface-guidelines/undo-and-redo)).
- Warn before leaving a page with unsaved work.

## 8. Remember where people were

- State that people would share or come back to lives in the URL: tabs, filters, sort, the open item, pagination ([Vercel](https://vercel.com/design/guidelines)).
- Restore scroll position on Back and Forward.
- Reopen where they left off, down to the detail ([Apple, Launching](https://developer.apple.com/design/human-interface-guidelines/launching)). The launch screen looks like the first real screen, not a splash.
- Use real links for navigation so modifier-click, middle-click and "open in new tab" work.
- Pick good defaults so most people never touch the setting.

## 9. Keyboard and command palette

- One global shortcut (usually ⌘K / Ctrl+K) opens the palette everywhere; the same shortcut closes it, and closing returns focus to where it was ([Superhuman](https://blog.superhuman.com/how-to-build-a-remarkable-command-palette/)).
- Every action is a command. Search is case-insensitive and fuzzy, knows synonyms, and ranks by the current screen and recent use. Hide only commands that can't apply here.
- Show the shortcut next to each action in menus and the palette so people learn it ([Raycast](https://www.raycast.com/blog/a-fresh-look-and-feel)).
- Never repurpose a standard shortcut (copy, undo, find) ([Apple, Keyboards](https://developer.apple.com/design/human-interface-guidelines/keyboards)).
- Enter submits a single-field form; ⌘/Ctrl+Enter submits a textarea.
- No animation on keyboard-driven actions (see the `ui-motion` skill).

## 10. Microcopy

- Labels are specific verbs: "Save draft", not "Continue" or "OK". Clear, not clever ([Apple, Writing](https://developer.apple.com/design/human-interface-guidelines/writing)).
- Keep a term list and use one noun per concept everywhere.
- One capitalisation style per kind of element.
- Tone follows the stakes: direct when something went wrong, warmer when something went well. Tone measurably shifts how friendly a product feels, but only modestly ([NN/g, tone of voice](https://www.nngroup.com/articles/tone-of-voice-dimensions/)).
- Numerals for counts ("3 files"), and locale-aware dates and numbers.
- Never colour alone: pair it with text or an icon.

## 11. Make craft a habit

Polish holds up only when it's routine. Linear's practices: every bug is fixed or explicitly closed, and each engineer fixes one small imperfection a week (most under 30 minutes) and shows it to the team. Over two years that came to more than 1,000 fixes ([Linear](https://linear.app/now/quality-wednesdays), [zero-bugs policy](https://linear.app/now/zero-bugs-policy)).

## Why it matters, and the caveat

The evidence is mixed. People rate attractive interfaces as easier to use, and in some studies the link survives actual use ([Kurosu & Kashimura 1995](https://chi1995.chistatic.hosting.acm.org/proceedings/shortppr/mk_bdy.htm), [Tractinsky et al. 2000](https://academic.oup.com/iwc/article/13/2/127/898608)). The caveat cuts the other way in testing: polish can hide real problems, because people praise the looks after struggling with the task. So watch what people do, not just what they say ([NN/g](https://www.nngroup.com/articles/aesthetic-usability-effect/)). Later work also found the reverse effect: poor usability makes a design look worse ([Tuch et al. 2012](https://doi.org/10.1016/j.chb.2012.03.024)).

## Review checklist

- [ ] Changing and aligned numbers use `tabular-nums`; headings balance.
- [ ] Nested radii are concentric; one radius and spacing scale.
- [ ] Every control has rest, hover (hover devices only), pressed, focus-visible and disabled states.
- [ ] Focus ring 2 px at 3:1, never hidden by sticky UI.
- [ ] Hit areas ≥24 px, ≥44 px on touch, no dead zones.
- [ ] Empty states explain and offer the action; never "none" while loading.
- [ ] Errors sit beside the cause, say what to do, and keep the input.
- [ ] Routine deletes undo; only rare, irreversible loss confirms, with verb buttons.
- [ ] Tabs, filters and the open item are in the URL; scroll restores on Back.
- [ ] Labels are specific verbs; one noun per concept.

Full notes and every source: [references/sources.md](references/sources.md).
