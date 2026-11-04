# The tells: a catalogue of generic generated UI

**This is opinion, not research.** It lists patterns that recur in model-generated interfaces in 2025–26, compiled from three practitioner sources and written here in our own words:

- **[I]** Impeccable by Paul Bakaus: its detector rules and design-review patterns. https://impeccable.style/slop, https://github.com/pbakaus/impeccable (Apache-2.0, tag skill-v4.8.0)
- **[T]** Taste Skill by Leon Lin: its house rules, tells list and ban list. https://github.com/Leonxlnx/taste-skill (MIT; main at 12af471f, v2 at 51c272e2)
- **[A]** Anthropic's frontend-design skill and its blog post. https://github.com/anthropics/skills/tree/main/skills/frontend-design (Apache-2.0; commit 41bbe19d), https://claude.com/blog/improving-frontend-design-through-skills

Two warnings before using it:

1. **Tells date.** What reads as generated changes as models and taste move on, and Taste Skill says this outright. The cream-and-terracotta palette is an example: it started as the cure for purple gradients and is now itself flagged [A]. Treat the list as a snapshot to re-check, not as law.
2. **Context beats the list.** A tell is fine when it is a deliberate, stated choice that fits the brand, such as a hard offset shadow on a real neobrutalist site [I] or an italic serif in an editorial publication. Taste Skill also admits that some of its bans are the author's own taste and appear *more* often on top gallery sites [T].

Numbers here come from those sources and are their conventions, not measurements, unless a primary source is given.

---

## Colour

| Tell | Why it reads as generated | Fix | Src |
| --- | --- | --- | --- |
| Purple/violet/indigo gradients, cyan on dark | The most common default "tech" palette in training data | A colour from the product's meaning or brand; solid grounds | I, T, A |
| Gradient text | Decoration that hurts legibility and contrast checks | Solid ink; carry emphasis with size or weight | I |
| Coloured glow / zero-offset coloured shadow on dark | Glow as a substitute for hierarchy | Remove it, or use a real shadow with offset and blur | I, T |
| Radial halo or spotlight behind the hero | Fills empty dark space with haze | Real content or image; let the dark ground be calm | I |
| Grey text on a coloured background | Washed out, often fails contrast | Tint secondary text from the surface hue, or use the on-colour token | I |
| Reflexive cream/beige ground with terracotta or rust | Now the stock "tasteful" palette | Choose the ground by context of use, not by category | I, T, A |
| Many accents | No single colour means "act here" | One accent, on the primary action (and at most one other element) | T |
| Pure #000 or untinted greys | Flat and harsh next to colour | Neutrals tinted slightly toward the brand hue (chroma about 0.005–0.015 in OKLCH, per [I]) | I, T |

## Layout and space

| Tell | Why | Fix | Src |
| --- | --- | --- | --- |
| Three equal icon-topped feature cards | The default "features" section; gives every point equal weight | Rank the points: one large, the rest smaller; or a list; or show the product | I, T, A |
| Every section the same structure | No rhythm, reads as a template | Vary section families; a page of about 8 sections uses at least 4 layouts [T] | T |
| Cards inside cards | Boxes standing in for grouping | Group with space and type; one container level | I |
| Hairline border *and* a wide soft shadow on every card | Both depth cues at once, on everything | Pick one; separate surfaces by tone steps | I, T |
| Hero-metric block (big number, small label, three stats) | A stock "impressive" layout with no story | Show the one number that matters in context, or drop it | I |
| Everything centred | Lazy symmetry, long ragged lines | Left-align by default; one deliberate centred moment | T |
| One spacing value everywhere | No rhythm, no grouping | A spacing scale; tighter inside groups than between them; more space above a heading than below | I |
| Cramped padding in coloured or bordered boxes | Under about 8 px reads cheap | 12–16 px or more [I] | I |
| Text against the viewport edge on phones | Under about 16 px margin [I] | Consistent side margins from the spacing scale | I |
| Lines longer than about 75–80 characters | Hard to track | Cap the measure (`max-width: 65ch` or similar) | I, T |
| 01 / 02 / 03 section numbering, "Step 2" labels | Scaffolding as decoration | Real headings | I, T, A |
| Split header: big headline left, small paragraph right | A stock pattern | Stack them, or make the paragraph earn the column | T |
| Bento grid with empty or filler cells | Cells invented to fill the grid | As many cells as real items [T] | T |

## Typography

| Tell | Why | Fix | Src |
| --- | --- | --- | --- |
| Inter, Roboto, Arial, system UI, Geist, Space Grotesk, Plus Jakarta Sans or Fraunces picked by default | The highest-probability choices | Choose a family for a stated reason (subject, voice, legibility, licence); system fonts are fine in app UI when that is the reason | I, T, A |
| A tiny tracked uppercase eyebrow or pill above headings | The most frequent generated heading ornament | Drop it; let the heading carry the message | I, T, A |
| An accent word in italic serif or colour inside the headline | Stock emphasis trick | One voice in the headline; emphasis through words | T |
| An oversized italic serif display headline | Current "editorial" default | Only with a real editorial reason | I |
| Headlines wrapping to three lines or more | Too many words at display size | Cut words first, then widen the measure, then reduce size [T] | T |
| Flat hierarchy: heading and body sizes too close | No clear first thing | Steps of at least about 1.25× between levels [I]; stronger contrast for display | I |
| Monospace as a "technical" costume | Signals "developer tool" without reason | Monospace only for code and aligned data | I, T |
| Five or more sizes on one page, or three or more families | Noise | One or two families, a small scale [T] | T |
| Extreme negative tracking on display | Letters collide | Usually −0.02 to −0.03 em; not past −0.04 em [I] | I |
| Wide tracking on body text, all-caps paragraphs | Slows reading | Tracking only on short caps labels | I |
| Body below 14–16 px, line-height below about 1.3 | Strains reading | 16 px body, 1.4–1.6 line height for prose | I, T |
| One-word last lines, lopsided heading wraps | Unbalanced | `text-wrap: balance` on headings, `pretty` on paragraphs | I |

## Decoration and imagery

| Tell | Why | Fix | Src |
| --- | --- | --- | --- |
| Thick coloured border down one side of a card | Called the most recognisable generated tell [I] | Remove; mark state or category with a label or icon | I |
| Thick accent border on a rounded element | The corner and the border fight | Remove, or use a full outline | I |
| Glassmorphism as decoration | Blur standing in for hierarchy; legibility suffers | Solid surfaces; materials only for overlays and controls (see Apple Materials) | I, T |
| Grid-paper or diagonal-stripe backgrounds | Texture with no meaning | Plain ground, or a texture from the subject | I |
| Illustrations assembled from circles and rectangles | Reads as clip art | Real product imagery or photos, or no illustration | I |
| Rough hand-rolled SVG icons, mixed icon sets | Inconsistent stroke and style | One icon family, one stroke weight | I, T |
| Emoji used as icons | Inconsistent across platforms, unserious | Icons from the set | I, T |
| Images buried under a near-opaque wash | The picture was decoration | Show it, or drop it | I |
| Extreme card radius (very large on small cards) | "Soft" taken too far | Card radius about 12–16 px [I]; pills only for small controls | I |
| Fake dashboards or screenshots built from divs | Unconvincing and unreal | Real screenshots of the product | T |

## Motion

| Tell | Why | Fix | Src |
| --- | --- | --- | --- |
| The same fade-up on every block | Motion with no reason | One orchestrated entrance; everything else quiet (see `ui-motion`) | T, A |
| Bounce or elastic easing on UI | Feels toy-like | Ease-out curves, or critically damped springs | I, T |
| Pulsing "live" dot, blinking cursor, typewriter headline | Fake liveness | Real status only when it is real | I, T |
| Auto-scrolling marquee | Can't be read or paused | Static list; at most one marquee, with pause (WCAG 2.2.2) | I, T |
| Hover lift on every card | Everything looks clickable | Hover feedback only on what is interactive | T |
| Reveal animations that can leave blank bands | Content hidden if the script fails | Content visible by default; animate from visible | I, T |
| Decorative scroll cue in the hero | Ornament | Remove | T |

## Copy and content

| Tell | Why | Fix | Src |
| --- | --- | --- | --- |
| Buzzwords: seamless, elevate, unlock, unleash, supercharge, empower, streamline, next-gen, effortless, world-class, cutting-edge | Generic claims anyone could make | Say what it does, concretely, with the product's nouns | I, T |
| "Not just X, it's Y", "X. Not Y." punchlines, tricolon headlines | Stock rhetorical shapes | One plain claim | I, T |
| "Get started" / "Learn more" as the only labels; two CTAs with the same intent | Vague and duplicated | Specific verbs ("Start free trial", "See pricing"); one CTA per intent | T |
| Invented logos, testimonials, star rows, "10,000+ teams", fake-precise numbers | Fabricated proof | Real evidence only; otherwise leave the section out | T |
| "Lorem ipsum", "John Doe", "Acme" | Placeholder never replaced | Real or realistic content from the product | T |
| Heavy em-dash use | A strong tell in generated prose | Full stops and commas; flag at high density (Impeccable warns at about 8 or more per page) | I, T |
| Agency micro-meta: local time, weather, version stamps, middle-dot strings | Decorative filler | Remove | T |
| Hero subtext of more than about 20 words [T] | The point is buried | One sentence | T |

## Design-level patterns (need judgement, not a detector)

- **No point of view**: the screen passes every rule above and is still forgettable. Apply the specificity test in `SKILL.md`.
- **Bare after cleanup**: ornament removed, nothing specific put in its place [T].
- **Identical card grids** wherever there is a list [I].
- **One font for everything**, with no variation in size, weight or spacing [I].
- **Modal by reflex** for things that could be inline [I].
- **Light or dark chosen by category** ("dev tools are dark") rather than by where and when the product is used [I].
