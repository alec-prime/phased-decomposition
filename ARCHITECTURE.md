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

## Shape of This Architecture

R1 ships the phased decomposition skill in the form Anthropic's skill-authoring documentation specifies. Phase 2 has committed what R1 ships, the binding quality standard it ships against, the executor roster that builds it, and the dogfooding gate that validates it. Phase 3's job is structural resolution: turning the binding constraints from `[[RELEASE]]` §Binding Quality Standard into specific commitments about what the built skill looks like as a directory of files, how its universal roles are realized in skill content, and how the build loop runs against the producer/supervisor/coordinator roster Phase 2 committed.

Every commitment below traces upward to a binding constraint in `[[RELEASE]]` or to a framework rule established in `[[CONCEPT]]` or `project-notes.md`. Phase 4 (spec, design, spec-level test plan) and Phase 5 (execution plan, eval set) are downstream of this document; this document deliberately stops short of their work.

## Skill Shape

The Anthropic-conformance constraint cluster from `[[RELEASE]]` §Binding Quality Standard resolves into a single structural commitment about what the built skill looks like as a directory.

**Directory layout.**

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
│   ├── phase-6-execution.md
│   ├── calibrator-profile-coding-agent.md
│   └── calibrator-profile-human-operator.md
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

No `scripts/` directory. The framework's procedures are judgment-heavy and have no deterministic operations that benefit from script execution. If a script need surfaces during Phase 5 authoring, the directory is added then; preemptively scaffolding it would commit to a category the skill does not use.

No `evals/` directory in the shipped skill. Evals are R1 build-time material — the supervisor runs them against producer output during Phase 6 to gate iteration. They do not ship to skill-execution-time Claude. They live at the project root during the build (`evals/evals.json`) and are not packaged into the skill's distributable form. This is a locus call: evals govern build-time Claude (per project-notes _Authoring for R1 executors passes a locus test_), so they belong with build infrastructure, not with the skill's own files.

**Reference topology.** One level deep from `SKILL.md`. Every file in `references/` is invoked directly from the routing section of `SKILL.md`; no reference file points at another reference file as a continuation. Phase guides may name role files as the role each phase activates, but the activation reads as "consult the calibrator profile for {executor type}" rather than as a chain that walks from one reference into the next. The chain-versus-mention distinction is what Anthropic's one-level-deep rule constrains.

**Categorization.** Files Claude reads to learn how to do the work go in `references/`. Files Claude stamps into a project as the operator's working substrate go in `assets/`. The phase templates live in `assets/` because their role is to be copied into a new project's directory and filled in, not loaded into Claude's context as instructions. The phase guides and calibrator profiles live in `references/` because they instruct Claude on how to run the phase or occupy the role; they are never copied anywhere.

`subagent-template.md` and `claude-md-template.md` live in `assets/` because Phase 2 of a downstream project stamps them into that project's `.claude/agents/` and project root respectively. They are operator-facing scaffolds, not Claude-facing instructions.

**Skill name.** Gerund form per Anthropic convention. Working candidate: `running-phased-decomposition`. Names the activity (running a project through the framework) rather than the framework as an object. Final commitment deferred to Phase 4 once description-trigger optimization runs against the eval suite — the name appears in the description and affects trigger rate. Alternates in scope: `decomposing-projects-by-phase`, `phasing-projects`, `structuring-projects-in-phases`. The candidate set is open; Phase 4 picks.

**Model-tier targeting.** `[[RELEASE]]` §Binding Quality Standard names model-tier targeting as a Phase 3 commitment because it affects voice calibration and content density throughout. R1 commits Opus for Phases 0, 1, 2, and 5; Sonnet for Phases 3 and 4. Phase 6's tier is set per release in `CLAUDE.md`, not here. The split tracks where mistakes hide. Phases 0, 1, 2, and 5 originate commitments downstream phases consume — concept articulation, illustration, release shape, decomposition and eval authoring — and a wrong call in any of them reads fine on the day it is made and rots downstream artifacts that descend from it. Opus is worth the cost where the loop cannot catch the mistake. Phases 3 and 4 resolve commitments against upstream structure (architecture against release, spec against architecture); their mistakes show up immediately as broken downstream work, and Sonnet handles them reliably.

**Voice constraints.** Phase 6 producer authoring is bound to the scribe voice standard committed in `project-notes.md` (§Graduated → _Framework roles_ → Scribe): demonstrative rather than persuasive, theory-of-mind explanations of why rather than imperative MUSTs, science-communicator register that brings a reader up to speed without requiring them to absorb a stance first. Anthropic's best-practices guidance on theory-of-mind voice over heavy-handed MUSTs (`[[RELEASE]]` §Binding Quality Standard) aligns with the scribe standard and reinforces it; the constraint is not new to the skill.

## Role Architecture

The framework commits three universal roles — interrogator, scribe, calibrator (`project-notes.md` §Graduated → _Framework roles_) — but defers to Phase 3 the question of how those roles realize in the built skill. This section commits the realization.

**Role definitions live in SKILL.md body, not in references.** The interrogator standard, the scribe voice standard, and the calibrator's operating question are universal across phases and need to be in Claude's context from the moment the skill triggers, not loaded on-demand mid-session. SKILL.md body absorbs them as part of the framework's universal context, alongside the phase pipeline overview and the routing table to phase guides.

Role definitions are authoring-time material for SKILL.md itself — they shape what SKILL.md says, and once SKILL.md is written they have done their job. They do not ship as separate files because they are not consulted independently at runtime; they are embodied in the skill's instructions from the start of every session. This keeps the body under Anthropic's 500-line target only if the role definitions are tight; Phase 5 authoring has to hold them to the operating-question + failure-examples shape committed in project-notes rather than expanding into prose treatises.

**Phase guides invoke roles by name, not by definition.** Each phase reference file in `references/` names which roles run during that phase and what they do in that phase's specific context. Phase 0's guide names that the interrogator runs first against kernel surfacing (per project-notes _Phase 0 — the kernel_), then against the section sweep, with the failure modes from the kernel framework-notes entry as negative test cases. Phase 5's guide names that the interrogator runs first, the calibrator runs during decomposition, and the scribe runs against the dual-artifact output. The phase guide assumes SKILL.md has established what each role _is_; the phase guide commits where each role _applies_ and what the role's output for that phase looks like.

**Calibrator runtime references.** The calibrator is the only role whose judgment depends on field-level material that varies across executors and moves over time (`project-notes.md` §Open → _Calibrator runtime references_). Its runtime references — per-executor profiles describing what right-sized work looks like for each executor type — live as sibling files in `references/`. Naming convention: `calibrator-profile-{executor-type}.md`. R1 ships with two profiles: `calibrator-profile-coding-agent.md` (drawing from Anthropic's agentic coding guidance and Claude Code task-decomposition patterns) and `calibrator-profile-human-operator.md` (drawing from operator self-knowledge about working patterns). New executor types add new files; profiles update independently of any role definition. The calibrator role definition in SKILL.md names that the calibrator consults profiles at runtime and where to find them.

The unresolved question from `project-notes.md` §Open → _Calibrator's relationship to Phase 2 reference documentation_ — whether `CLAUDE.md` alone carries enough substrate or whether the calibrator also needs the source documentation that informed `CLAUDE.md` — does not block Phase 3. Architecture commits the profile mechanism; R1 execution observes which the calibrator actually reaches for; whichever holds graduates per the project-notes graduation trigger.

**Why interrogator and scribe don't have profiles.** The interrogator's job — asking targeted questions in the operator's register to clear context gaps — depends on the operating question, which is universal, plus failure examples that calibrate judgment, which are also universal. The scribe's voice standard is similarly universal: the operating question (does this teach or work on the reader) and the failure examples carry across all artifacts. Neither role's judgment depends on material that varies by project or moves over time, so neither needs runtime references. If a future role surfaces with that kind of judgment dependency, it gets profiles; the calibrator remains the only one for now.

**Relationship to per-release `CLAUDE.md`.** The skill ships `assets/claude-md-template.md` and authoring guidance in `references/phase-2-release-activation.md`. A downstream project's `CLAUDE.md` is authored in that project's Phase 2, naming the specific executors that release will use. The skill knows about executor types in general (via calibrator profiles); each release knows about its specific executors (via its own `CLAUDE.md`). The two kinds of executor knowledge coexist without overlap because they live at different scopes — framework-level versus release-level.

R1's own `CLAUDE.md` at the project root is an instance of this pattern, authored in this project's Phase 2 — it is exempted from artifact frontmatter as an operational file (matching the subagent definition convention), and it is not part of the shipped skill.

## Execution Model

How R1 itself gets built, given the draft→test→refine constraint from `[[RELEASE]]` §Binding Quality Standard and the executor roster from `[[RELEASE]]` §Execution Resourcing.

**Executors.** Already committed in Phase 2:

- _Coordinator_ — main Claude Code session (`CLAUDE.md` at project root). Orchestrates the producer↔supervisor loop, holds project-level state across whatever shape Phase 5 hands over, routes handoffs.
- _Producer_ — Claude Code subagent (`.claude/agents/PRODUCER.md`), skill-creator-class. Authors skill content against scoped units of work. Fresh spawn per iteration, no cross-package memory, framing artifacts delivered via preloaded skill.
- _Supervisor_ — Claude Code subagent (`.claude/agents/supervisor.md`). Runs the eval suite against producer output, characterizes failures, returns advance/rework/escalate verdicts.
- _Operator_ — three touchpoints only: session start (set walls), wall hits (review state), escalations (decide upstream amendment).

Architecture's job here is not to re-commit the roster but to commit the structural mechanics the roster needs to operate.

**The loop.** Producer drafts or refines content against a unit of work the coordinator delivers. Supervisor runs the eval suite, separating mechanical checks (always run) from held-out trigger-rate checks (run when description or routing changes). If checks clear the Phase 5-committed threshold on held-out cases, the iteration advances. If they fail, supervisor characterizes the failure — which check, what the standard expected, what the producer output did instead — and routes back to the coordinator. The coordinator respawns producer with the characterization as input.

The shape Phase 5 hands over (queue, tree, batch, other) determines what counts as a unit of work and how units relate. Architecture commits the loop runs per unit; Phase 5 commits what the units are.

**Eval suite location and lifecycle.** Evals live at the project root in `evals/evals.json` during build. They are authored in Phase 5 as a terminal artifact alongside the execution plan. Phase 6 consumes them via the supervisor; they are not packaged into the shipped skill. Evals are versioned with the project repo, not with the skill distribution.

**Termination.** Two conditions, both required, both committed in `[[RELEASE]]` §Definition of Done as bound by `[[TEST-PLAN]]` §Test Plan Closure. Architecture restates them here for execution-model clarity:

- Mechanical evals pass at the Phase 5-committed threshold on held-out cases.
- The dogfooding gate (`[[TEST-PLAN]]` rows R1-TP-01 through R1-TP-11) closes through operator judgment on March and Nell runs.

Either alone is insufficient. Mechanical pass without dogfooding ships a skill that triggers correctly but produces poor artifacts. Dogfooding pass without mechanical evals ships a skill that worked for the operator on the day they tested but degrades on inputs they did not try.

**Escalation.** When the supervisor returns _escalate_, the coordinator halts work on the affected unit, packages the escalation per `CLAUDE.md` §Escalation handling, and surfaces it to the operator. The operator decides which upstream phase the failure traces to:

- _Phase 5_ if the eval was wrong.
- _Phase 4_ if the spec was wrong.
- _Phase 3_ if the architecture was wrong.
- _Phase 2_ if a binding constraint in `[[RELEASE]]` was wrong.

Each escalation point produces an amendment to the relevant artifact and re-derivation of everything downstream. The amendment is not a quiet edit; it carries a changelog entry naming the trigger and the affected downstream artifacts.

**Session walls.** Bound autonomous coordinator operation. Set per session by the operator at session start (`CLAUDE.md` §Session walls). Architecture commits that walls exist and gate the loop; specific walls are operator decisions per session, not architectural commitments.

**Dogfooding execution.** The March and Nell dogfooding runs are outside the producer/supervisor loop entirely. They are operator-direct runs against the shipped skill, judged against `[[MOCKUP]]` Scene 1 as the reference standard per `[[TEST-PLAN]]`. Architecture commits the boundary: anything Phase 5 hands to the coordinator that looks like dogfooding work is a handoff error and gets flagged to the operator, not spawned against.

## Test Plan Decomposition

The release-level test plan (`[[TEST-PLAN]]`) commits one outcome — dogfooding artifact quality — and eleven rows that close it. This section commits how testing decomposes downward into Phase 4 and Phase 5, and what the build loop's mechanical checks look like as a separate concern from release validation.

**Two layers of testing, distinct purposes.**

- _Release validation._ What the operator does with the shipped skill. Owned by `[[TEST-PLAN]]`. Closes the release. Cannot be mechanically checked because content quality is not mechanically checkable (`[[RELEASE]]` §Binding Quality Standard).
- _Build-loop gating._ What the supervisor does during Phase 6 to gate each producer iteration. Owned by Phase 5's eval suite. Closes individual iterations, not the release. Mechanical because it has to run per-iteration without operator intervention.

The release-level plan and the eval suite are not in a parent-child relationship. They are parallel artifacts serving different gates. Eval suite entries do not have `traces_to` pointing at `[[TEST-PLAN]]` rows because they are not decompositions of those rows — they check different things at different times for different audiences.

**What the eval suite covers.** Mechanical behavior the supervisor can check per-iteration without operator judgment. Phase 5 commits the specific entries; this document commits the categories the entries fall into:

- _Description trigger rate._ Held-out user queries that should invoke the skill produce skill invocation; queries that should not invoke do not.
- _Phase routing._ Given a user request that maps to a specific phase, Claude consulting `SKILL.md` reaches the correct phase reference file before producing.
- _Reference-file discovery._ Claude follows reference invocations from phase guides without prompting from the user.
- _Interrogator-before-production._ Given a phase-task user request, Claude runs the phase's interrogator (asks gating questions, verifies context) before drafting any artifact content.
- _Kernel-surfacing protocol._ Given a Phase 0 cold start, Claude resists proposing a kernel candidate and presses the operator to name one. Failure modes from `project-notes.md` §Graduated → _Phase 0 — the kernel_ serve as the negative test cases.

Phase 5 commits which categories produce which specific eval entries, what their pass thresholds are, and which entries are held out from training.

**Spec-level tests in Phase 4.** Cover individual reference-file content correctness — for each reference file, what does invoking it cause Claude to do, and how is that observable. Spec-level tests trace upward to architectural commitments in this document (specifically §Skill Shape and §Role Architecture), not to `[[TEST-PLAN]]` rows.

**Task-level tests in Phase 5.** Cover individual draft-iteration outputs against eval criteria — for each work package, what does the producer's output need to satisfy. Task-level tests trace upward to spec-level entries.

**Work-package-level tests in Phase 6.** Cover individual producer outputs against supervisor gating criteria per iteration. Trace upward to task-level entries.

The chain is downward-decomposable from spec through task through work-package. A test that cannot decompose downward is a signal that the artifact at the level above is under-resolved. The chain does _not_ extend upward into `[[TEST-PLAN]]` because the release plan checks something different (operator judgment of dogfooding artifact quality) than the build loop checks (mechanical conformance of skill content).

## Scope Exclusions

Pulls Phase 3 work has to resist, named here so the resistance is committed.

**Phase 4 design detail.** Interface contracts, data schemas, file formats, API shapes, the eval suite's `evals.json` structure, the specific contents of phase-guide reference files. This architecture commits to what structures exist (an eval suite, calibrator profiles, a reference topology, a coordinator/producer/supervisor loop); Phase 4 commits to what they look like in detail. The eval suite format is the canonical trap: it is named in this document because it is part of the execution model, but its schema belongs in Phase 4's `spec.md`, not here.

**Phase 5 decomposition.** Which work packages exist, how each is sized for which executor, in what order they run, what evals gate which package, what the eval thresholds are. Phase 3 commits that the loop runs and how it gates; Phase 5 commits what runs through the loop. The calibrator role is named here as part of role architecture, but the calibrator does not actually run until Phase 5 — naming the role is a structural commitment, running it is a planning commitment.

**Re-litigation of Phase 2 commitments.** Whether R1 is the right release shape, whether Anthropic conformance is the right binding standard, whether dogfooding against March and Nell is the right validation gate, whether the framework should ship as a skill at all, whether the coordinator/producer/supervisor split is the right executor roster. These are committed in `[[CONCEPT]]`, `[[ROADMAP]]`, and `[[RELEASE]]`. If architecture work surfaces a real reason to revisit any of them, that goes through an amendment to the upstream artifact and a re-derivation of downstream artifacts — not through this document quietly absorbing the decision.

**Memory mechanics.** `[[RELEASE]]` §Execution Resourcing cut memory from R1 scope. Architecture does not reintroduce it. The graduated project-notes entry on memory-as-durability remains valid for future projects; R1 doesn't exercise it.

## Open Structural Questions

Gaps surfaced during architecture work, scoped to what later phases need to resolve. None blocks Phase 3 from closing.

- _Final skill name._ Working candidate `running-phased-decomposition`. Final name commits in Phase 4 after description-trigger optimization runs and the name's effect on trigger rate is observable.
- _Eval threshold for trigger rate and per-category pass conditions._ This document names that thresholds are required; Phase 5 commits the numbers. Setting them here would fix eval criteria before the eval suite exists, which inverts the dependency.
- _`claude-md-template.md` content scope._ The template lives in `assets/`, but the contents are unspecified — does it stamp a skeleton with executor-type slots, or a worked example projects edit? Both have tradeoffs. Phase 4 resolves against the trigger of authoring the first non-meta `CLAUDE.md` (likely during the March or Nell dogfooding runs).
- _Reference topology under stress._ The proposed topology has nine files in `references/`. If Phase 5 authoring reveals that any phase or role guide grows to a length that wants further splitting, the one-level-deep rule forces a structural decision: split horizontally and keep both at top level, or accept a subdirectory grouping. Architecture cannot predict which file outgrows itself; Phase 5 surfaces it.
- _Writing for two readers._ Opus reads Phase 0, 1, 2, and 5 guides; Sonnet reads Phase 3 and 4. The two want different things from a page. If Phase 5 authoring reveals incompatible conventions, the question routes back to architecture.
- _Phase 3's mixed failure-mode shape._ The architecture phase is resolving-shaped overall (binding constraints in, structural commitments out), but model-tier targeting and role-architecture commitments are framing-shaped — wrong calls hide until Phase 6 blows up. Current split runs Phase 3 on Sonnet and treats this as a risk to watch during dogfooding. If dogfooding reveals Phase 3 needs Opus for the framing-shaped sub-decisions specifically, the resolution is either bumping Phase 3 entirely or splitting Phase 3 across two tiers, which the framework currently does not do.
- _Calibrator's relationship to Phase 2 reference documentation._ Inherited open question from `project-notes.md`. R1 execution is the resolution mechanism; architecture commits the profile mechanism that lets either answer hold.

## Phase 3 Terminal

Phase 3 closes when all of the following hold.

- Every binding constraint from `[[RELEASE]]` §Binding Quality Standard maps to a specific commitment in §Skill Shape, §Role Architecture, §Execution Model, or §Test Plan Decomposition. The mapping is auditable: a reader checking each `[[RELEASE]]` constraint can find the section and paragraph that resolves it.
- Open structural questions are logged in §Open Structural Questions with the phase that resolves each one named.
- Status flips from `draft` to `approved` after operator review.

Phase 4 begins on approval. The first Phase 4 work is `spec.md`, descending from §Skill Shape and §Role Architecture, and the spec-level test plan, descending from §Test Plan Decomposition.

## Changelog

- **2026-04-14 — Rewrite from ground up against corrected Phase 2 cluster.** Prior `ARCHITECTURE.md` (2026-04-13 draft) authored before the structural correction that relocated `CLAUDE.md`, release-level test plan, and release schedule from Phase 3 to Phase 2. Stale draft retained out-of-tree as reference for post-rewrite diff check. New draft descends from `[[RELEASE]]` only (single upstream artifact, since Phase 2 now carries everything Phase 3 used to absorb), narrows scope to pure structural resolution, drops `CLAUDE.md` and release-level test plan as Phase 3 sub-artifacts, drops `evals/` from the shipped skill directory (build-time material, not skill-execution-time material), restructures Test Plan section as Test Plan Decomposition to clarify the build loop's evals are parallel to `[[TEST-PLAN]]` rather than a decomposition of them. Substance carried forward from the prior draft where it survived the correction: directory layout (minus `evals/`), reference topology, role architecture realization, execution loop mechanics, scope exclusions, model-tier targeting decision.