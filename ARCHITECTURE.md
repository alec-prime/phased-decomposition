---
artifact: architecture
phase: 3
project: "[[Phased Decomposition]]"
release: R1
status: approved
created: 2026-04-14
framework_version: 0.1.0
descends_from:
  - "[[RELEASE]]"
---
# Architecture — Phased Decomposition R1

## Skill directory

```
phased-decomposition/
├── SKILL.md
├── references/
│   ├── phase-0-concept-roadmap.md
│   ├── phase-1-northstar-mockup.md
│   ├── phase-2-release-activation.md
│   ├── phase-3-architecture.md
│   ├── phase-4-spec.md
│   ├── phase-5-execution-planning.md
│   └── phase-6-execution.md
└── assets/
    ├── concept-template.md
    ├── northstar-template.md
    ├── mockup-template.md
    ├── roadmap-template.md
    ├── release-template.md
    ├── architecture-template.md
    ├── spec-template.md
    ├── claude-md-template.md
    ├── subagent-template.md
    └── test-plan-template.md
```

No `scripts/`. The framework's procedures are judgment-heavy with no deterministic operations that benefit from script execution. Phase 5 may add the directory if a script need surfaces.

No `evals/` in the shipped skill. Evals are build-time material — the supervisor runs them against producer output during Phase 6 to gate iteration. They live at the project root in `evals/evals.json` during the build, not in the distributable skill.

## File categorization

| Directory | Contents | Read pattern |
|---|---|---|
| `references/` | Files Claude reads to learn how to do work | Loaded into context on phase invocation |
| `assets/` | Files Claude stamps into a project as operator substrate | Copied to project directory, never loaded as instructions |

Phase templates live in `assets/` because they are stamped, not loaded. Phase guides and agent roles live in `references/` because they instruct Claude on how to run the phase or occupy the role. `subagent-template.md` and `claude-md-template.md` live in `assets/` because Phase 2 of a downstream project stamps them into `.claude/agents/` and the project root.

## Reference topology

One level deep from `SKILL.md`. Every file in `references/` is invoked directly from the routing section of `SKILL.md`. No reference file points at another reference file as a continuation.

The constraint is on chains, not mentions. Phase guides may name role files as the role each phase activates without that constituting a chain. A chain is when reading file A directs Claude to read file B as a continuation of A's instructions, requiring B to be loaded to complete A's work.

## Skill name

`building-projects-with-claude`. Gerund form per Anthropic convention.

## Skill description

Authored in Phase 4 against the eval suite's trigger-rate optimization. Constraints binding on Phase 4:

- Third person
- Names what the skill does and when to use it
- Phrasing biased toward triggering — counter undertriggering, not overtriggering
- Under 1024 characters

The description is the mechanism by which the skill loads. Phase 5 authors matching eval entries; Phase 6 runs the optimization loop against them.

## Model-tier targeting

| Phase | Tier |
|---|---|
| 0, 1, 2, 5 | Opus |
| 3, 4 | Sonnet |
| 6 | Per-release in `CLAUDE.md` |

Tier follows where mistakes hide. Phases 0, 1, 2, and 5 originate commitments downstream phases consume — wrong calls there rot downstream artifacts before the loop can catch them. Phases 3 and 4 resolve commitments against upstream structure; mistakes show up immediately as broken downstream work.

## Universal roles in the skill

Three universal roles: interrogator, scribe, calibrator.

**Role definitions in SKILL.md body, not in `references/`.** Standards run across phases 0–5 and need to be in Claude's context from the moment the skill triggers, not loaded on-demand.

The body must hold them to the operating-question + failure-examples shape committed in `project-notes.md`.

**Phase guides invoke roles by name, not by definition.** Each phase reference file names which roles run during that phase and what they do in that phase's specific context. The phase guide assumes SKILL.md established what each role is; the phase guide commits where each role applies and what its output for that phase looks like.

**Calibrator runtime references.** The calibrator consults the per-release `CLAUDE.md` and `.claude/agents/` files for executor context. No framework-level profiles.

## Per-release CLAUDE.md

The skill ships `assets/claude-md-template.md` and authoring guidance in `references/phase-2-release-activation.md`. A downstream project's `CLAUDE.md` is authored in that project's Phase 2, naming the specific executors that release will use.

| Scope | Source | Captures |
|---|---|---|
| Framework | Calibrator role definition | Executor types in general |
| Release | Per-release CLAUDE.md | Specific executors for this release |

## Build loop

```
coordinator delivers unit → producer drafts/refines → supervisor evaluates → verdict
```
**Verdicts.** Three:

- `advance` — iteration satisfies the standard; coordinator moves on per Phase 5's structure
- `rework` — failure characterized; coordinator respawns producer with characterization as input
- `escalate` — bound hit, characterized failure recurs without progress, standard under-resolved, or upstream gap surfaced; coordinator halts and routes to operator

**Eval split.**

- _Mechanical checks_ — always run per iteration
- _Held-out trigger-rate checks_ — run when description or routing changes

Held-out cases gate advance. Threshold set in Phase 5 against the optimized description.

**Unit shape.** Whatever Phase 5 hands over determines what counts as a unit of work and how units relate.

**Eval suite location and lifecycle.**

| Concern | Location |
|---|---|
| Authoring | Phase 5 terminal artifact |
| Storage during build | `evals/evals.json` at project root |
| Consumed by | Supervisor during Phase 6 |
| Shipped to skill-execution-time Claude | No |
| Versioning | With project repo, not with skill distribution |

**Termination.** Two conditions, both required:

1. Mechanical evals pass at Phase 5's threshold on held-out cases
2. Dogfooding gate (`[[TEST-PLAN]]` rows R1-TP-01 through R1-TP-11) closes through operator judgment on March and Nell runs

## Escalation routing

When the supervisor returns `escalate`, the coordinator halts and packages per `CLAUDE.md` § Escalation handling.

| Failure characterization | Upstream phase |
|---|---|
| Eval was wrong | Phase 5 |
| Spec was wrong | Phase 4 |
| Architecture was wrong | Phase 3 |
| Binding constraint in `[[RELEASE]]` was wrong | Phase 2 |

Each escalation produces an amendment to the named artifact and re-derivation of everything downstream. Amendments carry a changelog entry naming the trigger and the affected downstream artifacts.

## Session walls

Bound autonomous coordinator operation. Set per session by the operator at session start per `CLAUDE.md` § Session walls.

## Test plan layers

Two layers, parallel rather than parent-child.

| Layer | Owner | Gate | Mechanically checkable? |
|---|---|---|---|
| Release validation | `[[TEST-PLAN]]` | Closes the release | No |
| Build-loop gating | Phase 5 eval suite | Closes individual iterations | Yes |

Eval suite entries do not have `traces_to` pointing at `[[TEST-PLAN]]` rows.

**Eval categories.** Phase 5 commits specific entries. Categories:

- _Description trigger rate_ — held-out queries that should invoke the skill do; queries that should not, do not
- _Phase routing_ — given a phase-mapped request, Claude reaches the correct phase reference file before producing
- _Reference-file discovery_ — Claude follows reference invocations from phase guides without user prompting
- _Interrogator-before-production_ — phases 0–5 only; Claude runs the phase's interrogator before drafting any artifact content
- _Kernel-surfacing protocol_ — given a Phase 0 cold start, Claude resists proposing a kernel candidate; failure modes from `project-notes.md` § _Phase 0 — the kernel_ serve as negative test cases

**Downward decomposition.**

| Level | Covers | Traces to |
|---|---|---|
| Phase 4 spec-level | Individual reference-file content correctness | This document § Skill directory, § Universal roles in the skill |
| Phase 5 task-level | Individual draft-iteration outputs against eval criteria | Spec-level entries |
| Phase 6 work-package-level | Individual producer outputs against supervisor gating | Task-level entries |

## Voice constraint on skill content

Phase 6 producer authoring of skill content is bound to Anthropic's skill-authoring methodology per `[[RELEASE]]` § Binding Quality Standard. The producer consults `skill-creator` as its authoring authority. Binding constraint from `[[RELEASE]]`: theory-of-mind voice over imperative MUSTs — skill content explains why reasoning matters rather than imposing rules, so the skill generalizes to unanticipated edge cases.

## Out of scope

| Excluded                                                                                                                | Reason                                                                                           |
| ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Phase 4 design detail (interface contracts, schemas, file formats, eval suite schema, specific reference-file contents) | Architecture commits structures exist; Phase 4 commits what they look like                       |
| Phase 5 decomposition (work packages, sizing, ordering, eval thresholds)                                                | Architecture commits the loop runs and how it gates; Phase 5 commits what runs through it        |
| Re-litigation of Phase 2 commitments                                                                                    | Committed in `[[CONCEPT]]`, `[[ROADMAP]]`, `[[RELEASE]]`. Revisits go through upstream amendment |
| Memory mechanics                                                                                                        | Cut from R1 scope per `[[RELEASE]]` § Execution Resourcing                                       |

## Phase 3 closes when

- Every binding constraint from `[[RELEASE]]` § Binding Quality Standard maps to a specific commitment in this document.
- Open questions are logged with the phase that resolves each one named.
- Status flips from `draft` to `approved` after operator review.

Phase 4 begins on approval. First Phase 4 work: `spec.md` descending from § Skill directory and § Universal roles in the skill, and the spec-level test plan descending from § Test plan layers.

## Changelog

- **2026-04-15 — Author-facing rationale stripped; skill name rationale removed.** Removed explanatory prose serving author understanding rather than build-time Claude decision-making. Substance unchanged.

- **2026-04-14 — Calibrator profiles collapsed; skill name committed.** Both framework-level calibrator profiles removed from `references/`. Skill name committed as `building-projects-with-claude`.

- **2026-04-14 — Re-draft against inward-facing register.** Dropped orientation prose, compressed to structured fields and tables, retained explanatory prose only where it calibrates judgment the consumer Claude needs to make.

- **2026-04-14 — Rewrite from ground up against corrected Phase 2 cluster.** Descends from `[[RELEASE]]` only. Drops `CLAUDE.md` and release-level test plan as Phase 3 sub-artifacts, drops `evals/` from shipped skill directory.