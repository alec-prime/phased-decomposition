---
artifact: architecture
phase: 3
project: "[[Phased Decomposition]]"
release: R1
status: draft
created: 2026-04-13
framework_version: 0.1.0
descends_from:
  - "[[ROADMAP]]"
  - "[[CONCEPT]]"
---
# Architecture — Phased Decomposition R1

## Shape of This Architecture

R1 is the only release the phased decomposition project ships, and its terminal artifact is the phased decomposition skill in the form Anthropic's skill-authoring documentation specifies. This architecture commits the structural decisions that shape what gets built, how the build runs, what gates it, and what Phase 3 deliberately leaves to later phases. Every commitment below traces upward to the binding quality standard in [[ROADMAP|the roadmap]]'s R1 section, and through that to the framework structure in [[CONCEPT|concept]].

## Skill Shape

The Anthropic-conformance constraint cluster from the roadmap resolves into a single structural commitment about what the built skill looks like as a directory of files.

**Directory layout.**

```
phased-decomposition/
├── SKILL.md
├── references/
│   ├── phase-0-concept.md
│   ├── phase-1-northstar-mockup.md
│   ├── phase-2-roadmap.md
│   ├── phase-3-architecture.md
│   ├── phase-4-spec.md
│   ├── phase-5-execution-planning.md
│   ├── phase-6-execution.md
│   ├── calibrator-profile-coding-agent.md
│   └── calibrator-profile-human-operator.md
├── assets/
│   ├── concept-template.md
│   ├── northstar-template.md
│   ├── mockup-template.md
│   ├── roadmap-template.md
│   ├── architecture-template.md
│   ├── spec-template.md
│   ├── claude-md-template.md
│   └── test-plan-template.md
└── evals/
    └── evals.json
```

No `scripts/` directory. The framework's procedures are judgment-heavy and have no deterministic operations that benefit from script execution. If a script need surfaces during Phase 5 authoring, the directory is added then; preemptively scaffolding it would add a category the skill does not use.

**Reference topology.** One level deep from `SKILL.md`. Every file in `references/` is invoked directly from the routing section of `SKILL.md`; no reference file points at another reference file as a continuation. Phase guides may name role files as the role each phase activates, but the activation reads as "consult `role-interrogator.md` before producing" rather than as a chain that walks from one reference into the next. The chain-versus-mention distinction is what Anthropic's one-level-deep rule actually constrains.

**Categorization.** Files Claude reads to learn how to do the work go in `references/`. Files Claude stamps into a project as the operator's working substrate go in `assets/`. The phase templates live in assets because their role is to be copied into a new project's directory and filled in, not to be loaded into Claude's context as instructions. The phase and role guides live in references because they instruct Claude on how to run the phase or occupy the role; they are never copied anywhere.

**Skill name.** Gerund form per Anthropic convention. The working candidate is `running-phased-decomposition`, chosen because it names the activity (running a project through the framework) rather than the framework as an object. Final commitment is deferred to Phase 4 once description-trigger optimization runs against the eval suite — the name appears in the description and affects trigger rate.

**Model-tier targeting.** Opus for Phases 0, 1, 2, and 5. Sonnet for Phases 3 and 4. Phase 6's tier is set per release in `CLAUDE.md`, not here. The split tracks where mistakes hide. Phases 0, 1, 2, and 5 originate commitments that downstream phases consume — concept articulation, illustration, release shape, decomposition and eval authoring — and a wrong call in any of them reads fine on the day it is made and rots downstream artifacts that descend from it. Opus is worth the cost where the loop cannot catch the mistake. Phases 3 and 4 resolve commitments against upstream structure (architecture against roadmap, spec against architecture); their mistakes show up immediately as broken downstream work, and Sonnet handles them reliably.

**Voice constraints.** Phase 5 authoring is bound to the scribe voice standard committed in [[Projects/Phased Decomposition/project-notes#The scribe role and the artifact voice standard|framework-notes]]: demonstrative rather than persuasive, theory-of-mind explanations of why rather than imperative MUSTs, science-communicator register that brings a reader up to speed without requiring them to absorb a stance first. Anthropic's best-practices guidance on theory-of-mind voice over heavy-handed MUSTs aligns with the scribe standard and reinforces it; the constraint is not new to the skill.

## Role Architecture

The framework commits three universal roles — interrogator, scribe, calibrator — but defers to Phase 3 the question of how those roles realize in a built skill. This section commits the realization.

**Role definitions live in SKILL.md body, not in references.** The interrogator standard, the scribe voice standard, and the calibrator's operating question are universal across phases and need to be in Claude's context from the moment the skill triggers, not loaded on-demand mid-session. SKILL.md body absorbs them as part of the framework's universal context, alongside the phase pipeline overview and the routing table to phase guides.

This is the resolution of the role-references framework-notes entry's deferred question. Role definitions are authoring-time material for SKILL.md itself — they shape what SKILL.md says, and once SKILL.md is written they have done their job. They do not ship as separate files because they are not consulted independently at runtime; they are embodied in the skill's instructions from the start of every session.

**Phase guides invoke roles by name, not by definition.** Each phase reference file in `references/` names which roles run during that phase and what they do in that phase's specific context. Phase 0's guide names that the interrogator runs first against kernel surfacing, then against the section sweep, with the failure modes from the kernel framework-notes entry as negative test cases. Phase 5's guide names that the interrogator runs first, the calibrator runs during decomposition, and the scribe runs against the dual-artifact output. The phase guide assumes SKILL.md has established what each role *is*; the phase guide commits where each role *applies* and what the role's output for that phase looks like.

**Calibrator runtime references.** The calibrator is the only role whose judgment depends on field-level material that varies across executors and moves over time. Its runtime references — per-executor profiles describing what right-sized work looks like for each executor type — live as sibling files in `references/`. Naming convention: `calibrator-profile-{executor-type}.md`. R1 ships with two profiles: `calibrator-profile-coding-agent.md` (drawing from Anthropic's agentic coding guidance and Claude Code task-decomposition patterns) and `calibrator-profile-human-operator.md` (drawing from the operator's self-knowledge about their own working patterns). New executor types add new files; profiles update independently of any role definition. The calibrator role definition in SKILL.md names that the calibrator consults profiles at runtime and where to find them.

**Why interrogator and scribe don't have profiles.** The interrogator's job — asking targeted questions in the operator's register to clear context gaps — depends on the operating question, which is universal, plus failure examples that calibrate judgment, which are also universal. The scribe's voice standard is similarly universal: the operating question (does this teach or work on the reader) and the failure examples carry across all artifacts. Neither role's judgment depends on material that varies by project or moves over time, so neither needs runtime references. If a future role surfaces with that kind of judgment dependency, it gets profiles; the calibrator remains the only one for now.

**Relationship to CLAUDE.md.** `CLAUDE.md` is a per-release artifact authored in Phase 3 of each release that uses the framework, naming the specific executors that release will use. It is not part of the skill itself; the skill ships a template for it (`assets/claude-md-template.md`) and authoring guidance in `references/phase-3-architecture.md`. The skill knows about executor types in general (via calibrator profiles); each release knows about its specific executors (via its own CLAUDE.md). The two kinds of executor knowledge coexist without overlap because they live at different scopes — framework-level versus release-level.

## Execution Model

How R1 itself gets built, given the draft→test→refine constraint from the roadmap.

**Executors for R1.**

- _Producer._ A Claude instance with write access to the skill directory. Drafts and refines `SKILL.md`, reference files, asset templates, and the eval suite based on Phase 5's work packages. Coding-agent-class executor; the coding-agent calibrator profile applies.
- _Supervisor._ A Claude instance with read access to the skill directory and execution access to the eval suite. Runs evals against producer output, characterizes failures, and gates iteration. Holds the work-package-level test plan as its standard.
- _Operator._ The framework author. Gates the loop at iteration boundaries, authors dogfooding feedback against March and Nell, and resolves escalations the supervisor surfaces.

The calibrator role runs in Phase 5 (eval-suite authoring and work-package sizing), not in Phase 6. Phase 6's loop consumes the calibrator's output rather than re-running it.

**The loop.** Producer drafts or refines content against a work package. Supervisor runs the eval suite, separating mechanical checks (always run) from held-out trigger-rate checks (run when description or routing changes). If the eval pass rate clears the Phase 5-committed threshold on held-out cases, the iteration advances. If it fails, supervisor characterizes the failure — which eval, which expected behavior, what the producer output did instead — and routes back to producer with the characterization as input for the next iteration.

**Termination.** Two conditions, both required. Mechanical evals pass at the Phase 5-committed threshold on held-out cases. The dogfooding gate (§Test Plan Architecture) closes through operator judgment on March and Nell runs. Either alone is insufficient: mechanical pass without dogfooding ships a skill that triggers correctly but produces poor artifacts; dogfooding pass without mechanical evals ships a skill that worked for the operator on the day they tested but degrades on inputs they did not try.

**Escalation.** When supervisor detects a failure the loop cannot close — the producer cannot satisfy an eval because the eval is checking for behavior the SKILL.md cannot produce given current architecture — supervisor halts and escalates to operator review. The operator decides which upstream phase the failure traces to. Phase 5 if the eval was wrong. Phase 4 if the spec was wrong. Phase 3 if the architecture was wrong. Phase 2 if a binding constraint in the roadmap was wrong. Each escalation point produces an amendment to the relevant artifact and re-derivation of everything downstream.

## Test Plan Architecture

The release-level test plan is a sibling artifact derived from this section, authored in parallel with this document per the universal parallel-test-plan rule. This section commits what the plan contains and how testing decomposes downward into Phase 4 and Phase 5.

**Mechanical checks.** Run by supervisor throughout the loop. Each check is observable, repeatable, and has a defined pass condition.

- _Description trigger rate._ Held-out user queries that should invoke the skill produce skill invocation; queries that should not invoke the skill do not. Threshold set in Phase 5 against the optimized description.
- _Phase routing._ Given a user request that maps to a specific phase, Claude consulting `SKILL.md` reaches the correct phase reference file before producing.
- _Reference-file discovery._ Claude follows reference invocations from phase guides without prompting from the user.
- _Interrogator-before-production._ Given a phase-task user request, Claude runs the phase's interrogator (asks gating questions, verifies context) before drafting any artifact content.
- _Kernel-surfacing protocol._ Given a Phase 0 cold start, Claude resists proposing a kernel candidate and presses the operator to name one in their own words. The failure modes from the kernel framework-notes entry serve as the negative test cases.

**Dogfooding gate.** Run by the operator against the live skill, after mechanical checks pass. Each dogfooded project clears Phase 0 minimum: the operator runs the project through Phase 0 using the skill, and the resulting `concept.md` is judged against the corresponding scene in [[MOCKUP|mockup]] as the reference standard. The gate closes when both March and Nell produce concept docs the operator finds the skill sufficient for. Dogfooding failures route to the same escalation paths the loop's mechanical failures use.

**Decomposition strategy.** Spec-level tests in Phase 4 cover individual reference-file content correctness — for each reference file, what does invoking it cause Claude to do, and how is that observable. Task-level tests in Phase 5 cover individual draft-iteration outputs against eval criteria — for each work package, what does the producer's output need to satisfy. Work-package-level tests in Phase 6 cover individual producer outputs against supervisor gating criteria — for each iteration of each package, did this draft clear the gate.

The chain is downward-decomposable: every Phase 4 spec-level test traces upward to a release-level entry in this section, every Phase 5 task-level test traces upward to a Phase 4 entry, every Phase 6 work-package test traces upward to a Phase 5 entry. A test that cannot decompose downward is a signal that the artifact at the level above is under-resolved.

## Scope Exclusions

Three pulls Phase 3 work has to resist, named here so the resistance is committed.

**Phase 4 design detail.** Interface contracts, data schemas, file formats, API shapes, the eval suite's `evals.json` structure. This architecture commits to what structures exist (an eval suite, a CLAUDE.md template, a reference topology); Phase 4 commits to what they look like in detail. The eval suite format is the canonical trap: it is named in this document because it is part of the execution model, but its schema belongs in the concept doc's Phase 4 (`spec.md`), not here.

**Phase 5 decomposition.** Which work packages exist, how each is sized for which executor, in what order they run, what evals gate which package. Phase 3 commits that the loop runs and how it gates; Phase 5 commits what runs through the loop. The calibrator role is named here as part of role architecture, but the calibrator does not actually run until Phase 5 — naming the role is a structural commitment, running it is a planning commitment.

**Re-litigation of upstream commitments.** Whether R1 is the right release shape, whether Anthropic conformance is the right binding standard, whether dogfooding against March and Nell is the right validation gate, whether the framework should ship as a skill at all. These are roadmap and concept commitments. If architecture work surfaces a real reason to revisit any of them, that goes through an amendment to the upstream artifact and a re-derivation of downstream artifacts — not through this document quietly absorbing the decision.

## Open Structural Questions

Gaps surfaced during architecture work, scoped to what later phases need to resolve. None blocks Phase 3 from closing.

- _Final skill name._ Working candidate is `running-phased-decomposition`. Final name commits in Phase 4 after description-trigger optimization runs and the name's effect on trigger rate is observable. Alternatives in scope: `decomposing-projects-by-phase`, `phasing-projects`, `structuring-projects-in-phases`. The candidate set is open; Phase 4 picks.
- _Eval threshold for trigger rate._ This document names that a threshold is required; Phase 5 commits the number. Setting it here would fix an eval criterion before the eval suite exists, which inverts the dependency.
- _CLAUDE.md template scope._ The template lives in `assets/`, but the template's contents are unspecified — does it stamp a skeleton with executor-type slots, or a worked example projects edit? Both have tradeoffs. Phase 4 resolves against the trigger of authoring the first non-meta CLAUDE.md (likely during the March or Nell dogfooding runs).
- _Reference topology under stress._ The proposed topology has thirteen files in `references/`. If Phase 5 authoring reveals that any phase or role guide grows to a length that wants further splitting, the one-level-deep rule forces a structural decision: split the file horizontally and keep both at top level, or accept a subdirectory grouping. Architecture cannot predict which phase or role will outgrow its file. Phase 5 surfaces it; this document acknowledges the contingency.
- _Writing for two readers._ Opus reads Phase 0, 1, 2, and 5 guides; Sonnet reads Phase 3 and 4. The two want different things from a page. If Phase 5 authoring reveals incompatible conventions, the question routes back to architecture.
- _Phase 3's mixed failure-mode shape._ The architecture phase is resolving-shaped overall (binding constraints in, structural commitments out), but its CLAUDE.md and release-level test plan sub-artifacts are framing-shaped — wrong calls in either hide until Phase 6 blows up. The current split runs Phase 3 on Sonnet and treats this as a risk to watch during dogfooding. If dogfooding reveals that Phase 3 needs Opus for the framing-shaped sub-artifacts specifically, the resolution is either bumping Phase 3 entirely or splitting Phase 3 across two tiers, which the framework currently does not do.

## Phase 3 Terminal

Phase 3 closes when all of the following hold.

- Every binding constraint from the roadmap's §Binding quality standard maps to a specific commitment in §Skill Shape, §Role Architecture, §Execution Model, or §Test Plan Architecture. The mapping is auditable: a reader checking each roadmap bullet can find the section and paragraph that resolves it.
- `CLAUDE.md` is authored as a sibling artifact, naming the producer, supervisor, and operator executors committed in §Execution Model, with their boundaries and read/write scopes specified.
- The release-level test plan is authored as a sibling artifact, derived from §Test Plan Architecture, with each entry traced to the constraint or framework rule that generates it.
- Open structural questions are logged in §Open Structural Questions with the phase that resolves each one named.
- Status flips from `draft` to `approved` after operator review.

Phase 4 begins on approval. The first Phase 4 work is `spec.md`, which descends from §Skill Shape and §Role Architecture, and the spec-level test plan, which descends from §Test Plan Architecture's decomposition strategy.

## Changelog

- **2026-04-13 — Initial draft.** Eight sections derived from the four-part rhythm rule and the binding constraints in the roadmap's R1 section. Role Architecture added as a substance section after framework-notes review surfaced that the universal roles' skill-instantiation was deferred to Phase 3. Scope Exclusions added after cross-comparison with concept and roadmap showed every prior phase artifact carried explicit bounds.
- **2026-04-13 — Role definitions relocated from references/ to SKILL.md body.** Initial draft placed `role-interrogator.md`, `role-scribe.md`, and `role-calibrator.md` in `references/`. Operator review caught the category error: role definitions are authoring-time material for SKILL.md itself, not on-demand runtime material. Universal role standards now live in SKILL.md body and are invoked by name from phase guides. Calibrator profiles remain in `references/` as the only true runtime role material. Directory file count drops from thirteen to nine.