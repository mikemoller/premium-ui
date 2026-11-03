# Working on this repo

A collection of agent skills. Each skill is `skills/<name>/SKILL.md` (frontmatter `name` matching the folder, and a `description` that says what it covers and when to use it) plus `references/sources.md` with the research notes behind it.

- Every rule cites the primary source that owns it (a platform guideline, spec, standard or the original paper), not a blog post summarising it. If only a secondary source exists, say so.
- Say how strong the evidence is. Mark conventions as conventions and unverified numbers as unverified; don't repeat a figure you couldn't trace.
- Keep `SKILL.md` short enough to load in full (rules, a checklist, links). Detail goes in `references/`.
- Keep skills generic: no product-specific examples.
- A new skill is added to `.claude-plugin/marketplace.json`, `skills.sh.json` and the README table.
- Check discovery before pushing: `npx skills add . --list`.
