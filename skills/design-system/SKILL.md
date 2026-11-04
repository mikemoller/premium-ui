---
name: design-system
description: Build or extract a design system for a web or mobile app — design tokens in tiers (primitive, semantic, component), colour roles and on-colours, light and dark palettes in OKLCH, a type scale, spacing, radii, elevation and motion tokens — and record the decisions in a DESIGN.md that the other premium-ui skills read. Covers auditing an existing codebase for drift (every distinct colour, size, radius and shadow), the W3C Design Tokens format, CSS custom properties, light-dark(), Tailwind v4 @theme, and how Material 3, Apple HIG, Spectrum and Primer structure theirs. Use when starting a new product or redesign, when values are hard-coded or inconsistent, when adding dark mode or theming, when asked to "set up tokens", "create a design system" or "write DESIGN.md", or when the taste skill picks a direction that needs recording.
---

# Design system

A design system turns taste into constraints: a small set of named values that every screen uses, so the hundredth screen matches the first. Prefer what the project already has. Extend an existing token set or component library; never add a second styling system beside it.

## 1. Extract before inventing

On an existing codebase, inventory first, then name:

1. List every distinct value in use: colours, font sizes, weights, line heights, spacing, radii, shadows, durations, easings. Count how often each appears. (Brad Frost's interface inventory does the same with screenshots: gather every unique pattern, sort it by category, then decide what to keep, merge or drop ([Atomic Design, ch. 4](https://atomicdesign.bradfrost.com/chapter-4/)). Doing it in code is the same idea; that step is convention, with no primary source.)
2. Cluster near-duplicates (`#6B7280` and `#6C727F`; 15 px and 16 px) and choose one value per cluster.
3. Name the survivors by **role**, not by appearance. Replace the hard-coded values. Write `DESIGN.md`.

## 2. Tiers

Use three tiers. All four systems below separate raw values from roles:

- **Primitive / reference**: the raw palette and scales (`blue-600`, `space-4`). Never used directly in components.
- **Semantic / system**: roles that point at primitives (`color-text-secondary`, `color-bg-surface`, `color-action-primary`). Components use these. Themes and dark mode swap the values here.
- **Component** (sparingly): `button-primary-bg-hover`. Spectrum keeps these to a minimum ([Spectrum](https://spectrum.adobe.com/page/design-tokens/)).

Examples: Material 3 `md.ref` → `md.sys` → `md.comp` ([token source](https://github.com/material-components/material-web/tree/main/tokens/versions/latest/sass)); Primer base → functional → component, named `namespace-pattern-variant-property-scale` ([Primer](https://primer.style/product/primitives/token-names/)).

## 3. Colour

- **Roles in pairs.** Every background role has an "on" role for the text and icons on it (`primary` / `on-primary`, `surface` / `on-surface`), and each pair meets contrast: 4.5:1 for text, 3:1 for large text and UI parts ([Material 3 roles](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Color.md), [WCAG 1.4.3](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html), [1.4.11](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html)).
- **Minimum role set**: background, surface (plus one or two raised levels), text primary/secondary/disabled, border subtle/strong, accent plus on-accent, focus ring, and status (success, warning, danger, info), each with an on-colour.
- **Define by purpose, not appearance**, like Apple's `label`, `secondaryLabel` and `systemBackground`, and never reuse a role for something else ([Apple, Color](https://developer.apple.com/design/human-interface-guidelines/color)).
- **Build scales in OKLCH**: equal lightness steps look equal across hues, which HSL doesn't manage ([CSS Color 4](https://www.w3.org/TR/css-color-4/)). A 12-step scale with a job per step (backgrounds 1–2, components 3–5, borders 6–8, solid 9–10, text 11–12) is a convenient convention ([Radix](https://www.radix-ui.com/colors/docs/palette-composition/understanding-the-scale)). Stripe's rule of thumb: any two steps 5 apart pass 4.5:1 ([Stripe](https://stripe.com/blog/accessible-color-systems)).
- **Dark mode is its own palette.** It's not an inversion: dim base backgrounds recede, brighter elevated ones come forward for sheets and popovers ([Apple, Dark Mode](https://developer.apple.com/design/human-interface-guidelines/dark-mode)). Re-check every pair in both themes.
- On the web, set `color-scheme: light dark` and use `light-dark()` or a `[data-theme]` / `prefers-color-scheme` swap of the semantic tier ([CSS Color 5](https://www.w3.org/TR/css-color-5/), [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme)). Check browser support on MDN before relying on newer functions.

## 4. Type

- **A named scale with few steps** and clear contrast between levels. Material 3 uses 5 roles × 3 sizes, with body 16/24 and headline 32/40 to 24/32, and says no product uses them all ([M3](https://m3.material.io/styles/typography/type-scale-tokens)). Apple's iOS text styles run from Large Title 34 pt to Caption 2 11 pt, with body at 17 pt ([Apple](https://developer.apple.com/design/human-interface-guidelines/typography)). A modular ratio (1.2, 1.25, 1.333) is a convention, not a finding ([Brown 2011](https://alistapart.com/article/more-meaningful-typography/)).
- Each style is a composite token: family, size, line height, weight, tracking.
- Use `rem` so the user's font size applies, and on iOS support Dynamic Type. Fluid `clamp()` sizes need a `rem` component, or zoom breaks ([WCAG 1.4.4](https://www.w3.org/WAI/WCAG22/Understanding/resize-text.html)).
- One or two families. A family needs a reason (voice, legibility, licence); the `taste` skill checks that it isn't just the default.

## 5. Space, shape, elevation, motion

- **Spacing**: one base unit and a short scale, such as 4, 8, 12, 16, 24, 32, 48, 64. The 4- and 8-point grids are conventions. Material 3 measures spacing on an 8 dp scale, and Apple publishes no grid, only margins and safe areas ([M3](https://m3.material.io/styles/spacing/overview), [Apple](https://developer.apple.com/design/human-interface-guidelines/layout)). Use logical properties (`padding-inline`) so RTL works.
- **Radii**: a small scale. Material 3's runs none 0, 4, 8, 12, 16, 28 and full ([M3 shape](https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Shape.md)). Nested radii are concentric: inner = outer − padding (`interface-craft`).
- **Elevation**: a few levels. Material 3 uses 0, 1, 3, 6, 8 and 12 dp, and shows depth mostly through surface tone rather than shadow ([M3 tokens](https://github.com/material-components/material-web/tree/main/tokens/versions/latest/sass)). In dark mode, lighter surfaces show depth better than shadows do.
- **Motion**: duration and easing tokens. Material 3 has short 50–200 ms, medium 250–400 ms and the standard and emphasized curves. Values and rules are in `ui-motion`.
- **Z-index**: a named stack (base, dropdown, sticky, overlay, modal, toast) instead of loose numbers.

## 6. Where tokens live

- **CSS custom properties** on `:root` (primitives) and on theme selectors (semantic) ([CSS Variables](https://www.w3.org/TR/css-variables-1/)). Use `@property` when a token must animate or needs a type ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)).
- **Tailwind v4**: tokens that should generate utilities go in `@theme` (`--color-*`, `--text-*`, `--spacing`, `--radius-*`, `--shadow-*`, `--ease-*`); use `@theme inline` when a value references another variable ([Tailwind](https://tailwindcss.com/docs/theme)).
- **Several platforms** (web plus iOS and Android): keep a source of truth in the W3C Design Tokens format, version 2025.10 (`$value`, `$type`, `{group.token}` aliases). It's a stable Community Group report, not a W3C standard ([DTCG](https://www.designtokens.org/tr/2025.10/format/)). Generate platform files with [Style Dictionary](https://styledictionary.com/info/dtcg/).
- Native: SwiftUI asset catalogue colours with light and dark variants and text styles; Compose `MaterialTheme` colour, typography and shape schemes.

## 7. DESIGN.md

Write it when starting or redesigning a product, or when asked. Every skill in the suite reads it before making visual decisions. Keep it short and factual:

```markdown
# Design

## Direction
One sentence: what it is, for whom, the feel. The one thing only this product has.
Register: marketing / app. Theme: light, dark or both.

## Locks
Accent: <token> (primary action only). Shape: soft, radius scale 6/10/16. Families: <display>, <text>.

## Tokens
Where they live (file paths), tier names, and the semantic roles with light and dark values.

## Type scale · Spacing · Radii · Elevation · Motion
The scales, by name and value.

## Components
Which library, and the house variants (button, input, card, dialog, toast).

## Do / Don't
Short rules specific to this product (e.g. "Never put the accent on more than one element per screen").
```

## Checklist

- [ ] Existing values inventoried; near-duplicates merged before new tokens are named.
- [ ] Primitive → semantic (→ component) tiers; components use only semantic tokens.
- [ ] Every background role has an on-colour that passes 4.5:1 or 3:1, in light **and** dark.
- [ ] Dark mode is a designed palette, with elevated surfaces lighter.
- [ ] Type scale with few named steps in `rem`, survives 200% zoom; one or two families.
- [ ] One spacing scale, one radius scale, a few elevation levels, motion tokens, named z-index.
- [ ] No hard-coded colours, sizes or shadows left in components.
- [ ] `DESIGN.md` records direction, locks, tokens and do/don'ts.

Full notes and every source: [references/sources.md](references/sources.md).

**Finish with `taste`.** Any screen or component you build or change with this skill gets the `taste` pass before you call it done.
