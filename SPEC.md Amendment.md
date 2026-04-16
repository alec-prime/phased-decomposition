# SPEC.md Amendment — Phase 4 Gate Closure

Amendment addresses five gaps identified at Phase 4 gate check:

1. Reference file content specifications (7 files)
2. Unpackaged architectural commitments
3. Work package file schema
4. Theory-of-mind voice as explicit producer constraint
5. Status revert to `draft`

Placement notes precede each insertion.

---

## Insertion 1 — New section: § Reference File Specifications

**Placement:** After § Asset Template Specifications, before § Execution Configuration.

**Rationale:** References and assets are the two resource categories in the shipped skill. Spec treats them in parallel.

---

## Reference File Specifications

The skill ships seven phase reference files in `references/`. Each is read by skill-execution-time Claude when a phase is invoked — loaded into context on demand per the progressive disclosure contract. Phase 6's producer authors each file against the must-address lists below.

All seven reference files share baseline requirements:

**Baseline — must address:**

- Phase purpose: what this phase commits and what it does not
- Terminal artifacts: what the phase produces and what closes it
- Which universal roles run in this phase, in what order, per § Per-Phase Role Application
- Operating procedure: how Claude runs the phase from session start to terminal artifact
- Phase closes when: observable conditions that flip status from draft to approved
- Handoff to the next phase: what the downstream phase inherits

**Baseline — must not prescribe:**

- Content of upstream artifacts (already committed in their respective phases)
- Content of downstream phases (belongs in downstream reference files)
- Executor-specific or release-specific detail (belongs in `CLAUDE.md` and `.claude/agents/`)

Per-file specifications below commit the additional content each file must address beyond the baseline.

### `phase-0-concept-roadmap.md`

**Must address:**

- Kernel-surfacing protocol per § Interrogator, including all three moves (surface framing check, why-does-it-matter push, compression check) and the recognition criterion
- The section-sweep pattern: interrogator runs the kernel check first, then sweeps remaining concept sections against what the operator has grounded
- Ungrounded-section handling: sections the operator did not ground are stubbed, not invented
- Scribe register for concept authoring: science-communicator voice per § Scribe, with failure examples drawn from § Scribe
- Concept template field rules per § Asset Template Specifications → `concept-template.md`
- Roadmap as Phase 0 sibling: roadmap commits release sequence and points at release artifacts for activation; does not activate releases

**Must not prescribe:**

- What the kernel of any specific project is — the kernel is operator-surfaced
- Release activation detail — belongs to Phase 2

**Source:** `SPEC.md § Interrogator`, `SPEC.md § Scribe`, `SPEC.md § Asset Template Specifications` → `concept-template.md` and `roadmap-template.md`.

### `phase-1-northstar-mockup.md`

**Must address:**

- Abstraction-vs-instantiation split: northstar at abstraction level (no project names, operator names, or filenames in scene bodies); mockup as one concrete instantiation
- Medium-follows-surface rule: mockup medium follows the project's external surface (visual products get visual mockups; scripted experiences get scripts; document-and-reading frameworks get narrated prose)
- Scene-to-scene correspondence: one mockup scene per northstar scene, same order
- Re-instantiability test: scenes survive substitution of project, operator, filenames, tools
- Descent from concept: northstar extends a specific concept commitment into phenomenological territory; each scene names which concept claim it pays off

**Must not prescribe:**

- Scene count — follows from what the concept commits to
- Scene content shape beyond the required subsections in `mockup-template.md`

**Source:** `SPEC.md § Asset Template Specifications` → `northstar-template.md` and `mockup-template.md`.

### `phase-2-release-activation.md`

**Must address:**

- Two jobs of Phase 2: mapping the release sequence (roadmap) and activating the current release (release artifact plus siblings)
- Release artifact as standalone product framing, not an increment
- Binding quality standard as input to Phase 3: each constraint passes forward as a structural resolution requirement
- Release-level test plan as operator-observable outcomes, not mechanism checks
- Release schedule as target, not contract; schedule moves when standard pressures it
- Executor roster: release commits names and roles; full definitions live in `.claude/agents/` files authored using `subagent-template.md`; session-start instructions live in `CLAUDE.md` authored using `claude-md-template.md`
- Locus rule: instructions governing skill-execution-time Claude do not belong in `CLAUDE.md` or subagent definitions; those govern build-time executors for this release

**Must not prescribe:**

- Release count — follows from roadmap
- Specific executor roster — follows from what the release requires
- Test plan row content — follows from the release's definition of done

**Source:** `SPEC.md § Asset Template Specifications` → `release-template.md`, `test-plan-template.md`, `claude-md-template.md`, `subagent-template.md`.

### `phase-3-architecture.md`

**Must address:**

- Phase 3 purpose: stress-test the release against its binding quality standard, resolve structural questions, commit to an approach
- Inward-facing register: architecture artifacts are consumed by build-time Claude, not operators; structured fields and tables where substance allows; explanatory prose only where it calibrates judgment
- Interrogator in Phase 3: translates architectural gaps into operator-register questions per § Per-Phase Role Application; never asks the operator in architecture vocabulary
- Descent from release: every binding constraint maps to a specific architectural commitment, auditable by section
- Open questions format: question → resolution mechanism naming which phase closes it
- Out-of-scope section: what the architecture explicitly does not decide, with the downstream phase that decides each thing named

**Must not prescribe:**

- What the structural commitments are — release-specific
- Directory structures, topology rules, tier targeting — release-specific

**Source:** `SPEC.md § Asset Template Specifications` → `architecture-template.md`, `SPEC.md § Per-Phase Role Application`.

### `phase-4-spec.md`

**Must address:**

- Phase 4 purpose: package architecture into executable form; commit field-level detail producers need to author correctly
- Inward-facing register per Phase 3
- Descent from architecture: every architectural commitment maps to a spec entry; no spec content originates outside architecture
- Asset and reference coverage: spec commits specifications for every file the release's architecture committed to the skill directory
- Execution Configuration section: runtime inputs the coordinator confirms at session start; fields with no default are blocking
- Spec-level test plan descending from release-level test plan rows
- Phase 5 terminal artifact structure specification: spec commits schema and field rules sufficient for the calibrator to size work packages

**Must not prescribe:**

- What the assets and references are — release-specific
- Section structure beyond the four-part rhythm (opener, substance, bounds-and-gaps, terminal)

**Source:** `SPEC.md § Asset Template Specifications` → `spec-template.md`, `SPEC.md § Execution Configuration`, `SPEC.md § Phase 5 Terminal Artifact Structure`.

### `phase-5-execution-planning.md`

**Must address:**

- Phase 5 purpose: decompose spec into work packages sized for specific executors; author the eval suite the Phase 6 supervisor gates against
- Calibrator role in Phase 5 per § Calibrator: sizes against executor definitions committed in `CLAUDE.md` and `.claude/agents/`; does not invent executor context
- Dual-artifact gate: `work-instructions.md` (operator-facing) and `execution-plan.json` (machine-readable) reviewed together; approval triggers Phase 6
- Eval suite as Phase 5 terminal artifact, authored against § Role Definitions failure examples and stored at `evals/evals.json` during build
- Work package file authoring: each package gets a file at `work-packages/<id>.md` per § Work Package File Schema
- Todoist load operation: triggered after gate approval; human work packages become Todoist tasks

**Must not prescribe:**

- Work package count — follows from spec decomposition
- Specific eval thresholds — committed against the optimized description

**Source:** `SPEC.md § Calibrator`, `SPEC.md § Phase 5 Terminal Artifact Structure`, `SPEC.md § Work Package File Schema`.

### `phase-6-execution.md`

**Must address:**
- Phase 6 purpose: coordinator runs the producer↔supervisor loop against Phase 5's artifacts; no planning or derivation happens here
- Supervisor-as-interrogator pattern: supervisor holds the work-package test plan as its standard and gates each producer iteration against it per § Per-Phase Role Application
- Verdict routing: advance / rework / escalate per § Build Loop Mechanics
- Escalation routing: failure characterization maps to upstream phase per § Escalation Routing
- Session walls: operator-set at session start per § Session Walls; coordinator does not spawn until confirmed
- Release-level UAT boundary: release-level UAT runs outside the producer/supervisor loop and is not decomposed into work packages; if anything Phase 5 hands to the coordinator looks like UAT work, that is a handoff error — coordinator flags to operator rather than spawning
- Operator touchpoints: session start, wall hits, escalations; no per-iteration review

**Must not prescribe:**
- Specific iteration bounds — operator-set per session
- Loop adaptation mid-execution — adaptation is an escalation, not a runtime decision

**Source:** `SPEC.md § Build Loop Mechanics`, `SPEC.md § Escalation Routing`, `SPEC.md § Session Walls`, `CLAUDE.md` as the canonical coordinator instance for R1.

---

## Insertion 2 — New section: § Build Loop Mechanics

**Placement:** After § Reference File Specifications, before § Execution Configuration.

**Rationale:** Packages architecture's § Build loop into spec form. The reference file spec for `phase-6-execution.md` points at this section.

---

## Build Loop Mechanics

The Phase 6 loop executes per-unit work Phase 5 hands over. Unit shape (queue, tree, batch) is committed by Phase 5; the loop mechanics are constant across shapes.

**Loop structure:**

```
coordinator delivers unit → producer drafts/refines → supervisor evaluates → verdict
```

**Verdicts — three options, coordinator routes per:**

|Verdict|Coordinator action|
|---|---|
|`advance`|Mark unit closed, move on per Phase 5's structure|
|`rework`|Respawn producer with supervisor's failure characterization as input; increment iteration count|
|`escalate`|Halt work on the unit, package for operator per § Escalation Routing|

**Eval execution during loop:**

|Eval category|Run cadence|
|---|---|
|Mechanical checks|Every iteration|
|Held-out trigger-rate checks|When description or routing changes|

Held-out cases gate `advance`. Threshold committed in Phase 5 against the optimized description.

**Termination — two conditions required for Phase 6 to close:**

1. Mechanical evals pass at Phase 5's threshold on held-out cases
2. Release-level test plan closes through operator judgment per `[[TEST-PLAN]]`

**Coordinator constraints:**

- Does not interpret verdicts
- Does not override supervisor judgments
- Does not spawn against dogfooding work — handoff errors flag to operator
- Does not adapt loop mid-execution — adaptation is an escalation

---

## Insertion 3 — New section: § Escalation Routing

**Placement:** After § Build Loop Mechanics.

**Rationale:** Packages architecture's § Escalation routing.

---

## Escalation Routing

When the supervisor returns `escalate`, the coordinator halts and packages the escalation for the operator.

**Escalation conditions — supervisor escalates when any hold:**

- Iteration bound hit (operator-set per session)
- Characterized failure recurs without progress toward resolution
- Standard itself under-resolved — supervisor cannot form verdict without inventing criteria
- Producer output satisfies standard, but running it reveals an upstream artifact gap

**Upstream phase routing — supervisor names the candidate:**

|Failure characterization|Upstream phase|
|---|---|
|Eval or decomposition was wrong|Phase 5|
|Spec was wrong|Phase 4|
|Architecture was wrong|Phase 3|
|Binding constraint in release artifact was wrong|Phase 2|

**Escalation package contents:**

- Supervisor's failure characterization
- Candidate upstream phase
- Unit of work that triggered the escalation
- Iteration count at escalation

**Operator action on escalation:**

Operator decides which artifact gets amended and whether work resumes. Amendment produces a changelog entry on the amended artifact naming the trigger and affected downstream artifacts. Downstream artifacts re-derive from the amendment.

Escalation is the framework working as designed — a build-time signal surfacing that an upstream artifact needs amendment, routed to the only role empowered to amend it.

---

## Insertion 4 — New section: § Session Walls

**Placement:** After § Escalation Routing.

**Rationale:** Packages architecture's § Session walls.

---

## Session Walls

Walls bound autonomous coordinator operation. Operator sets walls at session start; coordinator does not spawn until walls are confirmed.

**Minimum required walls:**

|Wall|Purpose|
|---|---|
|Iteration bound per unit of work|Catches runaway loops independent of failure shape|
|Session length bound|Returns to operator for review before session runs indefinitely|
|Escalation mode|`halt-on-escalation` (stop, surface, wait) or `queue-and-continue` (park escalated unit, continue, surface at session end); default `halt-on-escalation`|

**Additional walls are operator-discretionary per session.**

**Wall confirmation:**

- Confirmed explicitly at session start
- Prior-session values are not assumed — every session starts with fresh wall confirmation
- Wall hits trigger operator touchpoint: coordinator reports state (closed, pending, escalated) and waits for direction

---

## Insertion 5 — New section: § Test Plan Layers

**Placement:** After § Session Walls.

**Rationale:** Packages architecture's § Test plan layers. The two-layer architecture is not currently spec-committed.

---

## Test Plan Layers

Two layers, parallel rather than parent-child.

|Layer|Owner|Gate|Mechanically checkable|Traces upward to|
|---|---|---|---|---|
|Release validation|`[[TEST-PLAN]]`|Closes the release through operator judgment|No|Release Definition of Done|
|Build-loop gating|Phase 5 eval suite|Closes individual iterations through supervisor verdict|Yes|This spec § Role Definitions failure examples|

**Layer independence:** Eval suite entries do not trace to `[[TEST-PLAN]]` rows. The two layers check different properties against different gates at different times.

**Spec-level test plan placement:** Between layers. Spec-level rows (§ Spec-Level Test Plan) check reference-file and asset content correctness. They gate Phase 4 closure, not release validation and not build-loop iteration.

**Eval suite categories — Phase 5 commits specific entries:**

|Category|What it checks|
|---|---|
|Description trigger rate|Held-out queries that should invoke the skill do; queries that should not, do not|
|Phase routing|Given a phase-mapped request, Claude reaches the correct phase reference file before producing|
|Reference-file discovery|Claude follows reference invocations from phase guides without user prompting|
|Interrogator-before-production|Claude runs the phase's interrogator before drafting any artifact content|
|Kernel-surfacing protocol|Given a Phase 0 cold start, Claude resists proposing a kernel candidate; failure modes from § Interrogator serve as negative test cases|

---

## Insertion 6 — New section: § Reference Topology and Directory Constraints

**Placement:** After § Test Plan Layers.

**Rationale:** Packages architecture's § Reference topology, § File categorization, and § Skill directory structural rules that the producer must honor.

---

## Reference Topology and Directory Constraints

**Topology — one level deep:**

Every file in `references/` is invoked directly from `SKILL.md`. No reference file points at another reference file as a continuation.

The constraint is on chains, not mentions. Phase guides may name role files, other phase files, or asset files as context without that constituting a chain. A chain exists when reading file A directs Claude to read file B as a continuation of A's instructions, requiring B to be loaded to complete A's work.

**Directory categorization:**

|Directory|Contents|Read pattern|Producer authors?|
|---|---|---|---|
|`references/`|Files Claude reads to learn how to do work|Loaded into context on phase invocation|Yes, fresh against source artifacts|
|`assets/`|Files Claude stamps into a project as operator substrate|Copied to project directory, never loaded as instructions|Yes, fresh against template specifications|

**Directory-scoping rule:** Templates that get stamped live in `assets/`. Instructions that get loaded live in `references/`. `subagent-template.md` and `claude-md-template.md` live in `assets/` because Phase 2 of a downstream project stamps them, not because they instruct Claude during the stamping project's runtime.

**Omitted directories:**

- `scripts/` — not shipped. Framework procedures are judgment-heavy. Phase 5 may add the directory if a script need surfaces
- `evals/` — not shipped in the distributable skill. Evals are build-time material living at project root during the build

---

## Insertion 7 — New section: § Model-Tier Targeting

**Placement:** After § Reference Topology and Directory Constraints.

**Rationale:** Packages architecture's § Model-tier targeting. Binding on skill consumption, not just architecture-level commitment.

---

## Model-Tier Targeting

Skill content is authored to serve the model tier running each phase. Tier assignment per phase:

|Phase|Tier|
|---|---|
|0, 1, 2, 5|Opus|
|3, 4|Sonnet|
|6|Per-release in `CLAUDE.md`|

**Producer implication:** Reference-file voice calibrates to the tier running the phase. Opus-targeted reference files may assume more inference; Sonnet-targeted reference files commit more explicit structure. Phase 6 tier follows from the release's subagent definitions — the producer authors `phase-6-execution.md` at a tier-agnostic register and lets per-release `CLAUDE.md` commit the specific tiers.

---

## Insertion 8 — New section: § Voice Constraint on Producer Output

**Placement:** After § Model-Tier Targeting.

**Rationale:** Theory-of-mind voice is currently implicit in scribe role notes and asset authoring instructions. `[[RELEASE]]` § Binding Quality Standard commits it explicitly; spec has not packaged it.

---

## Voice Constraint on Producer Output

Phase 6 producer authoring of reference-file content is bound to theory-of-mind voice: explain why reasoning matters rather than impose rules. Skill content generalizes to unanticipated edge cases when it teaches a consuming Claude to think, not when it scripts compliance.

**Binding scope:**

- `SKILL.md` body
- All files in `references/`
- Inward-facing asset authoring instructions (`architecture-template.md`, `spec-template.md`, `claude-md-template.md`, `subagent-template.md`) — these are read as instructions by downstream Claude instances

**Not bound to theory-of-mind voice:**

- Outward-facing asset templates (`concept-template.md`, `northstar-template.md`, `mockup-template.md`, `roadmap-template.md`, `release-template.md`, `test-plan-template.md`) — these are stamped, not read as instructions; their voice is set by what the downstream project's scribe renders into them
- Spec-level and release-level test plan rows — operator-observable conditions, not reasoning guidance

**Failure modes:**

- Imperative MUSTs in reference file bodies: "Claude MUST run the interrogator before production." Theory-of-mind alternative: "The interrogator runs before production because a draft descending from Claude's inference rather than operator intent reads plausibly but fails downstream when the operator hits the concept doc and doesn't recognize it."
- Rule enumeration without rationale: a numbered list of kernel-surfacing steps with no explanation of why each step matters. Consuming Claude treats the list as ritual and misses the first edge case that doesn't match the numbered form.
- Scripted dialogue for operator interactions: reference files that commit exact phrasing for interrogator questions. Consuming Claude reads phrasing as required and loses the operator-register translation the interrogator is supposed to perform.

**Source:** Anthropic's skill-authoring documentation; `[[RELEASE]]` § Binding Quality Standard.

---

## Insertion 9 — New section in § Phase 5 Terminal Artifact Structure: § Work Package File Schema

**Placement:** Inside § Phase 5 Terminal Artifact Structure, after the `execution-plan.json` subsection and before the Todoist load operation subsection.

**Rationale:** `execution-plan.json` points at `work-packages/<id>.md` files; the spec does not commit what those files contain.

---

### Work Package Files

Each work package gets a file at `work-packages/<id>.md` at the project root. Phase 5 authors these alongside `execution-plan.json`. Machine packages are read by the producer at iteration start; human packages are referenced by Todoist tasks and read by the operator.

**Frontmatter:**

```yaml
artifact: work-package
phase: 5
project: "[[<Project Name>]]"
release: <R1 | R2 | ...>
work_package_id: <WP-NN-NN>
type: <machine | human>
executor: <subagent name | human>
iteration_bound: <n — machine only>
status: pending
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[SPEC]]"
```

**Body sections — required for all packages:**

| Section             | Guidance                                                                                                                                |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Work Package Scope  | One paragraph. What this package produces. Observable at closure.                                                                       |
| Inputs              | Upstream artifacts the executor consults. Files, sections, specific upstream commitments.                                               |
| Acceptance Criteria | Observable conditions that satisfy the package. Each criterion traces to a test plan row by id.                                         |
| Test Plan Entries   | Table with schema `id \| check \| traces_to \| status`. Rows are the specific checks the supervisor (machine) or operator (human) runs. |

**Additional sections — machine packages only:**

|Section|Guidance|
|---|---|
|Supervisor Standard|The standard the supervisor gates iterations against. Descends from Acceptance Criteria; makes the gating explicit.|
|Escalation Conditions|When the supervisor escalates instead of returning rework. Draws from § Escalation Routing; names specific conditions for this package.|

**Additional sections — human packages only:**

|Section|Guidance|
|---|---|
|Verification Steps|Ordered steps the operator performs to confirm the package closed. Each step observable.|

**Field rules:**

- Every acceptance criterion traces to a test plan row
- Machine packages commit iteration bound in frontmatter; human packages omit the field
- Work package files are read-only to the producer; status flips are written to `execution-plan.json`, not to the file
- Dependencies between packages are committed in `execution-plan.json`, not duplicated in work package files

---

## Insertion 10 — Amendment to § Phase 4 Closes When

**Placement:** Replace existing § Phase 4 Closes When.

**Rationale:** Conditions list expands to cover new spec sections.

---

## Phase 4 Closes When

- Every architectural commitment in `[[ARCHITECTURE]]` maps to a spec entry. Auditable: a reader checking each architecture section finds the spec section that packages it.
- Reference file specifications cover every file in `[[ARCHITECTURE]]` § Skill directory → `references/`.
- Asset template specifications cover every file in `[[ARCHITECTURE]]` § Skill directory → `assets/`.
- Execution Configuration section is complete with all runtime inputs enumerated.
- Phase 5 terminal artifact structure is committed with schema and field rules sufficient for the calibrator to size work packages against it, including work package file schema.
- Build loop mechanics, escalation routing, session walls, test plan layers, reference topology, model-tier targeting, and voice constraints are each packaged in their own sections.
- Status flips from `draft` to `approved` after operator review.

Phase 5 begins on approval. First Phase 5 work: eval set authoring against § Role Definitions failure examples, then decomposition against § Phase 5 Terminal Artifact Structure.

---

## Insertion 11 — Frontmatter

**Placement:** Spec frontmatter.

**Change:** `status: approved` → `status: draft` until gate re-runs.

---

## Insertion 12 — Changelog entry

**Placement:** Append to § Changelog.

---

- **2026-04-15 — Phase 4 gate closure amendment.** Added seven new top-level sections packaging architectural commitments that had not previously been spec-committed: § Reference File Specifications (covers all seven phase reference files with must-address / must-not-prescribe / source per file), § Build Loop Mechanics, § Escalation Routing, § Session Walls, § Test Plan Layers, § Reference Topology and Directory Constraints, § Model-Tier Targeting, § Voice Constraint on Producer Output. Added § Work Package File Schema as a subsection of § Phase 5 Terminal Artifact Structure committing frontmatter, body sections, and field rules for `work-packages/<id>.md` files. Expanded § Phase 4 Closes When to cover the new sections. Status reverted from `approved` to `draft` pending operator review of the amendment. _Why:_ Phase 4 gate check found four architectural commitments unpackaged (reference topology, model-tier targeting, escalation routing, session walls, dogfooding boundary — last one handled via `phase-6-execution.md` reference spec rather than its own section), four partially packaged (file categorization, build loop, test plan layers, voice constraint), zero reference file content specifications, and no work package file schema. The gate required all gaps closed before Phase 5. _Affects:_ Reference file authoring in Phase 6 (now has explicit must-address lists per file), Phase 5 decomposition (now has work package file schema to decompose against), spec-level test plan rows S1-TP-01 through S1-TP-08 (now have content specifications to check against).