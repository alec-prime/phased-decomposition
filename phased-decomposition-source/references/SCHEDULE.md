---
artifact: schedule
phase: 2
project: "[[Phased Decomposition]]"
release: R1
status: approved
created: 2026-04-14
framework_version: 0.1.0
descends_from:
  - "[[RELEASE]]"
---
# Release Schedule — Phased Decomposition R1

## Shape of This Schedule

R1 commits a working cadence and a milestone sequence terminating in ship. The schedule is what binds R1's execution to the operator's calendar; it lives at the release level rather than the project level because the cadence and milestones are R1-specific commitments. Future releases would author their own schedules at activation. R1 is the only release this project ships, so this document is the only schedule the project will ever have.

The schedule is a target, not a contract. The contract is `[[RELEASE]]` §Definition of Done. If the schedule pressures the loop, the schedule moves; the standard does not.

## Working Cadence

A few hours most days. The schedule assumes the project stays warm in the operator's head between sessions because the gap between sessions is short.

## Milestones

Rows are discrete dated events. `gate` names what closes the milestone — the artifact criterion or the role whose judgment closes it. `traces_to` names the upstream artifact section the gate descends from. Status flips when the gate closes.

| id  | milestone                                                         | target date | gate                                                         | traces_to                                                            | status  |
| --- | ----------------------------------------------------------------- | ----------- | ------------------------------------------------------------ | -------------------------------------------------------------------- | ------- |
| M1  | Phase 3 closes (`architecture.md` approved)                       | 2026-04-17  | Operator review                                              | `ARCHITECTURE.md` §Phase 3 Terminal                                  | pending |
| M2  | Phase 4 closes (`spec.md` + spec-level test plan approved)        | 2026-04-20  | Operator review                                              | `ARCHITECTURE.md` §Phase 3 Terminal (Phase 4 trigger)                | pending |
| M3  | Phase 5 closes (execution plan + eval set approved)               | 2026-04-23  | Operator review at dual-artifact gate                        | `CONCEPT.md` §The Phases — Phase 5                                   | pending |
| M4  | Phase 6 build loop terminates (mechanical evals pass on held-out) | 2026-04-27  | Supervisor verdict on held-out evals                         | `RELEASE.md` §Binding Quality Standard                               | pending |
| M5  | March dogfooding gate closes                                      | 2026-04-29  | Operator judgment                                            | `TEST-PLAN.md` R1-TP-01..05                                          | pending |
| M6  | Nell dogfooding gate closes                                       | 2026-04-30  | Operator judgment                                            | `TEST-PLAN.md` R1-TP-06..10                                          | pending |
| M7  | R1 ships                                                          | 2026-04-30  | All test-plan rows passed or accepted-limitation, signed off | `TEST-PLAN.md` §Test Plan Closure + `RELEASE.md` §Definition of Done | pending |

## Schedule Closure

The schedule closes when M7 closes per `[[RELEASE]]` §Definition of Done and `[[TEST-PLAN]]` §Test Plan Closure. There is no separate schedule-level closure condition; the schedule exists to bind execution to the calendar, and the calendar's job ends when the release ships.

## Changelog

- **2026-04-14 — Restructured around a milestone table.** Replaced the prose commitments (working cadence, ship target, next contact point) with a seven-row milestone table covering phase closes, dogfooding gates, and ship. Each row names its gate and traces upward to the artifact section the gate descends from. Working cadence kept as a separate prose section since it binds frequency rather than discrete events. §What Falls Out of the Cadence updated to reference milestone ids; §What This Schedule Does Not Bind updated to clarify the table commits phase closures rather than intra-phase checkpoints. `descends_from` updated from `[[ROADMAP]]` to `[[RELEASE]]` per the Phase 2 cluster correction. _Affects:_ structure of `SCHEDULE.md`; no downstream artifacts.
    
- **2026-04-14 — Initial authoring.** Authored as a Phase 2 release-activation sibling artifact alongside the [[ROADMAP|roadmap]] amendment of the same date. The cadence anchor (a few hours most days), ship target (end of April 2026), and next contact point (begin `architecture.md` rewrite) come from operator commitments made during the same session. Three consequences of the tight window named explicitly: phase boundaries stay crisp, dogfooding is the rate-limiting step, and a slip moves the ship date rather than cutting scope.