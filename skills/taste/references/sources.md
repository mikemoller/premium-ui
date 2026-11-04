# Taste and anti-slop: research notes

Compiled 2026-11-04. Two kinds of source sit behind this skill, and they are kept apart:

- **Grounded**: specs, platform guidelines and peer-reviewed papers for hierarchy, type, colour, grouping, contrast and first impressions (sections 1–6).
- **Opinion**: practitioner skills that catalogue what generated UI looks like (sections 7–9). Nobody has measured these, so they are labelled as opinion wherever they are used.

"Unverified" marks what I could not confirm against the primary text.

---

## 1. Why generated UI converges

- Anthropic, "Improving frontend design through Skills", 12 Nov 2025, Rajasekaran, Wei and Bricken: https://claude.com/blog/improving-frontend-design-through-skills
  - Uses the term *distributional convergence* informally: sampling follows the statistics of the training data, so without direction the model lands on the high-probability centre.
  - Names the defaults it produces: Inter, Roboto, Open Sans, Lato, system fonts, purple gradients on white.
  - Its prompt advice includes extreme weight contrast (100/200 against 800/900) and size jumps of 3× or more. That is vendor prompt advice, not evidence.
- Anthropic `frontend-design` skill, Apache-2.0: https://github.com/anthropics/skills/tree/main/skills/frontend-design. The skill was rewritten more than once.
  - **Dec 2025** (commit 00756142): banned Inter, Roboto, Arial, system fonts and purple-on-white gradients by name, and warned against converging on Space Grotesk.
  - **Sep 2026** (41bbe19d, "avoid generic design defaults"): names no fonts. Instead it asks to:
    - ground the design in the subject;
    - use one or two families;
    - keep lines under about 80 characters;
    - pick 4–6 named colours;
    - make every border, label or number encode information.
  - The Sep 2026 version also lists new clichés: a cream (#F4F1EA) and terracotta (#D97757) palette, identical rounded cards with soft shadows, tracked eyebrow labels, 01/02/03 markers, "→" on buttons, and fade-up on every section.
  - Cite a commit, because the content keeps changing.

## 2. Hierarchy and layout

- Apple HIG, Layout, https://developer.apple.com/design/human-interface-guidelines/layout (platform guideline):
  - group related items with negative space, container shapes or separators;
  - order content by importance, top and leading side first;
  - indented items read as subordinate;
  - respect safe areas, margins and layout guides.
  - Apple publishes no general point grid.
- Material 3, spacing, https://m3.material.io/foundations/layout/grids-spacing/spacing (design system): similar elements get the same spacing and size, and generous space frames what matters. M3 spacing is "measured on an 8dp scale". The "4dp grid" comes from the Material 2 era and is unverified for M3.
- Müller-Brockmann, *Grid Systems in Graphic Design* (Niggli, 1981): the classic reference for modular grids. Book; the ISBN is from memory.
- Primary research on size contrast as a hierarchy cue: I found nothing solid, so hierarchy rules are platform convention.

## 3. Gestalt grouping (DOIs checked via Crossref)

- Wertheimer, M. (1923). Untersuchungen zur Lehre von der Gestalt II. *Psychologische Forschung* 4, 301–350. https://doi.org/10.1007/BF00410640
  - Proximity, similarity, common fate, good continuation, closure.
  - Demonstrations, not quantified experiments.
- Palmer, S. E. (1992). Common region: A new principle of perceptual grouping. *Cognitive Psychology* 24, 436–447. https://doi.org/10.1016/0010-0285(92)90014-S
  - A shared bounded region groups elements and can override proximity.
  - Experimental.
- Palmer, S. & Rock, I. (1994). Rethinking perceptual organization: the role of uniform connectedness. *Psychonomic Bulletin & Review* 1, 29–55. https://doi.org/10.3758/BF03200760

Design implication (my inference): common region is strong, which is why boxes work, and also why too many boxes (nested cards) over-segment a page. Space should do most of the grouping.

## 4. Typography

- **Material 3 type scale** (sp, size/line-height) from the token source `_md-sys-typescale.scss`. Reader page: https://m3.material.io/styles/typography/type-scale-tokens.
  - Display 57/64, 45/52, 36/44.
  - Headline 32/40, 28/36, 24/32.
  - Title 22/28, 16/24, 14/20.
  - Body 16/24, 14/20, 12/16.
  - Label 14/20, 12/16, 11/16.
  - M3 says no product uses every style.
- **Apple HIG, Typography**, https://developer.apple.com/design/human-interface-guidelines/typography:
  - iOS default text 17 pt, minimum 11 pt.
  - Minimise the number of typefaces.
  - Avoid Ultralight, Thin and Light weights.
  - Show hierarchy with weight, size and colour.
- **Modular scale**: Tim Brown, "More Meaningful Typography", *A List Apart*, 2011. https://alistapart.com/article/more-meaningful-typography/
  - A base size multiplied by a ratio.
  - Presented as a tool. No ratio has been shown to be better than another, so this is convention.
- **Measure**:
  - Bringhurst, *The Elements of Typographic Style* (1992), "Choose a comfortable measure": 45–75 characters for single-column text, with 66 "widely regarded as ideal". Bringhurst presents this as received convention.
    - Attribution confirmed through quoting sources, not the book itself; the section number needs checking against the book.
  - WCAG 2.2 SC 1.4.8 (AAA): no more than 80 characters, not justified, line spacing at least 1.5. https://www.w3.org/WAI/WCAG22/Understanding/visual-presentation.html
  - Dyson, M. C. & Haselgrove, M. (2001). The influence of reading speed and line length on the effectiveness of reading from screen. *IJHCS* 54(4), 585–612. https://doi.org/10.1006/ijhc.2001.0458
    - About 55 characters per line gave the best comprehension.
    - Read from the abstract only.
  - Dyson & Kipping (1998): 100 cpl read faster than 25 cpl, with 55 rated easiest. Secondary snippet only.
  - Conclusion: people read long lines faster but prefer moderate ones, so the evidence is mixed. Don't present 66 as a research finding.
- **Do not cite WCAG 1.4.12 as "use 1.5 line-height."** It is a robustness test: content must survive the user setting those values.

## 5. Colour

- **CSS Color 4**, https://www.w3.org/TR/css-color-4/ (Candidate Recommendation Draft, Nov 2026):
  - §7: HSL hue and lightness are not perceptually uniform.
  - §9.2: OkLCh has better hue linearity and chroma uniformity than CIE LCH, whose blues drift purple as chroma drops. Gamut mapping is done in OkLCh.
- **Material 3 HCT**, https://m3.material.io/blog/science-of-color-design:
  - Tone is L*; hue and chroma come from CAM16.
  - Google's rule for WCAG: a tone difference of 50 for small elements and 40 for large. This is Google's claim and was not checked against the WCAG maths.
- **Apple HIG, Color**, https://developer.apple.com/design/human-interface-guidelines/color:
  - don't use one colour for different meanings;
  - convey information in more than one way;
  - consider a limited palette;
  - work in light, dark and increased-contrast modes.
  - "Use color sparingly" appears only in the Liquid Glass context, so don't quote it as a general rule.
- **Apple HIG, Dark Mode**, https://developer.apple.com/design/human-interface-guidelines/dark-mode: dark colours aren't inversions of the light ones, and Dark Mode uses base and elevated backgrounds.
- **Albers, J. (1963). *Interaction of Color*. Yale University Press**: colour is relative to its surroundings. Demonstration exercises, not experiments.
- **WCAG 1.4.1 Use of Color (A)**: the normative version of "not colour alone".

## 6. Contrast and first impressions

- **WCAG 2.2** SC 1.4.3 (AA, 4.5:1 / 3:1 large; large = 18 pt or 14 pt bold), 1.4.6 (AAA, 7:1 / 4.5:1) and 1.4.11 (AA, 3:1 non-text). https://www.w3.org/TR/WCAG22/
  - Ratios are not rounded: 4.499:1 fails.
- **APCA is not normative anywhere.** The WCAG 3 Working Draft (10 Sep 2026) says the contrast algorithm "is yet to be determined" and doesn't name APCA. https://www.w3.org/TR/wcag-3.0/
- **Lindgaard, Fernandes, Dudek & Brown (2006).** Attention web designers: You have 50 milliseconds to make a good first impression! *Behaviour & IT* 25(2), 115–126. https://doi.org/10.1080/01449290500330448
  - Appeal ratings at 50 ms correlated highly with those at 500 ms.
  - Caveats: appeal only, not usability or trust; small sample; mid-2000s pages.
- **Tuch, Presslaber, Stöcklin, Opwis & Bargas-Avila (2012).** The role of visual complexity and prototypicality regarding first impression of websites. *IJHCS* 70(11), 794–811. https://doi.org/10.1016/j.ijhcs.2012.06.003
  - Low complexity plus high prototypicality was rated most appealing.
  - Complexity acts within 17 ms; prototypicality's effect grows with exposure.
  - **This is in tension with "be distinctive"**: unusual layouts can cost first-impression appeal. Hence the skill's split: distinctive for marketing and brand moments, conventional for app controls.
  - Not the same paper as Tuch et al. 2012 in *CHB*, which `ux-psychology` cites.
- **Reinecke, Yeh, Miratrix, Mardiko, Zhao, Liu & Gajos (2013).** Predicting users' first impressions of website aesthetics with a quantification of perceived visual complexity and colorfulness. CHI '13, 2049–2058. https://doi.org/10.1145/2470654.2481281
  - 450 sites, 548 participants.
  - The models explain about half the variance; complexity matters much more than colourfulness.
  - Preferences vary by demographics (from memory).
- **Whitespace**: the claim that "whitespace improves comprehension by about 20%" is unverified and appears only in blogs. Lin 2004 (CHB 20(4)) is real but is not about whitespace.
  - The closest study is Chaparro et al. (2004), "Reading online text: a comparison of four white space layouts", *Usability News* 6.2, Wichita State: margins helped speed and comprehension, while leading affected preference only. A lab newsletter, not peer-reviewed. https://portfolio.erau.edu/en/publications/reading-online-text-a-comparison-of-four-white-space-layouts/

## 7. Impeccable (Paul Bakaus)

https://impeccable.style/, https://github.com/pbakaus/impeccable. **Apache-2.0.** It started from Anthropic's frontend-design skill (per its README). Read at tag skill-v4.8.0, with older reference files from skill-v3.0.4.

- **Commands** (`/impeccable <command> <target>`; narrow targets work better):
  - Setup: init (writes PRODUCT.md), document (DESIGN.md from code), extract, shape (a UX brief with no visuals).
  - Review: critique, audit.
  - Steer: bolder, quieter, distill, colorize, typeset, layout, animate, delight, overdrive.
  - Finish: polish, harden, clarify, adapt, optimize, onboard.
  - Live: live, generate.
- **Modes by surface**: Persuade (marketing), Operate (apps), Read (docs), Experience (portfolios).
- **Detector**: 62 rules tagged "slop" or "quality", plus 6 design-review patterns that need judgement. It runs as a CLI or hook and needs no model. 12 rules are fixed immediately at edit time. Full rule list: [slop.md](slop.md).
- **Critique**:
  - Two independent assessments, ideally as sub-agents.
  - Assessment A, a design-director review:
    - a specificity verdict (could an unrelated product use this unchanged?);
    - cognitive load;
    - the emotional journey (peak–end);
    - Nielsen's 10 heuristics, each 0–4, for a total out of 40. Bands: 36–40 excellent, 28–35 good, 20–27 acceptable, 12–19 poor, under 12 critical.
  - Assessment B: the detector plus browser evidence.
  - Output: 2–3 strengths, then 3–5 priority issues tagged P0–P3. Tie-break: "would a user contact support?" If yes, it is at least P1.
  - Persona passes:
    - Alex, impatient power user
    - Jordan, confused first-timer
    - Sam, accessibility-dependent
    - Riley, stress tester
    - Casey, distracted mobile user
- **Audit**: five dimensions scored 0–4, for a total out of 20. The dimensions are accessibility, performance, theming, responsive, and implementation integrity.
- **Craft floor** (its conventions):
  - contrast 4.5:1 and 3:1, with secondary text on colour tinted from the surface;
  - more space above a heading than below;
  - measure 65–75ch, display at most 6rem, tracking no tighter than −0.04em;
  - one authored motion moment;
  - card radius 12–16 px.
  - It refuses reflexive modals, monospace as costume, emoji as icons, and picking the theme by category.
- **Older reference numbers** (v3.0.4):
  - modular type ratio 1.25, 1.333 or 1.5;
  - OKLCH, with neutrals tinted at chroma 0.005–0.015;
  - 60-30-10 colour split;
  - 4 pt spacing base;
  - durations of 100–150 / 200–300 / 300–500 / 500–800 ms, with exits about 75% of the entrance.
  - Two figures it repeats are untraced; don't use them: "vestibular disorders affect 35% of adults over 40" and "80 ms instant".
- **PRODUCT.md** holds product truth only, never visuals:
  - users, purpose, positioning, context;
  - constraints, brand commitments;
  - evidence on hand and what must not be fabricated;
  - principles, accessibility.
- **DESIGN.md**: YAML tokens (colours, typography, radii, spacing, components) followed by 8 prose sections: Overview, Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Do's and Don'ts.

## 8. Taste Skill (Leon Lin)

https://github.com/Leonxlnx/taste-skill, https://www.tasteskill.dev/docs. **MIT.** The repo was restructured in Oct 2026, so pin commits: main 12af471f (v2.1.5, script-driven) and 51c272e2 (the last single-file v2-experimental and v1). The docs site lags the repo.

- **Design read** (v2-exp §0):
  - Infers page kind, vibe words, references, audience, brand assets and quiet constraints (public sector, regulated, kids, accessibility). Quiet constraints override aesthetics.
  - States one sentence before any code. Asks a question only when readings genuinely diverge.
- **Dials** (v2-exp, 1–10):
  - Variance: symmetric up to artsy.
  - Motion: static up to cinematic.
  - Density: gallery up to cockpit.
  - Baseline 8/6/4, inferred from the brief.
  - The current version drops the dials. Instead a script deals 3 of 20 researched directions, and the agent picks one against 8–12 facts about the subject.
- **Locks**: one accent across the page; one radius system (sharp, 12–16 px soft, or pill); one page theme.
- **Current house rules** (paraphrased):
  - headings at most 2 lines at every width;
  - one ground, one ink, one accent;
  - no gradients except photo scrims;
  - no resting shadows or glows;
  - no eyebrows;
  - no monospace outside code;
  - at most two typefaces and five sizes;
  - body 16–20 px, leading 1.45–1.6, 45–75 characters;
  - no invented proof.
- **Removal is not enough**: an ornament-free empty page also reads as generated. Fill it with scale, real content, real images and colour fields.
- **Idea sentence**: "On this page, [something only this subject has] becomes [the device]", plus a competitor test.
- **Redesign protocol**:
  - Modes: greenfield, preserve or overhaul.
  - Audit before touching anything.
  - Fix order: type, colour, frame, states, stock components.
  - Never change URLs, nav labels, form fields, logo or legal copy without approval.
  - Inspect before and after.
- **Self-critique**: weighted like a jury (design 40, usability 30, creativity 20, content 10), with hard caps for any tell.
- **Evidence claim**: the rules are said to come from a study of 247 gallery sites. The data was not traced, so treat it as the author's own and unverified. The skill also admits that some bans are the owner's taste and are more common on top sites.

## 9. Other skills surveyed

Snyk's roundup "Top Claude skills for UI/UX engineers" (2 Mar 2026), https://snyk.io/articles/top-claude-skills-ui-ux-engineers/, lists:
- Anthropic frontend-design
- Vercel web-design-guidelines, react-best-practices, composition-patterns and react-native-skills
- UI/UX Pro Max
- Bencium UX Designer
- AccessLint

Its security note: skills can bundle scripts, so read every SKILL.md and script before installing. This suite ships no scripts.

## Gaps

- Not traced: Taste Skill's 247-site data, Impeccable's per-category placement of some rules on its site, the exact Bringhurst page, and the Müller-Brockmann and Albers ISBNs.
- No primary research exists for any individual "tell"; they are pattern observations.
