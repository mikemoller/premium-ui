# UI/UX skills

Agent skills for making web and mobile apps feel premium and well thought-out. Every rule links to the primary source it comes from (Apple's Human Interface Guidelines, Material 3, Android Developers, NN/g, WCAG, MDN, web.dev, the original research papers), and each skill says where the evidence is weak or the advice is just convention.

## Skills

| Skill | Use it when |
| --- | --- |
| [`premium-feel`](skills/premium-feel/SKILL.md) | You want an app to feel more premium and don't know where to start. Runs a prioritised review and routes to the others. |
| [`aha-moment`](skills/aha-moment/SKILL.md) | You need to find your product's aha moment and the peaks and ends worth designing for, with or without analytics. |
| [`ux-psychology`](skills/ux-psychology/SKILL.md) | A decision leans on a "law of UX": peak–end, response times, goal-gradient, aesthetic–usability, Zeigarnik. Graded by evidence. |
| [`perceived-performance`](skills/perceived-performance/SKILL.md) | The app feels slow, or you're adding loading states, mutations, caching or navigation. |
| [`ui-motion`](skills/ui-motion/SKILL.md) | You're adding or tuning animations, transitions, popovers, sheets or View Transitions. |
| [`mobile-native-feel`](skills/mobile-native-feel/SKILL.md) | A mobile web app, PWA or WebView shell (Tauri, Capacitor) feels like a website in a wrapper. |
| [`interface-craft`](skills/interface-craft/SKILL.md) | Building or reviewing components: typography, radii, states, focus, empty and error states, undo, copy. |

Each skill is a short `SKILL.md` of rules and a checklist, plus `references/sources.md` with the full research notes and every citation.

## Install

With the [skills CLI](https://github.com/vercel-labs/skills), for Claude Code, Cursor, Codex and other agents:

```bash
npx skills add mikemoller/ui-ux-skills
```

One skill only:

```bash
npx skills add mikemoller/ui-ux-skills --skill ui-motion
```

As a Claude Code plugin:

```bash
/plugin marketplace add mikemoller/ui-ux-skills
/plugin install ui-ux-skills@ui-ux-skills
```

## Credits

Some ideas in `interface-craft` and `ui-motion` (concentric radii, optical alignment, shadows for depth, icon stroke matching, scale on press) were inspired by Jakub Krehel's [make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better). The text here is written from scratch against the primary sources.

## License

[MIT](LICENSE)
