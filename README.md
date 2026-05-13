# Humanoid — documentation repository

Living technical docs for the humanoid bring-up. Field notes in the
"Field Notes" HTML design system, organized by topic.

## Start here

- [`design_guide.md`](design_guide.md) — design system reference. CSS tokens,
  components, and the complete CSS block. Read before editing or creating
  any HTML.
- [`design_guide.html`](design_guide.html) — rendered version of the design
  guide.
- [`setup_guide.html`](setup_guide.html) — how this repo and the robot repo
  wire together through Claude Code.

## Tuning

Identification procedures, per-joint tuning reports.

- [`tuning/motor_id_protocol.html`](tuning/motor_id_protocol.html) — motor
  identification protocol.
- [`tuning/pd_tuning_guide.html`](tuning/pd_tuning_guide.html) — PD gain
  selection and tuning procedure.

## Strategy

Pipeline, architecture, decision records.

- [`strategy/sim_to_real_guide.html`](strategy/sim_to_real_guide.html) —
  end-to-end sim-to-real pipeline.
- [`strategy/isaac_pendulum_guide.html`](strategy/isaac_pendulum_guide.html)
  — Isaac Gym training setup for the single-joint pendulum.
- [`strategy/gazebo_actuator.html`](strategy/gazebo_actuator.html) — Gazebo
  actuator model and validation.

## Hardware

Motor specs, wiring, mechanical, sensor calibration.

- [`hardware/ethercat_control_guide.html`](hardware/ethercat_control_guide.html)
  — MyActuator X6 EtherCAT servo control: CiA 402 modes, object dictionary,
  PDO mapping, PVT impedance gains (×1000 scaling).

## Conventions

See [`CLAUDE.md`](CLAUDE.md) for directory conventions, writing voice, and
the doc update protocol. New field notes are scaffolded by the
`write-field-note` skill at [`.claude/skills/write-field-note.md`](.claude/skills/write-field-note.md).

## Robot repo — companion setup

This repo is one half of the system described in
[`setup_guide.html`](setup_guide.html). The other half lives in the robot
code repo. Until the following are added there, Claude Code sessions
started in the robot repo won't see these docs.

1. **`CLAUDE.md`** — robot-repo context plus the docs integration rules.
   Full contents in §08 of [`setup_guide.html`](setup_guide.html).

2. **`.claude/settings.json`** — the centerpiece. The
   `additionalDirectories` array must contain the absolute path to this
   repo so every robot-repo session loads the docs alongside the code.
   Template in §09 of [`setup_guide.html`](setup_guide.html). Use absolute
   paths only — no `~`, no relative.

3. **`.claude/commands/sync-docs.md`** — the `/sync-docs` slash command.
   Diffs robot-repo activity against the last docs commit and proposes
   targeted doc updates. Contents in §10 of
   [`setup_guide.html`](setup_guide.html). Belongs in the robot repo, not
   here — it operates *on* this repo from the outside.

4. **`.gitignore`** — must include `.claude/settings.local.json` so personal
   overrides stay out of version control.

Optional: a `humanoid.code-workspace` file in the parent directory of both
repos opens them side-by-side in one VS Code window (§11).
