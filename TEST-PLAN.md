---

artifact: test-plan 
phase: 2 
project: "[[Phased Decomposition]]" 
release: R1 
status: draft 
created: 2026-04-14 
framework_version: 0.1.0 
descends_from:

- "[[ROADMAP]]"
- "[[MOCKUP]]"

---
# Release-Level Test Plan — Phased Decomposition R1

## Shape of This Test Plan

R1 is validated by one outcome. The test plan binds the release to that outcome and to nothing else. Mechanism-level checks the build loop may benefit from — trigger discrimination, phase routing, interrogator-before-production, kernel-surfacing protocol, sub-bullets of Anthropic conformance — are tools Phase 3 and Phase 5 are free to author, but they are not what the release is held to. The release-level plan does not pre-decide which mechanism checks exist, who runs them, or what their thresholds are. It commits the operator-level outcome and leaves everything beneath that to downstream phases.

This collapse is deliberate. Release validation is what the operator does _with_ the shipped skill, not what happens inside the build loop. If the skill conforms to every mechanism check Phase 3 could devise but the dogfooding artifacts come out wrong, R1 does not ship. If the dogfooding artifacts come out right but some mechanism check is technically missed, R1 probably ships and the miss is logged. The dogfooding outcome dominates everything else, so it is the only outcome the release plan binds.

## The Outcome

**Dogfooding artifact quality.** A fresh Claude session running the [[Projects/Phased Decomposition/CONCEPT|phased decomposition]] skill against March and Nell produces Phase 0 artifacts the operator judges sufficient against the corresponding scenes in [[MOCKUP|mockup]]. The operator is the gate; the mockup scenes are the reference standard. Both runs must clear operator review, or their failures must be explicitly accepted and logged in `SKILL.md` or its references, before R1 ships.

The outcome decomposes into three observable conditions, each of which the operator confirms as part of validation:

- _March Phase 0 run produces a `concept.md` the operator finds sufficient._ Sufficient is judged against Scene 1 in the mockup as the reference standard. The kernel is surfaced by the operator in their own words rather than proposed by Claude; the section sweep produces grounded sections; sections the operator did not ground are stubbed rather than invented.
- _Nell Phase 0 run produces a `concept.md` the operator finds sufficient._ Same standard, applied to a different project type. Nell is included specifically because it is a different shape of project from March; passing both is the first evidence the framework's project-type generalization claim holds for the two stress-tested cases.
- _Failures encountered during either run are either resolved in the skill or explicitly accepted as logged limitations._ A failure is anything the operator notices that the skill should have caught or guided differently. Resolution means the skill is amended and the run is re-attempted; acceptance means the limitation is logged in `SKILL.md` or its references with the logging itself reviewed by the operator. The release does not close on silent failures.

## What This Plan Does Not Bind

Naming the things this plan deliberately leaves open, so downstream phases do not interpret silence as prohibition or as requirement.

- _Mechanism-level evals._ Phase 3 architecture may commit an eval suite, may not, or may commit a smaller version than the prior architecture draft proposed. This plan does not require an eval suite to exist for R1 to ship. If Phase 3 commits one, its threshold and contents are Phase 3's call; if it does not, the dogfooding outcome alone validates the release.
- _The draft→test→refine loop's internal gates._ The loop runs in Phase 6 against whatever supervisor standards Phase 5 commits. This plan does not name those standards; they descend from Phase 5's eval authoring, not from here.
- _Trigger rate measurement._ §Binding quality standard in [[ROADMAP|roadmap]] names trigger optimization as a Phase 3/5/6 activity. This plan does not commit a trigger rate threshold or a measurement method. Both descend downstream.
- _Per-phase mechanism checks beyond Phase 0._ Dogfooding only stress-tests Phase 0 because March and Nell are at the cold-start point. R1 does not validate the skill against Phases 1–6 because no dogfooding project has reached them within R1's window. This is an accepted limitation, not a gap in the plan.

## What Failure Looks Like

A failure of the dogfooding outcome takes one of three shapes. Each routes differently.

- _The skill fails to trigger or fails to load when it should._ The fresh Claude session does not invoke the skill against the dogfooding project. Routes to a description-tuning loop (mechanism check, but the operator surfaces it through the dogfooding attempt rather than through a separate eval). Skill is amended; run is re-attempted.
- _The skill triggers but produces a `concept.md` the operator judges insufficient._ The artifact came out, the operator does not find it good enough against the mockup scenes. Routes to skill content revision — `SKILL.md`, the relevant phase reference file, or the relevant template. The diagnosis (which file is wrong, what about it is wrong) is the operator's call, with the failed artifact as evidence.
- _The skill triggers and produces an artifact the operator judges sufficient on the surface, but the operator notices a downstream-affecting issue._ The kernel is plausible but not the right kernel; a section is grounded but the grounding mechanism was Claude proposing rather than the operator naming. Routes to either skill revision (if the skill should have caught it) or accepted-limitation logging (if the failure is at a level the skill cannot reasonably catch). The acceptance route is the escape valve that prevents the release from being held hostage to perfection.

## Out of Scope

- _Generalization beyond March and Nell._ R1 does not test the framework's claim of generalizing across project types beyond what dogfooding stress-tests. Per `[[ROADMAP]]` §Scope exclusions, this is an accepted risk, not a gated outcome.
- _Validation of Phases 1–6._ Dogfooding only covers Phase 0 because March and Nell are at cold start. R1 ships with the framework's later phases unvalidated against fresh projects. This is an accepted limitation.
- _Performance, latency, token cost._ R1 ships a markdown skill; performance properties are out of scope for release validation.
- _Comparison against alternative frameworks._ R1 is held to its own standard, not to a competitive standard.

## Test Plan Closure

The test plan closes when all three observable conditions in §The Outcome hold:

- March Phase 0 run cleared.
- Nell Phase 0 run cleared.
- All encountered failures resolved in the skill or logged as accepted limitations with operator review.

R1 ships when this plan closes and the conditions in `[[ROADMAP]]` §Definition of R1 done hold.

## Changelog

- **2026-04-14 — Initial authoring.** Authored as a Phase 2 release-activation sibling artifact alongside the [[ROADMAP|roadmap]] amendment of the same date. Initially drafted as a roadmap subsection; relocated to a sibling file when the operator caught the category error. The test plan commits to one outcome by deliberate collapse of an earlier proposal that bound mechanism-level checks at the release level. The collapse descends from the operator's framing that release validation is what happens _with_ the shipped skill, not inside the build loop.