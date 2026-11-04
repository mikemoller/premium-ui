# Design system and tokens: primary-source notes

Compiled 2026-11-04. Each item is a claim, its URL and its strength. **Spec** is a standard or a Community Group report. **System** is a design system's own documentation or token source. **Convention** is common practice with no primary source behind it.

The m3.material.io pages render only with JavaScript, so the Material values below come from Google's own token source files (material-web Sass tokens, MDC-Android docs). Readers may prefer the m3.material.io pages; the token files are the evidence.

---

## 1. W3C Design Tokens Community Group format

- **Stable release 2025.10**, "Final Community Group Report", 28 Oct 2025. It describes itself as stable, but it is not a W3C Standard and not on the Standards Track. https://www.designtokens.org/tr/2025.10/format/ (spec)
- An editor's draft dated 8 Sep 2026 says "Do not attempt to implement this version". Pin to 2025.10. https://www.designtokens.org/tr/drafts/format/
- **Files**: the `.tokens` extension is preferred; `.tokens.json` is also recommended. Media type `application/design-tokens+json`.
- **Token properties**:
  - A token is an object with `$value`.
  - `$type` is optional and is inherited from the parent group or from the referenced token. Tools must not infer the type from the value.
  - Also `$description`, `$extensions` (reverse-domain keys; tools must keep data they don't understand) and `$deprecated`.
- **Groups**: a group is an object without `$value`. `$root` names a group's base token; `$extends` inherits from another group.
- **Aliases**: `"{group.token}"` points at a whole value. A JSON Pointer `$ref` points at part of a value, and tools must support it.
- **Types**:
  - Primitive: color, dimension, fontFamily, fontWeight, duration, cubicBezier, number.
  - Composite: strokeStyle, border, transition, shadow, gradient, typography.
  - The gradient and typography sections were read only in outline.
- **Style Dictionary** lives at github.com/style-dictionary/style-dictionary and has had first-class DTCG support since v4. Full 2025.10 support is "a work in progress in v5". https://styledictionary.com/info/dtcg/ (tool docs)

## 2. Tiers

- **Material 3** token files go from ref to sys to comp:
  - `_md-ref-palette.scss`
  - `_md-sys-color.scss`, `_md-sys-typescale.scss`, `_md-sys-shape.scss`, `_md-sys-elevation.scss`, `_md-sys-motion.scss`
  - `_md-comp-*.scss`
  - Source: https://github.com/material-components/material-web/tree/main/tokens/versions/latest/sass. Overview page (not rendered): https://m3.material.io/foundations/design-tokens/overview. (system)
- **Adobe Spectrum**, https://spectrum.adobe.com/page/design-tokens/ (system):
  - Primitive tokens define the available values (`blue-800`, `corner-radius-75`).
  - Alias tokens reference other tokens (`accent-content-color-default` → `accent-color-900` → `blue-900`).
  - Component tokens such as `tooltip-maximum-width` are kept to a minimum.
- **GitHub Primer**, https://primer.style/product/primitives/token-names/ (system):
  - Base tokens map to raw values (`base-size-4`, `base-color-green-5`).
  - Functional tokens cover global patterns (`bgColor-inset`, `fgColor-default`).
  - Component tokens are only for component CSS (`button-primary-bgColor-hover`).
  - Names have up to six blocks, prefix-namespace-pattern-variant-property-scale; only the property block is required.
- **Origin of "design tokens"**: Salesforce UX around 2014, with Theo (https://github.com/salesforce-ux/theo) and the Lightning Design System. Jina Anne is widely credited. No primary Salesforce page confirming this was found, and the Medium post returned 403, so this history is **unverified**.

## 3. Colour

- **Material 3 roles**, https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Color.md (system):
  - Primary, Secondary and Tertiary, each with on-X, X-container and on-X-container, plus fixed variants.
  - Error with its container and on-roles.
  - Surface and on-surface; surface-variant; surface dim and bright; surface containers from lowest to highest.
  - Outline and outline-variant; inverse roles.
  - Each X pairs with on-X. When customising, keep the luminance level so contrast holds.
  - Tonal palettes:
    - accents use tones 10/20/30/40/80/90;
    - neutrals use finer tones for the surface roles.
  - Dynamic colour comes from the wallpaper (Android 12+) or from one source colour in an image.
- **HCT** = hue, chroma, tone, based on CAM16 and L*. A tonal palette varies only in tone. https://github.com/material-foundation/material-color-utilities (system code)
- **Apple HIG Color**, https://developer.apple.com/design/human-interface-guidelines/color (guideline):
  - Dynamic system colours are defined by purpose, not appearance.
  - Labels: label, secondaryLabel, tertiaryLabel, quaternaryLabel.
  - Backgrounds: system and grouped sets, each primary/secondary/tertiary.
  - Don't redefine semantic meanings.
- **Apple HIG Dark Mode**, https://developer.apple.com/design/human-interface-guidelines/dark-mode (guideline):
  - Dark colours aren't inversions of the light ones.
  - Base and elevated background sets; elevated is used automatically for popovers and sheets.
  - Contrast at least 4.5:1, and aim for 7:1 for small custom-coloured text.
  - Test with Increase Contrast and Reduce Transparency.
- **CSS**:
  - `oklch()` is in CSS Color 4, Candidate Recommendation Draft, §9.2. https://www.w3.org/TR/css-color-4/
  - `color-mix()`, relative colour syntax, `light-dark()` and `contrast-color()` are in CSS Color 5, a Working Draft dated 29 Oct 2026. https://www.w3.org/TR/css-color-5/
  - MDN reference pages for `oklch`, `color-mix` and `light-dark`. Check Baseline status before relying on them. (spec)
- **Radix 12-step scale**, https://www.radix-ui.com/colors/docs/palette-composition/understanding-the-scale (convention):
  1. app background
  2. subtle background
  3. component background
  4. hover
  5. pressed or selected
  6. non-interactive border
  7. interactive border
  8. strong border and focus ring
  9. solid
  10. solid hover
  11. low-contrast text
  12. high-contrast text
  - Steps 11 and 12 are tuned to APCA Lc 60 and Lc 90 on step 2. APCA is not WCAG 2.x, so verify with WCAG ratios.
- **Stripe**, 2019, https://stripe.com/blog/accessible-color-systems (primary for Stripe's own practice):
  - Moved from HSL to CIELAB so every hue shares one lightness curve.
  - Colours 5 levels apart pass 4.5:1; 4 levels apart pass 3:1.

## 4. Type

- **Material 3 type scale**. Values are rem in px at 16, as size/line-height/tracking. Source: `_md-sys-typescale.scss` and https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Typography.md (system).
  - Display: 57/64/−0.25, 45/52/0, 36/44/0
  - Headline: 32/40, 28/36, 24/32
  - Title: 22/28/0, 16/24/0.15, 14/20/0.1
  - Body: 16/24/0.5, 14/20/0.25, 12/16/0.4
  - Label: 14/20/0.1, 12/16/0.5, 11/16/0.5
  - Title M/S and all Labels are Medium weight; the rest are Regular. There is also an "emphasized" scale.
- **Apple iOS text styles** at the default Large Dynamic Type size, in pt (size/leading). Source: https://developer.apple.com/design/human-interface-guidelines/typography (guideline).
  - Large Title 34/41
  - Title 1 28/34, Title 2 22/28, Title 3 20/25
  - Headline 17/22 semibold
  - Body 17/22
  - Callout 16/21, Subhead 15/20, Footnote 13/18
  - Caption 1 12/16, Caption 2 11/13
  - Default size 17 pt, minimum 11 pt. Sizes scale with Dynamic Type, xSmall to AX5.
- **Modular scale**: Tim Brown, *A List Apart*, 3 May 2011. https://alistapart.com/article/more-meaningful-typography/ It is presented as a tool, and no ratio is shown to be better (convention).
- `clamp()`: https://developer.mozilla.org/en-US/docs/Web/CSS/clamp. A vw-only clamp can defeat zoom, which fails WCAG 1.4.4 (https://www.w3.org/WAI/WCAG22/Understanding/resize-text.html).

## 5. Spacing

- Material 3: "measured on an 8dp scale, where space100 = 8dp". https://m3.material.io/styles/spacing/overview. The 4dp grid for icons and type inside components is from Material 2 and **unverified** for M3. The material-web token files contain no spacing scale.
- Apple HIG has no spacing scale, only safe areas, layout margins and guides. https://developer.apple.com/design/human-interface-guidelines/layout
- The 8-point grid is a **convention**. The usual citation is Bryn Jackson's "The 8-Point Grid" (spec.fm, about 2016), which was not fetched.
- Tailwind v4 uses one base, `--spacing: 0.25rem`, with every spacing utility a multiple of it. https://tailwindcss.com/docs/theme (vendor convention)
- CSS logical properties: https://www.w3.org/TR/css-logical-1/ (Working Draft)

## 6. Shape

- **Material 3 corner scale**:
  - none 0, extra-small 4, small 8, medium 12, large 16
  - large-increased 20, extra-large 28, extra-large-increased 32, extra-extra-large 48
  - full: 50% on Android, 9999px on the web
  - The values from large-increased onwards come from M3 Expressive.
  - Sources: `_md-sys-shape.scss` and https://raw.githubusercontent.com/material-components/material-components-android/master/docs/theming/Shape.md (system)
- **Concentric radii** (inner = outer − padding): Apple, WWDC25 "Get to know the new design system", https://developer.apple.com/videos/play/wwdc2025/356/. Already in `interface-craft`.

## 7. Elevation

- **Material 3** levels 0–5 = 0, 1, 3, 6, 8, 12 dp (`_md-sys-elevation.scss`, system). M3 separates surfaces mainly through surface-container tone. The exact wording on https://m3.material.io/styles/elevation/applying-elevation is **unverified**, because the page didn't render.
- **Apple Materials**, https://developer.apple.com/design/human-interface-guidelines/materials (guideline):
  - Liquid Glass is for the controls and navigation layer, not for content.
  - Standard materials (blur and vibrancy) are for the content layer.
  - Choose a material by meaning.

## 8. Motion tokens

- **Material 3** (`_md-sys-motion.scss`):
  - Durations: short 50/100/150/200, medium 250–400, long 450–600, extra-long 700–1000 ms.
  - Easings: standard `(0.2,0,0,1)`, standard-decelerate `(0,0,0,1)`, standard-accelerate `(0.3,0,1,1)`, emphasized-decelerate `(0.05,0.7,0.1,1)`, emphasized-accelerate `(0.3,0,0.8,0.15)`.
  - Springs, as damping/stiffness:
    - Spatial: fast 0.9/1400, default 0.9/700, slow 0.9/300.
    - Effects: 1/3800, 1/1600, 1/800.
    - Which Compose `MotionScheme` (standard or expressive) these values belong to is **unverified**.
- Full motion notes: `ui-motion/references/sources.md`.

## 9. Theming mechanics

- Custom properties: https://www.w3.org/TR/css-variables-1/ (Candidate Recommendation)
- `@property`: https://www.w3.org/TR/css-properties-values-api-1/
- `prefers-color-scheme`: Media Queries 5.
- `color-scheme`: https://www.w3.org/TR/css-color-adjust-1/. It must be set for `light-dark()` to resolve.
- **Tailwind v4 `@theme`** (vendor convention), https://tailwindcss.com/docs/theme:
  - Theme variables decide which utilities exist.
  - Namespaces: `--color-*`, `--font-*`, `--text-*`, `--spacing-*`, `--radius-*`, `--shadow-*`, `--ease-*`, `--animate-*`, `--breakpoint-*`.
  - Use `:root` for non-utility variables.
  - `@theme inline` for values that reference other variables.
  - `@theme` must be top-level.

## 10. Extracting from an existing product

- **Interface inventory**, from Brad Frost's *Atomic Design*, ch. 4. https://atomicdesign.bradfrost.com/chapter-4/ (practitioner)
  1. Gather people from every discipline.
  2. Screenshot each unique pattern, in a 30–90 minute timebox.
  3. Sort by category.
  4. Present each category.
  5. Decide what to keep, merge or drop, and name it.
- Inventorying distinct CSS values before naming tokens is the same idea applied to code. It is a **convention** with no primary source.
- `DESIGN.md` as a shared file that agents read is modelled on Impeccable's DESIGN.md (YAML tokens plus prose sections) and Taste Skill's locks. Both are practitioner conventions; see `taste/references/sources.md`.

## Gaps

Unverified: the M3 4dp spacing rule, the wording of M3 tonal elevation, which motion scheme the spring values belong to, and the Salesforce origin date. Not fetched: MDN pages.
