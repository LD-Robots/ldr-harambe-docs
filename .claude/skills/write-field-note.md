---
name: write-field-note
description: Create a new technical documentation HTML file in the Field
  Notes design system. Trigger when the user asks to create a new guide,
  doc, reference, report, or write-up on a topic.
---

# Writing a field note

When creating a new technical doc, follow this procedure precisely.

## 1. Read the design system first
Before writing any HTML, read `design_guide.md` at the docs repo root.
Use its CSS tokens, component patterns, and the complete CSS block at the
bottom of that file verbatim. Do not invent new styling, new tokens, or
alternative layouts.

## 2. Determine the filename and location
Based on the topic, pick the right subdirectory:

- `tuning/`   — motor identification, PD tuning, per-joint reports
- `strategy/` — pipeline, architecture, decision records, planning
- `hardware/` — motor specs, wiring, mechanical, sensor calibration

Filename: `topic_verb_guide.html` or `topic_report.html`. Examples:
- `tuning/shoulder_pd_tuning_guide.html`
- `strategy/deployment_pipeline_guide.html`
- `hardware/ethercat_master_setup_guide.html`

If the topic doesn't fit existing directories, ask before creating a new one.

## 3. Required structure
Every doc has:

1. `<header class="hero">` with:
   - Eyebrow text (mono, tracked, amber)
   - H1 title with italic Fraunces + one amber emphasis word
   - Subtitle in Fraunces light
   - 4-item meta row (Status, Target, Stack, or similar)
   - Table of contents

2. Numbered sections `<section id="...">` each with:
   - `<span class="section-num">§ NN — short descriptor</span>`
   - H2 with italic amber `<em>` on one emphasis word
   - Optional `<p class="lead">` first paragraph

3. At least one of each high-impact component:
   - `<div class="callout">` — the thesis line in Fraunces italic
   - `<div class="panel" data-label="...">` — a key insight or rules list
   - `<table class="ref">` — tabular reference if applicable

4. Footer with doc identification and date.

## 4. Part dividers (for docs with 10+ sections)
If the doc has more than ~10 sections, group them into Parts using the
`.part` structure from the design guide. Parts have big Fraunces italic
headings and a brief pdesc descriptor.

## 5. Charts (if applicable)
- Use Chart.js loaded from jsDelivr CDN.
- Primary series color: `#e8a33d` (amber).
- Secondary: `#5fb3b3` (cyan).
- Warning/wrong: `#d97757` (rose).
- Grid: `#1e1a13`. Ticks/labels: `#a79a82`.
- Always include units in axis titles: `time [s]`, `torque [N·m]`.
- Legend at bottom, mono font.

## 6. Math (if applicable)
- Load KaTeX from jsDelivr CDN (see design_guide.md for exact snippet).
- Wrap display math in `<div class="math-block labeled">` with a
  `<span class="lbl">` caption above it.
- Follow every math block with a `<dl class="vars">` variable legend
  OR prose that uses the symbols.

## 7. After creating the file
1. Add a link from `README.md` under the appropriate section.
2. Add an entry to `CHANGELOG.md` if it exists.
3. Show the user the final file and summarize what's inside.
4. Offer to open it: `xdg-open <path>` on Linux.

## Checklist before marking done
- [ ] design_guide.md was read before writing.
- [ ] Filename matches the pattern.
- [ ] File is in the correct subdirectory.
- [ ] Hero, numbered sections, at least one callout/panel/table present.
- [ ] Chart colors and font are from the palette.
- [ ] README.md updated with a link.
- [ ] No invented CSS classes.