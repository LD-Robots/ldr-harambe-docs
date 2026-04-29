# Humanoid project — documentation repository

## Purpose
Evolving technical documentation for the humanoid robot bring-up:
PD tuning, motor ID, sim-to-real strategy, Gazebo/Isaac integration,
hardware notes. Docs are living — they grow and mutate with the project.

## Design system
All HTML docs use the "Field Notes" design system defined in
`design_guide.md` at the repo root. Before creating or editing any HTML:

1. **For new docs, copy `boilerplate.html` and rename.** It carries the
   full token set, the reactive-palette toggle (Field Notes ↔ VSCode-matching),
   and the standard hero/section/footer skeleton already wired up.
2. Read `design_guide.md` to pick up the CSS tokens, component patterns,
   and complete CSS block at the end of the file.
3. Use the components defined there — panels, callouts, cards, steps,
   bullets, math-block, tables — don't invent parallel ones.
4. When in doubt, match the tone and structure of an existing doc in
   the same subdirectory.

## Directory conventions
- `tuning/`    — identification procedures, per-joint tuning reports
- `strategy/`  — pipeline/architecture docs, decision records
- `hardware/`  — motor specs, wiring, mechanical notes
- Filenames follow `topic_verb_guide.html` (e.g., `pd_tuning_guide.html`)
  or `topic_report.html` for one-off reports.

## Writing voice
- Direct. Acknowledge what's already been done.
- Honest assessment over flattery.
- Concrete over abstract — use real measured numbers when available.
- No preamble, no "great question" openings.
- Prefer prose and short paragraphs over deep bullet nesting.

## Project state (update when this changes)
- Stage: real-arm calibration — left arm (6 DoF, 5.1 kg URDF chain mass,
  X6/X6/X4/X6/X4/X4) running CST gravity comp via `humanoid_pd_tuning`
- Bench-rig identified params (single-joint X6, 70 cm bar, 1 kg @ 30 cm):
  J ≈ 0.102 kg·m², mgl = 2.943 N·m, F_c ≈ 0.15 N·m,
  F_v ≈ 0.05 N·m·s/rad, τ_m ≈ 10 ms
- Active task: discovering per-joint k_g on real hardware; URDF inertials
  suspect (gravity_comp_node with k_g = 1 drove arms to limits)
- Stack: ROS 2 (effort control), Isaac Gym training, Gazebo validation
- End goal: ONNX policies transferring to full humanoid

## When editing existing docs
- Preserve `§ NN` section numbers. If inserting a section, renumber all
  following ones and update the TOC.
- Keep chart data inline (in the same HTML file) — don't split into
  separate JS files.
- Update the hero's "Status" meta row when project state changes.
- If the change is significant, add an entry to `CHANGELOG.md`.

## Git discipline
- Commit docs separately from code changes.
- Prefix commit messages with `docs:` when committing here.
- One logical change per commit.