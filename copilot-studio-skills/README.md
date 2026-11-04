# Copilot Studio Scientific Skills

Ten scientific skills converted for upload to Microsoft Copilot Studio (Build → Skills → Upload a skill).

- `zips/` — one ready-to-upload ZIP per skill, with `SKILL.md` at the ZIP root
- `src/` — the unzipped contents of each ZIP, for review and editing

Skills: literature-review, peer-review, scientific-writing, scientific-critical-thinking, scholar-evaluation, hypothesis-generation, statistical-analysis, exploratory-data-analysis, scientific-visualization, scientific-brainstorming.

## Changes from the source

Adapted from `skills_all/claude-scientific-skills/scientific-skills/` in
[Microck/ordinary-claude-skills](https://github.com/Microck/ordinary-claude-skills) (MIT License, © 2025 K-Dense Inc.; see each skill's `LICENSE.md`).

- `SKILL.md` frontmatter holds only `name` and `description`
- Removed references to Claude Code tools (WebFetch, WebSearch, grep) and to Claude-only skills (gget, bioservices, datacommons-client)
- Removed the `scripts/` folders; script behavior is now written into the instructions:
  - literature-review: deduplication, DOI checks against CrossRef/doi.org, and document output are described as steps
  - statistical-analysis: standalone SciPy assumption-check example
  - scholar-evaluation: weighted scoring and quality-level tables
  - scientific-visualization: helper functions, palettes, and `.mplstyle` contents moved into `references/style_and_export_helpers.md`
- Kept all `references/` files and the Markdown templates in `assets/`

## Rebuilding a ZIP

```bash
cd src/<skill-name> && zip -qrX ../../zips/<skill-name>.zip . -x '.*'
```
