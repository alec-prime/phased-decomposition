---
name: phased-decomposition-source
description: "Preloaded framing artifacts for the phased decomposition skill build. Contains byte-for-byte copies of all Phase 0–4 artifacts: CONCEPT.md, NORTHSTAR.md, MOCKUP.md, ROADMAP.md, RELEASE.md, ARCHITECTURE.md, SPEC.md, TEST-PLAN.md, SCHEDULE.md, project-notes.md. Read the relevant artifact when the work package names it as an input."
---
# Phased Decomposition Source Artifacts

This skill contains the framing artifacts from Phases 0–4 of the Phased Decomposition project. These are the source material the producer authors the skill against.

## Contents

All files are in `references/` within this skill directory:

| File | Phase | Purpose |
|---|---|---|
| `CONCEPT.md` | 0 | Project concept — kernel, problem, idea, precedents |
| `NORTHSTAR.md` | 1 | Abstraction-level scenes |
| `MOCKUP.md` | 1 | Concrete instantiation scenes |
| `ROADMAP.md` | 1 | Release sequence |
| `RELEASE.md` | 2 | R1 release artifact — what ships, binding standard, execution resourcing |
| `ARCHITECTURE.md` | 3 | R1 architecture — structural commitments |
| `SPEC.md` | 4 | R1 spec — executable packaging of architecture |
| `TEST-PLAN.md` | 2 | Release-level test plan |
| `SCHEDULE.md` | 2 | Release schedule |
| `project-notes.md` | — | Per-project scratch pad |

## How to use

When the coordinator's work package delivery names a source artifact as an input, read it from `references/` in this skill directory. Do not read artifacts the work package does not name — they are not in scope for that iteration.

These are byte-for-byte copies. Do not modify them. If you find a contradiction between the source artifact and the work package specification, flag it — the coordinator routes it as an escalation.
