---

artifact: architecture 
phase: 3 
project: "[[Phased Decomposition]]" 
release: R1 
status: draft 
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

No `evals/` in the shipped skill. Evals are build-time material — the supervisor runs them against producer output during Phase 6 to gate iteration. They live at the project root in `evals/evals.json` during the build, not in the distributable skill. Locus call per `project-notes.md` § _Authoring for R1 executors passes a locus test_: evals govern build-time Claude.

## File categorization

|Directory|Contents|Read pattern|
|---|---|---|
|`references/`|Files Claude reads to learn how to do work|Loaded into context on phase invocation|
|`assets/`|Files Claude stamps into a project as operator substrate|Copied to project directory, never loaded as instructions|

Phase templates live in `assets/` because they are stamped, not loaded. Phase guides and calibrator profiles live in `references/` because they instruct Claude on how to run the phase or occupy the role. `subagent-template.md` and `claude-md-template.md` live in `assets/` because Phase 2 of a downstream project stamps them into `.claude/agents/` and the project root.

## Reference topology

One level deep from `SKILL.md`. Every file in `references/` is invoked directly from the routing section of `SKILL.md`. No reference file points at another reference file as a continuation.

The constraint is on chains, not mentions. Phase guides may name role files as the role each phase activates ("consult the calibrator profile for {executor type}") without that constituting a chain. A chain is when reading file A directs Claude to read file B as a continuation of A's instructions, requiring B to be loaded to complete A's work.

## Skill name

`building-projects-with-claude`. Gerund form per Anthropic convention. Names the activity in user vocabulary (a user thinking "I want to build something with Claude" is the user the skill targets) and signals the LLM-as-collaborator novelty from `[[CONCEPT]]` § Thesis.

The framework name "phased decomposition" remains the project name and the framework's internal vocabulary. Skill name and framework name diverge deliberately — framework name describes what the framework _is_, skill name describes what the skill _does for a user_.

The description carries disambiguation load: signals project-shaped work with structure, distinguishes from general Claude usage and from lighter-weight project planning. Phase 4 authors the description against the eval suite's trigger-rate optimization.

## Model-tier targeting

|Phase|Tier|
|---|---|
|0, 1, 2, 5|Opus|
|3, 4|Sonnet|
|6|Per-release in `CLAUDE.md`|

Tier follows where mistakes hide. Phases 0, 1, 2, and 5 originate commitments downstream phases consume. Wrong calls there read fine on the day made and rot downstream artifacts that descend from them — the loop cannot catch the mistake. Opus is worth the cost.

Phases 3 and 4 resolve commitments against upstream structure. Architecture resolves against release; spec resolves against architecture. Mistakes show up immediately as broken downstream work. Sonnet handles them reliably.

## Universal roles in the skill

Three universal roles per `project-notes.md` § _Framework roles_: interrogator, scribe, calibrator. Skill realizations:

**Role definitions in SKILL.md body, not in `references/`.** The standards are universal across phases and need to be in Claude's context from the moment the skill triggers, not loaded on-demand. SKILL.md body absorbs them alongside the phase pipeline overview and routing table.

Role definitions are authoring-time material for SKILL.md itself — they shape what SKILL.md says, and once SKILL.md is written they have done their job. They are not consulted independently at runtime. They embody in the skill's instructions from session start.

The body must hold them to the operating-question + failure-examples shape committed in `project-notes.md`. Expanding into prose treatises blows the 500-line target.

**Phase guides invoke roles by name, not by definition.** Each phase reference file names which roles run during that phase and what they do in that phase's specific context. Example: Phase 0's guide names that the interrogator runs first against kernel surfacing (per `project-notes.md` § _Phase 0 — the kernel_), then against the section sweep, with the kernel entry's failure modes as negative test cases. Phase 5's guide names that the interrogator runs first, the calibrator runs during decomposition, and the scribe runs against the dual-artifact output.

The phase guide assumes SKILL.md established what each role _is_. The phase guide commits where each role _applies_ and what its output for that phase looks like.

**Calibrator runtime references.** None at framework scope. Per-executor context lives in the per-release `CLAUDE.md` and subagent definitions authored in each downstream project's Phase 2. Phase 5 consults those for work-package sizing, not framework-shipped profiles.

The earlier draft committed framework-level calibrator profiles for coding-agent and human-operator executor types. Both collapsed: per-release `CLAUDE.md` and subagent definitions already carry executor-specific context at the scope where it varies. Framework-level profiles would have double-counted that material. The calibrator role definition in SKILL.md names that the calibrator consults the per-release `CLAUDE.md` and `.claude/agents/` files for executor context.

## Per-release CLAUDE.md

The skill ships `assets/claude-md-template.md` and authoring guidance in `references/phase-2-release-activation.md`. A downstream project's `CLAUDE.md` is authored in that project's Phase 2, naming the specific executors that release will use.

Two scopes of executor knowledge coexist without overlap:

|Scope|Source|Captures|
|---|---|---|
|Framework|Calibrator profiles|Executor types in general|
|Release|Per-release CLAUDE.md|Specific executors for this release|

R1's own `CLAUDE.md` at the project root is an instance of this pattern, authored in this project's Phase 2. Exempted from artifact frontmatter as an operational file (matches subagent definition convention). Not part of the shipped skill.

## Build loop

Executors already committed in `[[RELEASE]]` § Execution Resourcing. Architecture commits the structural mechanics they operate within.

```
coordinator delivers unit → producer drafts/refines → supervisor evaluates → verdict
```

**Verdicts.** Three. Per `.claude/agents/supervisor.md`:

- `advance` — iteration satisfies the standard; coordinator moves on per Phase 5's structure
- `rework` — failure characterized; coordinator respawns producer with characterization as input
- `escalate` — bound hit, characterized failure recurs without progress, standard under-resolved, or upstream gap surfaced; coordinator halts and routes to operator

**Eval split.**

- _Mechanical checks_ — always run per iteration
- _Held-out trigger-rate checks_ — run when description or routing changes

Held-out cases gate advance. The threshold is set in Phase 5 against the optimized description.

**Unit shape.** Whatever Phase 5 hands over (queue, tree, batch, other) determines what counts as a unit of work and how units relate. Architecture commits the loop runs per unit. Phase 5 commits what the units are.

**Eval suite location and lifecycle.**

|Concern|Location|
|---|---|
|Authoring|Phase 5 terminal artifact|
|Storage during build|`evals/evals.json` at project root|
|Consumed by|Supervisor during Phase 6|
|Shipped to skill-execution-time Claude|No|
|Versioning|With project repo, not with skill distribution|

**Termination.** Two conditions, both required, per `[[RELEASE]]` § Definition of Done as bound by `[[TEST-PLAN]]` § Test Plan Closure:

1. Mechanical evals pass at Phase 5's threshold on held-out cases
2. Dogfooding gate (`[[TEST-PLAN]]` rows R1-TP-01 through R1-TP-11) closes through operator judgment on March and Nell runs

Either alone is insufficient. Mechanical pass without dogfooding ships a skill that triggers correctly but produces poor artifacts. Dogfooding pass without mechanical evals ships a skill that worked for the operator on the day they tested but degrades on inputs they did not try.

## Escalation routing

When the supervisor returns `escalate`, the coordinator halts and packages per `CLAUDE.md` § Escalation handling. The operator decides which upstream phase the failure traces to.

|Failure characterization|Upstream phase|
|---|---|
|Eval was wrong|Phase 5|
|Spec was wrong|Phase 4|
|Architecture was wrong|Phase 3|
|Binding constraint in `[[RELEASE]]` was wrong|Phase 2|

Each escalation produces an amendment to the named artifact and re-derivation of everything downstream. Amendments carry a changelog entry naming the trigger and the affected downstream artifacts. Quiet edits are not permitted.

## Session walls

Bound autonomous coordinator operation. Set per session by the operator at session start (`CLAUDE.md` § Session walls). Architecture commits walls exist and gate the loop. Specific walls are operator decisions per session, not architectural commitments.

## Dogfooding boundary

March and Nell runs are outside the producer/supervisor loop. They are operator-direct against the shipped skill, judged against `[[MOCKUP]]` Scene 1 as the reference standard per `[[TEST-PLAN]]`.

If anything Phase 5 hands to the coordinator looks like dogfooding work, that is a handoff error. The coordinator flags to the operator rather than spawning against it.

## Test plan layers

Two layers, parallel rather than parent-child.

|Layer|Owner|Gate|Mechanically checkable?|
|---|---|---|---|
|Release validation|`[[TEST-PLAN]]`|Closes the release|No (per `[[RELEASE]]` § Binding Quality Standard — content quality is not mechanically checkable)|
|Build-loop gating|Phase 5 eval suite|Closes individual iterations|Yes|

Eval suite entries do not have `traces_to` pointing at `[[TEST-PLAN]]` rows. They check different things at different times for different audiences. Release validation is what the operator does _with_ the shipped skill. Build-loop gating is what the supervisor does during Phase 6.

**Eval categories.** Phase 5 commits specific entries. Categories:

- _Description trigger rate_ — held-out queries that should invoke the skill do; queries that should not, do not
- _Phase routing_ — given a phase-mapped request, Claude reaches the correct phase reference file before producing
- _Reference-file discovery_ — Claude follows reference invocations from phase guides without user prompting
- _Interrogator-before-production_ — Claude runs the phase's interrogator before drafting any artifact content
- _Kernel-surfacing protocol_ — given a Phase 0 cold start, Claude resists proposing a kernel candidate; failure modes from `project-notes.md` § _Phase 0 — the kernel_ serve as negative test cases

**Downward decomposition.**

|Level|Covers|Traces to|
|---|---|---|
|Phase 4 spec-level|Individual reference-file content correctness|This document § Skill directory, § Universal roles in the skill|
|Phase 5 task-level|Individual draft-iteration outputs against eval criteria|Spec-level entries|
|Phase 6 work-package-level|Individual producer outputs against supervisor gating|Task-level entries|

A test that cannot decompose downward signals that the artifact at the level above is under-resolved. The chain does not extend upward into `[[TEST-PLAN]]` — release validation checks something different (operator judgment of dogfooding artifact quality) than the build loop checks.

## Voice constraint on skill content

Phase 6 producer authoring of skill content (SKILL.md, references, assets) is bound to the scribe voice standard per `project-notes.md` § _Framework roles_ → Scribe: demonstrative rather than persuasive, theory-of-mind explanations of why rather than imperative MUSTs, science-communicator register.

Anthropic's skill-authoring guidance (`[[RELEASE]]` § Binding Quality Standard) aligns with the scribe standard and reinforces it. The constraint is not new to the skill.

This applies to skill content, not to architecture or downstream Phase 3–6 artifacts. Those follow inward-facing register per `project-notes.md` § _Outward-facing vs inward-facing artifacts_.

## Out of scope

|Excluded|Reason|
|---|---|
|Phase 4 design detail (interface contracts, schemas, file formats, eval suite schema, specific reference-file contents)|Architecture commits structures exist; Phase 4 commits what they look like|
|Phase 5 decomposition (work packages, sizing, ordering, eval thresholds)|Architecture commits the loop runs and how it gates; Phase 5 commits what runs through it|
|Re-litigation of Phase 2 commitments (R1 shape, Anthropic conformance, dogfooding gate, skill ship form, executor roster)|Committed in `[[CONCEPT]]`, `[[ROADMAP]]`, `[[RELEASE]]`. Real reasons to revisit go through upstream amendment, not silent absorption here|
|Memory mechanics|Cut from R1 scope per `[[RELEASE]]` § Execution Resourcing|

## Open questions

|Question|Resolution mechanism|
|---|---|
|Eval thresholds|Phase 5 commits numbers against held-out cases|
|`claude-md-template.md` content scope (skeleton vs worked example)|Phase 4 resolves against the trigger of authoring the first non-meta `CLAUDE.md`|
|Reference topology under stress (any guide outgrowing one file)|Phase 5 surfaces; resolution is split horizontally or accept subdirectory|
|Writing for two readers (Opus on Phases 0, 1, 2, 5; Sonnet on Phases 3, 4)|Phase 5 surfaces if conventions conflict; routes back to architecture|
|Phase 3's mixed failure-mode shape (resolving overall but framing-shaped on model-tier and role-architecture decisions)|Watched during dogfooding; if Phase 3 needs Opus for framing-shaped sub-decisions, route back to architecture|
|Inward-facing artifact shape|This document is the first instance; Phase 4 spec authoring against it surfaces what holds|

## Phase 3 closes when

- Every binding constraint from `[[RELEASE]]` § Binding Quality Standard maps to a specific commitment in this document. Mapping is auditable: a reader checking each `[[RELEASE]]` constraint can find the section and paragraph that resolves it.
- Open questions are logged with the phase that resolves each one named.
- Status flips from `draft` to `approved` after operator review.

Phase 4 begins on approval. First Phase 4 work: `spec.md` descending from § Skill directory and § Universal roles in the skill, and the spec-level test plan descending from § Test plan layers.

## Changelog

- **2026-04-14 — Calibrator profiles collapsed; skill name committed.** Both framework-level calibrator profiles (`calibrator-profile-coding-agent.md`, `calibrator-profile-human-operator.md`) removed from `references/`. Per-executor context already lives in per-release `CLAUDE.md` and subagent definitions at the scope where it varies; framework-level profiles double-counted that material. Calibrator role consults `CLAUDE.md` and `.claude/agents/` files for executor context at runtime. Skill name committed as `building-projects-with-claude` — gerund form, names the activity in user vocabulary, signals the LLM-as-collaborator novelty. _Affects:_ § Skill directory (two files removed), § Universal roles in the skill (calibrator runtime references subsection rewritten), § Skill name (committed), § Open questions (final-skill-name and calibrator-profile-relationship rows removed). _Downstream:_ `project-notes.md` § _Calibrator runtime references_ should expel; `project-notes.md` § _Calibrator's relationship to Phase 2 reference documentation_ resolves (no profiles to relate to references).
    
- **2026-04-14 — Re-draft against inward-facing register.** Prior re-draft (same date, earlier session) followed the four-part rhythm rule, which the inward/outward split graduated to project-notes scoped to outward-facing artifacts only. This draft drops the rhythm, drops orientation prose, drops narrative tracing language, compresses to structured fields and tables where substance allows, retains explanatory prose only where it calibrates judgment the consumer Claude needs to make. Substance unchanged from prior draft; structure rebuilt for build-time Claude consumption. Stale outward-facing draft retained out-of-tree as reference for diff check.
    
- **2026-04-14 — Rewrite from ground up against corrected Phase 2 cluster.** Prior `ARCHITECTURE.md` (2026-04-13 draft) authored before the structural correction that relocated `CLAUDE.md`, release-level test plan, and release schedule from Phase 3 to Phase 2. New draft descends from `[[RELEASE]]` only, narrows scope to pure structural resolution, drops `CLAUDE.md` and release-level test plan as Phase 3 sub-artifacts, drops `evals/` from the shipped skill directory (build-time material), restructures Test Plan section to clarify build loop evals are parallel to `[[TEST-PLAN]]` rather than a decomposition of them.