---
artifact: test-plan
phase: 2
project: "[[Phased Decomposition]]"
release: R1
status: approved
created: 2026-04-14
framework_version: 0.1.0
descends_from:
  - "[[RELEASE]]"
---
# Release-Level Test Plan — Phased Decomposition R1

## Shape of This Test Plan

R1 is validated by one outcome. The test plan binds the release to that outcome and to nothing else. Mechanism-level checks the build loop may benefit from — trigger discrimination, phase routing, interrogator-before-production, kernel-surfacing protocol, sub-bullets of Anthropic conformance — are tools Phase 3 and Phase 5 are free to author, but they are not what the release is held to. The release-level plan does not pre-decide which mechanism checks exist, who runs them, or what their thresholds are. It commits the operator-level outcome and leaves everything beneath that to downstream phases.

This collapse is deliberate. Release validation is what the operator does _with_ the shipped skill, not what happens inside the build loop. If the skill conforms to every mechanism check Phase 3 could devise but the dogfooding artifacts come out wrong, R1 does not ship. If the dogfooding artifacts come out right but some mechanism check is technically missed, R1 probably ships and the miss is logged. The dogfooding outcome dominates everything else, so it is the only outcome the release plan binds.

## The Outcome

**Dogfooding artifact quality.** A fresh Claude session running the [[Projects/Phased Decomposition/CONCEPT|phased decomposition]] skill against March and Nell produces Phase 0 artifacts the operator judges sufficient against the corresponding scenes in [[MOCKUP|mockup]]. The operator is the gate; the mockup scenes are the reference standard. Both runs must clear operator review, or their failures must be explicitly accepted and logged in `SKILL.md` or its references, before R1 ships.

The table below commits the checks the operator performs during validation. Rows are sorted by project. `traces_to` names a parent test row by id when one exists; release-level rows have no parent and the column is empty, but the schema is preserved so Phase 4 spec-level and Phase 5 task-level test plans can name these rows as their parents.

| id       | check                                                                                                                   | traces_to | status  | operator_signoff |
| -------- | ----------------------------------------------------------------------------------------------------------------------- | --------- | ------- | ---------------- |
| R1-TP-01 | Skill triggers and loads on a cold-start request for March                                                              | —         | pending |                  |
| R1-TP-02 | March kernel is surfaced by the operator in their own words, not proposed by Claude                                     | —         | pending |                  |
| R1-TP-03 | Interrogator runs the section sweep against March before production                                                     | —         | pending |                  |
| R1-TP-04 | Sections the operator did not ground for March are stubbed rather than invented                                         | —         | pending |                  |
| R1-TP-05 | March `concept.md` is legible to a cold reader against `MOCKUP` Scene 1 as reference standard                           | —         | pending |                  |
| R1-TP-06 | Skill triggers and loads on a cold-start request for Nell                                                               | —         | pending |                  |
| R1-TP-07 | Nell kernel is surfaced by the operator in their own words, not proposed by Claude                                      | —         | pending |                  |
| R1-TP-08 | Interrogator runs the section sweep against Nell before production                                                      | —         | pending |                  |
| R1-TP-09 | Sections the operator did not ground for Nell are stubbed rather than invented                                          | —         | pending |                  |
| R1-TP-10 | Nell `concept.md` is legible to a cold reader against `MOCKUP` Scene 1 as reference standard                            | —         | pending |                  |
| R1-TP-11 | Failures encountered during either run are resolved in the skill or logged as accepted limitations with operator review | —         | pending |                  |

## Test Plan Closure

The plan closes when every row in §The Outcome is `passed` or `accepted-limitation` with operator signoff recorded, and the conditions in `[[ROADMAP]]` §Definition of R1 done hold.

## Changelog

- **2026-04-14 — Outcome restructured as a traceable table.** §The Outcome reworked from prose-with-bulleted-conditions into a row-per-test table. Schema: `id | check | traces_to | status | operator_signoff`. Eleven rows committed, sorted by project (March block, Nell block, R1-wide row at end). `traces_to` holds parent test ids for downstream child plans; release-level rows have no parent so the column is empty, but the schema is preserved so Phase 4 and Phase 5 test plans can name these rows as parents. §Test Plan Closure rewrote to reference row status and operator signoff instead of the prior three-condition decomposition. _Why:_ the prior structure committed the same substance but as prose, which made the plan unusable as a traceability anchor — downstream plans had no row ids to point at. _Affects:_ §The Outcome, §Test Plan Closure.

- **2026-04-14 — Initial authoring.** Authored as a Phase 2 release-activation sibling artifact alongside the [[ROADMAP|roadmap]] amendment of the same date. Initially drafted as a roadmap subsection; relocated to a sibling file when the operator caught the category error. The test plan commits to one outcome by deliberate collapse of an earlier proposal that bound mechanism-level checks at the release level. The collapse descends from the operator's framing that release validation is what happens _with_ the shipped skill, not inside the build loop.