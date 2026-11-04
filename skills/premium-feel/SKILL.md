---
name: premium-feel
description: Entry point for making a web or mobile app feel premium and well thought-out. Runs a prioritised review (speed, negative peaks, states, mobile feel, motion, accessibility, craft, taste) and routes to the focused skills — perceived-performance, ui-motion, mobile-native-feel, accessibility, interface-craft, design-system, taste, ux-psychology and aha-moment. Use when asked to make an app feel more premium, polished, native, delightful or "less cheap", to audit a screen or flow for quality, or when it's unclear which of the other skills applies.
---

# Premium feel

"Premium" is rarely one thing. It's the sum of an app that answers instantly, never loses your work, behaves like the platform it runs on, moves with purpose, and gets the small details right everywhere. The order matters: speed and reliability have the strongest evidence and the biggest effect; decoration has the weakest.

## Work in this order

1. **Find the moments that matter.** Which flow delivers the product's core value, and where does it end? If nobody knows, run the `aha-moment` skill first. Those moments get the most care.
2. **Remove negative peaks.** Lost input, a crash or wrong state at the critical moment, an error that doesn't say what to do, a spinner on something that should be instant. The worst moment colours the memory of the whole experience (`ux-psychology`).
3. **Make it fast.** Every input acknowledged within 100 ms, INP ≤ 200 ms, no flashing spinners, cached data first, optimistic updates where failure is rare (`perceived-performance`).
4. **Cover every state.** Empty, loading, error, offline, very long content, focus, pressed, disabled (`interface-craft`).
5. **Make it native on phones.** Targets, press states, back behaviour, sheets, safe areas, keyboard, haptics in the shell (`mobile-native-feel`).
6. **Add motion that explains.** Short, eased, interruptible, origin-aware, nothing on keyboard or very frequent actions, reduced motion respected (`ui-motion`).
7. **Make it usable by everyone.** Contrast, names, keyboard, focus, forms, zoom and screen readers to WCAG 2.2 AA (`accessibility`).
8. **Polish the details.** Tabular numbers, concentric radii, optical alignment, consistent icons, specific labels (`interface-craft`).
9. **Design the end.** A calm, clear finish to bounded flows; rare, switchable celebration; no upsell at the last moment (`ux-psychology`).
10. **Taste pass, always last.** Render it, name the generic tells, check hierarchy, type, colour and space, and fix in the project's tokens (`taste`). This runs after every task in the suite, not just full reviews.

## Before changing anything

- Read the project's design system, tokens, component library and `DESIGN.md`, and express every fix in them. If there are no tokens, or values are hard-coded and drifting, run `design-system` first. Never add a second styling system or an animation library for something CSS already does.
- Note what the project already does well and keep it consistent; one inconsistent "improvement" makes the whole feel worse.

## Review output

When reviewing, report findings as a table, most severe first:

| Severity | Where | Problem | Fix | Rule (skill) |
| --- | --- | --- | --- | --- |
| High | `src/Checkout.tsx:41` | Form clears on a failed submit | Keep the values; show the error beside the field | Errors keep input (interface-craft) |

Severity: **High** loses data, blocks a task or breaks the critical moment. **Medium** is noticeably slow, inconsistent or unclear. **Low** is polish. Also list what you checked and found fine, and what you couldn't check (for example, anything that needs a real device).

## The skills

| Skill | Covers |
| --- | --- |
| `aha-moment` | Finding the product's core-value moment and its peaks and ends, with or without data |
| `ux-psychology` | Which UX laws hold up: peak–end, response times, goal-gradient, aesthetic–usability |
| `perceived-performance` | Response limits, INP, loading states, optimistic UI, prefetch, layout shift, undo, offline |
| `ui-motion` | Durations, easing, springs, enter/exit, interruptibility, View Transitions, reduced motion |
| `mobile-native-feel` | Touch, haptics, back, sheets, safe areas, keyboard, scrolling, wake lock |
| `interface-craft` | Typography, geometry, states, focus, icons, empty and error states, undo, copy |
| `accessibility` | WCAG 2.2 AA audit and fixes: contrast, names, keyboard, dialogs, forms, zoom, screen readers |
| `design-system` | Tokens, colour roles, type and spacing scales, dark mode, `DESIGN.md` |
| `taste` | The final gate: anti-slop review, direction, and bolder / quieter / distill / typeset modes |

Every rule in these skills links to the primary source it comes from, and each skill says where the evidence is weak.
