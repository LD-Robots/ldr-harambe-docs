# Changelog

All notable changes to the LDR Harambe docs repository.

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
