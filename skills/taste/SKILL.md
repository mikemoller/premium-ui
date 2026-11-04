---
name: taste
description: The taste and anti-slop gate for UI work. Looks at the rendered design, names the generic "AI-made" tells (purple gradients, three equal icon cards, eyebrow labels, gradient text, nested cards, default fonts, buzzword copy), checks hierarchy, type, colour and space against primary sources, then fixes it in the project's own tokens. Modes for review, direction, bolder, quieter, distill, typeset, colour, layout and polish. Use whenever building, generating, redesigning or reviewing any page, screen or component — including as the final pass after any other premium-ui skill — and whenever asked to make something less generic, less "AI-looking", more distinctive, bolder, calmer, or to critique a design.
---

# Taste

Generated UI drifts to the middle. A model samples the most likely design, so without direction it converges on the same fonts, gradients and card grids as everyone else; Anthropic calls this *distributional convergence* ([Anthropic, 2025](https://claude.com/blog/improving-frontend-design-through-skills)). Taste is the opposite move: specific choices that come from *this* product, made once and held everywhere.

Run this pass on every piece of UI work before calling it done, unless the user opts out. Express every fix in the project's existing tokens and components; if there's a `DESIGN.md`, it wins over this skill's defaults.

## How to look

1. **Render it.** Screenshot at about 1440, 768 and 390 px wide (or the simulator), in light and dark if both exist. Judge from pixels; spacing, rhythm and colour interplay barely show in code. If nothing can be rendered, review the code and **say the verdict is lower-confidence**.
2. **Squint, then read.** Blur your eyes at the screenshot: what is seen first, second, third? Then read every word as the user would.
3. **Read context.** `DESIGN.md`, `PRODUCT.md`, tokens, brand assets. Never invent brand facts, logos, testimonials or metrics.

## The pass

1. **Design read.** One sentence: *what* this is, *for whom*, and the *feel* it should have ("a billing dashboard for finance teams; calm, dense, trustworthy"). Pick the register: **marketing** pages persuade and may be distinctive; **app** screens help people operate and should feel familiar. People rate typical-looking, low-complexity sites as more appealing at first sight ([Tuch et al. 2012](https://doi.org/10.1016/j.ijhcs.2012.06.003)), so "distinctive" belongs in the brand moments, not in the controls.
2. **Specificity test.** Could a competitor ship this screen unchanged with their logo on it? If yes, it has no point of view. Find one thing only this product has (its data, its subject, its voice) and let it shape the hero or the key screen.
3. **Slop scan.** Check the tells below, then the full list in [references/slop.md](references/slop.md).
4. **Fundamentals.** Hierarchy, type, colour, space and contrast (below). These are grounded; the tells are opinion.
5. **Verdict and fixes.** Two or three things that work, then at most five priority fixes. Apply them, re-render, and compare before and after side by side.

**Removal isn't enough.** Stripping every ornament leaves a bare page that also reads as generated. Replace a tell with something specific (real content, real images, a deliberate scale contrast), not with nothing, and not with the *next* cliché: the anti-slop look itself (cream background, serif display, terracotta accent) is now a tell ([anthropics/skills frontend-design, Sep 2026](https://github.com/anthropics/skills/tree/main/skills/frontend-design)).

## Modes

Default is **review**. The user can ask for a mode by name; scope it to the part they point at.

| Mode | Does |
| --- | --- |
| `review` | The pass above, ending in a verdict table. |
| `direction` | Before building: design read, one point of view, the locks below, then type, colour and spacing picks. Writes `DESIGN.md` only if asked or if generating something new (see `design-system`). |
| `bolder` | Raise one part to the conviction the rest implies: bigger scale contrast, a committed accent, a real image. Leave the rest alone. |
| `quieter` | Fewer colours, weights, borders and shadows; more space; less motion. Keep the point of view. |
| `distill` | Remove whatever doesn't earn its place: duplicate copy, decorative containers, extra CTAs, sections without a claim. |
| `typeset` | Fix families, the scale, weights, measure, leading and wrapping. |
| `colour` | Give colour a job: one accent on the action, neutrals tinted toward the brand hue, semantic colours for status. |
| `layout` | Fix grouping, rhythm, alignment and hierarchy; vary section structure. |
| `polish` | Final pass inside the current direction. If the concept itself is wrong, say so rather than quietly redesigning. Hand detail work to `interface-craft`. |

## Locks (opinion, from Taste Skill)

Decide once per product and hold them everywhere ([taste-skill v2](https://github.com/Leonxlnx/taste-skill/blob/51c272e2/skills/taste-skill-v2-experimental/SKILL.md)):

- **One accent colour**, used on the primary action and at most one other element.
- **One shape language**: sharp, soft (one radius scale) or pill, not a mix without a rule.
- **One theme** per page: light or dark, no accidental mid-page inversion.
- **One or two typefaces.** Vary size, weight and spacing before adding a family.

## The tells to check first

Opinion, not research: these are patterns that recur in generated UI, compiled from [Impeccable](https://impeccable.style/slop), [Taste Skill](https://github.com/Leonxlnx/taste-skill) and Anthropic's frontend-design skill. Each has a reason and a fix in [references/slop.md](references/slop.md). A tell is fine when it's a deliberate, stated choice.

- **Colour**: purple-to-blue or violet gradients; gradient text; coloured glows and radial halos on dark; grey text on coloured backgrounds; the reflexive cream-and-terracotta "tasteful" palette.
- **Layout**: a row of three equal icon-topped cards; every section the same card grid; cards inside cards; centred everything; the hero-metric block (huge number, small label, three stats); one spacing value everywhere.
- **Type**: Inter, Roboto, Geist or Space Grotesk chosen by default rather than for a reason; a tiny tracked uppercase eyebrow or pill above every heading; an accent word in italic serif or colour inside the headline; headings wrapping to three lines or more; weak contrast between heading and body sizes.
- **Decoration**: a thick coloured border down one side of a card; hairline border *and* wide soft shadow on every card; glassmorphism as decoration; grid-paper or stripe backgrounds; illustrations built from primitive shapes; emoji as icons; 01 / 02 / 03 section numbering.
- **Motion**: the same fade-up on every block; bounce easing; pulsing "live" dots and fake typing cursors; hover lift on every card.
- **Copy**: buzzwords (seamless, elevate, unlock, supercharge, next-gen); "Not just X, it's Y"; tricolon headlines; "Get started" and "Learn more" as the only labels; invented logos, quotes, star ratings and "10,000+ teams"; Lorem ipsum or "Acme" placeholders; heavy em-dash use.

## Fundamentals (grounded)

- **Hierarchy**: one clear first thing per screen, then a second and third. Show rank with size, weight and colour; order by importance, top and leading side first ([Apple, Layout](https://developer.apple.com/design/human-interface-guidelines/layout), [Typography](https://developer.apple.com/design/human-interface-guidelines/typography)).
- **Grouping**: related things sit closer together than unrelated ones (proximity, [Wertheimer 1923](https://doi.org/10.1007/BF00410640)); a shared background or border groups as well, often more strongly ([Palmer 1992](https://doi.org/10.1016/0010-0285(92)90014-S)). Space usually beats a box, so reach for spacing before adding a card.
- **Type**: use a scale with clear steps, such as [Material 3](https://m3.material.io/styles/typography/type-scale-tokens) or Apple's text styles (iOS body 17 pt), or a modular ratio, which is a convention ([Brown 2011](https://alistapart.com/article/more-meaningful-typography/)). Avoid light weights for body text ([Apple](https://developer.apple.com/design/human-interface-guidelines/typography)). For measure, 45–75 characters is a typographic convention (Bringhurst, *The Elements of Typographic Style*, §2.1.2), and the screen research is mixed ([Dyson & Haselgrove 2001](https://doi.org/10.1006/ijhc.2001.0458)). WCAG's AAA ceiling is 80 ([1.4.8](https://www.w3.org/WAI/WCAG22/Understanding/visual-presentation.html)).
- **Colour**: build palettes in OKLCH, because HSL lightness isn't perceptually even and yellow and blue at the same "L" look different ([CSS Color 4](https://www.w3.org/TR/css-color-4/)). Colour is relative to what surrounds it (Albers, *Interaction of Color*, 1963). Don't give one colour two meanings, and never rely on colour alone ([Apple, Color](https://developer.apple.com/design/human-interface-guidelines/color), [WCAG 1.4.1](https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html)). Dark mode is a separate palette, not an inversion ([Apple, Dark Mode](https://developer.apple.com/design/human-interface-guidelines/dark-mode)).
- **Contrast**: text needs 4.5:1, and large text and UI parts need 3:1 ([WCAG 1.4.3](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html), [1.4.11](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html)). Taste never overrides this; the full audit is in `accessibility`.
- **Consistency**: similar elements share spacing and size ([Material 3, spacing](https://m3.material.io/foundations/layout/grids-spacing/spacing)), and every value comes from the token scale (`design-system`).

First impressions form in about 50 ms and largely hold ([Lindgaard et al. 2006](https://doi.org/10.1080/01449290500330448)). Visual complexity drives that judgement much more than colourfulness does ([Reinecke et al. 2013](https://doi.org/10.1145/2470654.2481281)), so cut clutter before tuning colour. One caution: none of this measures usability, so watch what people do, not just what they say (`ux-psychology`).

## Review output

Start with the design read, the confidence level (rendered or code-only) and what works. Then:

| Priority | Where | Tell or problem | Why it reads as generic or wrong | Fix | Basis |
| --- | --- | --- | --- | --- | --- |
| P1 | Hero | Violet-to-blue gradient behind a centred headline | The most common generated hero; says nothing about the product | Solid ground in the brand neutral, product screenshot left-aligned beside the headline | Opinion (slop) |
| P1 | Pricing | Body text #9CA3AF on white, 2.5:1 | Fails contrast | Use the secondary text token at 4.5:1 or more | WCAG 1.4.3 |

**P0** blocks the task or fails accessibility badly. **P1** makes the design read as generic or confuses the hierarchy. **P2** is noticeable. **P3** is polish. Label each finding **Grounded** (with its source) or **Opinion**. Also list what you couldn't check.

## Checklist

- [ ] Rendered and screenshotted at three widths, or the verdict flagged as code-only.
- [ ] One-sentence design read; register chosen (marketing or app).
- [ ] Passes the specificity test: something on it only this product could say.
- [ ] Locks held: one accent, one shape language, one theme, one or two families.
- [ ] No unstated tells from the list; none replaced by the next cliché.
- [ ] Clear first, second and third thing; spacing groups before boxes do.
- [ ] Type scale with real steps; measure about 45–75 characters; no light body weights.
- [ ] Contrast at 4.5:1 and 3:1; nothing relies on colour alone.
- [ ] Copy is concrete, with no buzzwords and no invented proof.
- [ ] Before and after compared; at most five priority fixes per pass.

Full tell list with reasons and fixes: [references/slop.md](references/slop.md). Notes and every source: [references/sources.md](references/sources.md).
