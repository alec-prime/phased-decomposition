---
artifact: schedule
phase: 2
project: "[[Phased Decomposition]]"
release: R1
status: draft
created: 2026-04-14
framework_version: 0.1.0
descends_from:
  - "[[ROADMAP]]"
---
# Release Schedule — Phased Decomposition R1

## Shape of This Schedule

R1 commits a working cadence, a ship target, and a next contact point. The schedule is what binds R1's execution to the operator's calendar; it lives at the release level rather than the project level because the cadence and target are R1-specific commitments. Future releases would author their own schedules at activation. R1 is the only release this project ships, so this document is the only schedule the project will ever have.

The schedule is a target, not a contract. The contract is `[[ROADMAP]]` §Definition of R1 done. If the schedule pressures the loop, the schedule moves; the standard does not.

## The Commitments

- _Working cadence._ A few hours most days. Not every day, but the default expectation is contact-most-days rather than weekly windows or irregular bursts. The schedule assumes the project stays warm in the operator's head between sessions because the gap between sessions is short.
- _Ship target._ End of April 2026. Two weeks from the date this schedule is committed.
- _Next contact point._ Begin `architecture.md` rewrite. The roadmap amendment closes with this commit; architecture work begins in the next session.

## What Falls Out of the Cadence

Two weeks is tight for R1's scope — Phases 3, 4, 5, and 6 across an Anthropic-conforming skill, plus two dogfooding runs. The schedule deliberately accepts the tightness rather than padding it. Three consequences are named here so they do not surface as surprises mid-execution.

- _Phase boundaries are crisp, not extended._ Each downstream phase resolves its commitments and closes; revisits to upstream artifacts run as explicit amendments with changelog entries, not as quiet extensions of the prior phase. Two weeks does not survive ambiguous phase state. If a phase has not closed by the time the next phase is supposed to begin, the right move is naming what is unresolved and committing it as an open question with a graduation trigger, not extending the phase indefinitely.
- _Dogfooding is the long pole._ Skill authoring (Phases 3–6) compresses against the calendar; the dogfooding runs do not, because they require the operator to sit with fresh artifacts and judge them in their own time. The schedule treats dogfooding as the rate-limiting step, which means skill authoring needs to finish with enough headroom for two dogfooding cycles plus any failure-routing back through skill revision and re-runs.
- _A slip moves the ship date, not the scope._ R1's scope is the binding standard from `[[ROADMAP]]` §Binding quality standard and `[[TEST-PLAN]]`'s dogfooding outcome. If the calendar is going to be missed, the right move is moving the ship target — not removing March or Nell from dogfooding, not lowering the operator's sufficiency bar on the artifacts, not skipping Phase 5's eval authoring. End of April is the target. The hard commit is that R1 closes when its standards hold.

## What This Schedule Does Not Bind

- _Per-phase deadlines._ The schedule commits an overall ship target, not Phase 3 closes by date X, Phase 4 by date Y. Internal phase scheduling is the operator's call session-by-session, against the working cadence.
- _Session structure._ How long a session runs, what gets done in a session, when a session ends — none of this is committed here. The schedule binds frequency, not duration or content.
- _Recovery protocol if the schedule slips._ If end of April is missed, the recovery is "move the target and revise this document." The schedule does not pre-author what the new target would be.

## Schedule Closure

The schedule closes when R1 ships per [[ROADMAP|Roadmap]] of R1 done and [[TEST-PLAN|Test Plan]] Closure. There is no separate schedule-level closure condition; the schedule exists to bind execution to the calendar, and the calendar's job ends when the release ships.

## Changelog

- **2026-04-14 — Initial authoring.** Authored as a Phase 2 release-activation sibling artifact alongside the [[ROADMAP|roadmap]] amendment of the same date. The cadence anchor (a few hours most days), ship target (end of April 2026), and next contact point (begin `architecture.md` rewrite) come from operator commitments made during the same session. Three consequences of the tight window named explicitly: phase boundaries stay crisp, dogfooding is the rate-limiting step, and a slip moves the ship date rather than cutting scope.