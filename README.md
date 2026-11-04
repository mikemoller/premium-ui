<h1 align="center">premium-ui</h1>

<p align="center">
  <strong>Agent skills that make web and mobile apps feel premium, with taste and without the AI slop.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img alt="MIT licence" src="https://img.shields.io/badge/licence-MIT-black"></a>
  <img alt="10 skills" src="https://img.shields.io/badge/skills-10-black">
  <img alt="Works with Claude Code, Cursor, Codex" src="https://img.shields.io/badge/works%20with-Claude%20Code%20·%20Cursor%20·%20Codex-black">
</p>

---

Coding agents can build a working UI in minutes. It usually looks like every other generated UI: a violet gradient, a centred headline with a tiny uppercase label above it, and three equal icon cards. It also tends to feel slow, flat, and slightly off on a phone.

**premium-ui** is a suite of ten skills that fixes both problems. The agent learns what makes software feel fast, native, accessible and carefully made, and every piece of UI work then goes through a **taste gate**. That gate looks at the rendered screen, names the generic tells, checks hierarchy, type, colour and space against primary sources, and fixes them in your own design tokens.

## What's different

- **Sourced.** Every rule links to the source that owns it: WCAG, Apple's Human Interface Guidelines, Material 3, Android Developers, MDN, NN/g, the W3C token spec, or the original research paper. Blog posts that only summarise those sources aren't used.
- **Honest about evidence.** Each skill marks where the evidence is strong, where it's only convention, and where a number that gets repeated everywhere couldn't be traced. The anti-slop list is labelled as opinion, because it is.
- **Taste on by default.** You don't have to ask for a taste check. Every skill in the suite ends with the `taste` pass.
- **Generic and portable.** The skills contain only Markdown, with no scripts and no product-specific examples. They work with any stack and any agent that reads `SKILL.md`.

## The skills

| Skill | Use it when |
| --- | --- |
| [`premium-feel`](skills/premium-feel/SKILL.md) | **Start here.** You want an app to feel more premium and don't know where to begin. Runs a prioritised review and routes to the others. |
| [`taste`](skills/taste/SKILL.md) | **The gate.** Something looks generic or "AI-made", or you want a design critiqued, given a direction, or made bolder, quieter or simpler. Runs after every other skill. |
| [`design-system`](skills/design-system/SKILL.md) | You need tokens: colour roles, dark mode, type and spacing scales, radii, elevation, and a `DESIGN.md` that every skill reads. |
| [`accessibility`](skills/accessibility/SKILL.md) | You want a WCAG 2.2 AA audit and fixes: contrast, names, keyboard, dialogs, forms, zoom, and screen readers on web, iOS and Android. |
| [`perceived-performance`](skills/perceived-performance/SKILL.md) | The app feels slow, or you're adding loading states, mutations, caching or navigation. |
| [`ui-motion`](skills/ui-motion/SKILL.md) | You're adding or tuning animations, transitions, popovers, sheets or View Transitions. |
| [`mobile-native-feel`](skills/mobile-native-feel/SKILL.md) | A mobile web app, PWA or WebView shell (Tauri, Capacitor) feels like a website in a wrapper. |
| [`interface-craft`](skills/interface-craft/SKILL.md) | You're building or reviewing components: typography, radii, states, focus, empty and error states, undo, copy. |
| [`ux-psychology`](skills/ux-psychology/SKILL.md) | A decision leans on a "law of UX" such as peak–end, response times, goal-gradient or aesthetic–usability. Each one is graded by its evidence. |
| [`aha-moment`](skills/aha-moment/SKILL.md) | You need to find your product's aha moment and the peaks and ends worth designing, with or without analytics. |

Each skill is a short `SKILL.md` of rules and a checklist that an agent can load in full. Next to it, `references/` holds the full research notes and every citation.

## How it fits together

```
            premium-feel  (routes and prioritises)
                 │
  ┌──────────┬───┴────────┬──────────────┬───────────────┐
  speed     motion    mobile feel   accessibility    craft · psychology · aha
  └──────────┴───┬────────┴──────────────┴───────────────┘
                 │      all of them read DESIGN.md  ◄── design-system
                 ▼
               taste   (render → name the tells → check fundamentals → fix)
```

## What a taste pass looks like

> **Design read:** a billing dashboard for finance teams; it should feel calm, dense and trustworthy. Register: app. Verdict from rendered screenshots at 1440, 768 and 390 px.
>
> **Works:** the table density and tabular numbers.
>
> | Priority | Where | Tell or problem | Fix | Basis |
> | --- | --- | --- | --- | --- |
> | P1 | Header | Violet-to-blue gradient behind the page title | Solid surface token; the title carries the hierarchy | Opinion (slop) |
> | P1 | KPI row | Hero-metric block: four equal stat cards | Lead with the one number that matters; the rest become a compact row | Opinion (slop) |
> | P1 | Filters | Secondary text #9CA3AF on white, 2.5:1 | `text-secondary` token at 4.6:1 | WCAG 1.4.3 |
> | P2 | Cards | Hairline border *and* a wide soft shadow on every card | Keep the border; separate surfaces by tone | Opinion (slop) |

You can ask for one mode by name: `review`, `direction`, `bolder`, `quieter`, `distill`, `typeset`, `colour`, `layout`, `polish`.

## Install

With the [skills CLI](https://github.com/vercel-labs/skills), for Claude Code, Cursor, Codex and other agents:

```bash
npx skills add mikemoller/premium-ui
```

One skill only:

```bash
npx skills add mikemoller/premium-ui --skill taste
```

As a Claude Code plugin:

```bash
/plugin marketplace add mikemoller/premium-ui
/plugin install premium-ui@premium-ui
```

Then ask your agent something like "make this screen feel premium", "this looks AI-generated, fix it", or "audit this form for accessibility".

## Credits

- The `taste` skill draws its anti-slop catalogue and its modes from three projects: [Impeccable](https://impeccable.style/) by Paul Bakaus (Apache-2.0), [Taste Skill](https://github.com/Leonxlnx/taste-skill) by Leon Lin (MIT), and Anthropic's [frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) skill (Apache-2.0). Every rule is credited where it's used and rewritten in our own words.
- Some ideas in `interface-craft` and `ui-motion` (concentric radii, optical alignment, shadows for depth, icon stroke matching, scale on press) were inspired by Jakub Krehel's [make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better).

All the text here was written from scratch against the primary sources.

## License

[MIT](LICENSE)
