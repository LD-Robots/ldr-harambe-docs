# Changelog

All notable changes to the LDR Harambe docs repository.

## 2026-06-03 — Whole-body PVT system

Documents the robot-level PVT stack landing in the robot repo — the
multi-joint port of the bench-rig PVT controller plus the new
`harambe_ethercat_driver` and its coupled ankle. The pendulum guides
(`pvt_tuning_guide.html`, `safety_supervisor_guide.html`) are left as
the single-joint bench-rig reference; the robot material goes into new
notes.

### Added

- `tuning/robot_pvt_guide.html` — new field note on
  `robot_pvt_control/RobotPVTController`: the per-joint drive-side PD law,
  body groups (`arms` / `arms_waist` / `legs` / `full`), the five PVT
  command interfaces and `HarambePvtDriver` (RxPDO `0x1601` / TxPDO
  `0x1A02`, ×1000 kp/kd), the new **GRAVCOMP** mode (`~/gravcomp`,
  selective Kp-zeroing with the rate limiter pinned to measured q),
  commanded-vs-measured endpoint hold and start-knot seeding, the 1 kHz
  real-time deployment (`lock_memory` / `thread_priority 80` /
  `cpu_affinity 3`), `~/reference` + `~/command` tuning telemetry,
  `robot_pvt.launch.py` / `robot_pvt_viewer.launch.py`, and the
  per-joint / dual-bus-rail `robot_safety` integration.
- `hardware/ankle_linkage.html` — new field note on the ankle
  eccentric-pushrod linkage: two X4 drives → pitch+roll through cams,
  the `r_exc` / `d_pitch` / `d_roll` / `pitch_sign` / `roll_sign`
  geometry, forward (motor→joint) and inverse (joint→motor) position
  kinematics, the velocity/effort Jacobian with cosine-singularity
  guards, `ankle_linkage_pairs` wiring, the right-ankle pitch/roll name
  swap (`c590020`), and the ±45° / dual-effort safety envelope.

## 2026-05-27 — DAMPING mode & live safety reload

Documents the third controller mode and the runtime-mutable safety
limits added to `pendulum_pvt_control` and `pendulum_safety`.

### Changed

- `strategy/safety_supervisor_guide.html` §05–§08 — `EstopAction` gains
  `DAMPING` (estop_state code `3`); §06 covers all three actions and
  notes the PD path still aliases DAMPING to HOLD; §07 callout that
  every `safety.*` parameter is now hot-reloaded; §08 consumer table
  reflects the new per-controller behaviour.
- `tuning/pvt_tuning_guide.html` §04 / §05 / §08 / §09 — `Mode::DAMPING`
  documented, `~/damp` service example added, live-tuning paragraph
  extended to cover `Kd_damp` and every `safety.*` limit, params table
  gains a `Kd_damp` row.

## 2026-05-26 — pendulum_test_v2 sync

Whole-repo sync after the `pendulum_test_v2` bench rig landed an
entirely new control stack (PVT controller, ONNX policy node,
centralised safety supervisor, filtered joint-state broadcaster,
drive telemetry, RL training pipeline).

### Added

- `strategy/safety_supervisor_guide.html` — new field note on the
  `pendulum_safety` package: limiter library + supervisor node, the
  e-stop signalling contract, per-trigger FREE/HOLD, `safety_limits.yaml`
  as single source of truth, integration shape per controller.
  (`f8d65c2` — *docs: new safety supervisor field note*)
- `tuning/pvt_tuning_guide.html` — new field note on PVT / CiA-402
  mode 5 impedance control on the X6: drive-side vs software PD
  matrix, streaming setpoint contract, FollowJointTrajectory action
  server with the lag governor, offline `pvt_sim_gui` workflow.
  (`3d94ef3` — *docs: new PVT tuning field note*)

### Changed

- `tuning/pd_tuning_guide.html` §11 — three subsections appended on
  the host-side filtered joint-state broadcaster (30 Hz IIR at 1 kHz),
  the `drive_status_broadcaster` telemetry topics, and the move of
  effort clamping out of per-controller helpers into `pendulum_safety`.
  Hero status line updated. (`2a804fd`)
- `strategy/isaac_pendulum_guide.html` §20 — added a subsection on the
  PVT deployment path (mirrors `ImplicitActuatorCfg`); pure-effort
  deployment kept as the alternative. §21 gains four rows for
  safety-supervisor failure modes (latched e-stop on reset, slew
  clipping, stale joint-state breaches, thermal `kp_scale` derate).
  (`0e8c472`)
- `strategy/sim_to_real_guide.html` §02 + §08 — gap-hierarchy section
  now closes by pointing at the central supervisor; architecture
  section gains an annotation panel showing where safety sits relative
  to the per-Option torque flows (*above* the flow, not in series).
  Hero "Stage now" advanced to "PVT bench loop · safety supervisor live."
  (`1625093`)
- `hardware/ethercat_control_guide.html` §23 — added a cross-ref panel
  pointing at `pendulum_pvt_control` for the ros2_control integration
  side of PVT mode 5 (the PVT README already cross-references §23, so
  this reciprocates). (`2f18ad7`)
