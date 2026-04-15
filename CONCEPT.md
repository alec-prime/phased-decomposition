---
artifact: concept
phase: 0
project: "[[Phased Decomposition]]"
release:
status: approved
created: 2026-04-12T14:40:59-07:00
framework_version: 0.1.0
descends_from: []
---
# Phased Decomposition — Concept


## Introduction

Most of what it takes to turn an idea into something real happens between the first intuition and the finished thing. Resolving what the idea actually is, what it isn't, what it's supposed to become, how it gets built — this is the bulk of the work, and it is the work that phased decomposition is for. Its purpose is to make ideas real.

Historically, carrying an idea across that stretch at any serious scale required a team. A single person could get there, but the path was long and the load was heavy enough that most ideas held by one person stayed ideas. LLM collaboration changes the arithmetic. One operator working with a capable model can cover ground that previously took a team — but only if the model is shaped into something that can do the work. Raw conversation with an LLM produces transcripts, not progress. The model has to be harnessed.

[[Phased Decomposition|Phased decomposition]] is that harness. It stages the work of making an idea real into discrete phases, each producing a terminal artifact that carries forward both decisions and the reasoning behind them. Phases run from cheapest-to-change thinking to most expensive: concept, illustration, mapping, architecture, execution. Drift and contradiction surface upstream, where revision costs little, rather than downstream during build, where revision costs the most. Artifacts carry the project across the gaps between sessions, so that reasoning built in one sitting survives into the next. Phased decomposition is what a one-person team looks like when the rest of the team is an LLM, shaped to be useful.

## The Thesis

Phased decomposition is a framework for making ideas real. It runs the work between idea and finished form as a pipeline of discrete phases, each producing a terminal artifact that carries decisions forward with their rationale. Phases progress cheapest-to-change to most-expensive, so drift and contradiction surface upstream where revision is cheap rather than downstream during build where it isn't. An LLM collaborator, shaped by the framework into something that can do the work, is the enabling condition that makes one operator running the full pipeline viable — the novelty is not staged work, which is old, but one-person execution of it.

The operator who enters the pipeline in idea-space — sketching, staying general, speaking to how things should feel rather than how they should be built — stays in idea-space throughout. Residual ambiguity in any phase is routed through the LLM collaborator, which asks questions in the operator's native register and renders the answers into production vocabulary on their behalf. The same translation role extends into supervision of running work against its upstream artifacts, so the operator's cognitive mode is preserved even when they are not in the session. The operator never has to shift register to commit execution-grade decisions or to supervise what descends from them.

Each phase has a primary mode. Phase 0 teaches — its artifacts orient a reader to the project from cold. Phase 1 illustrates — it makes the concept tangible. Phase 2 maps — it lays out the route from the illustrated end state to a sequence of releases. Later phases narrow into per-release architecture and execution. The progression mirrors how understanding actually deepens: from why, to what, to how, to do.

### The Phases

The pipeline is five phases. Each produces a terminal artifact (or set of artifacts) that carries the phase's decisions and rationale forward.

- **Phase 0 — Concept.** Articulates the project from cold: kernel, stakes, intent, shape, precedent. The kernel — a compressed statement of what the project is, one layer below its surface framing — is the load-bearing element every downstream phase descends from, and surfacing it is Phase 0's most consequential production requirement. Non-technical, legible to outside readers. _Artifact:_ `concept.md`. Mutable after downstream phases begin, with a changelog tracking what changed and which downstream artifacts are affected.

- **Phase 1 — North Start & Mockup.** Makes the concept tangible. The [[NORTHSTAR|north star]] describes the scenes that illustrate the concept's payoff at the level of abstraction — re-instantiable, with no specific operator, project, or filenames. The [[MOCKUP|mockup]] is one concrete instantiation of those scenes in the medium the project's external surface calls for. The first outward-communicating artifacts; a guide for downstream phases, not a lock. _Artifacts:_ `northstar.md`, `mockup.md`.

- **Phase 2 — Release Roadmap.** [[ROADMAP|Maps]] the route from the illustrated end state to a sequence of independent releases, and activates the current release by committing the artifacts that turn it from a planned unit into an active project. The roadmap is directional across all releases; activation artifacts exist only for the release being worked on and are authored fresh each time a new release begins. _Artifacts:_ `roadmap.md` (all releases); `CLAUDE.md`, release-level test plan, and release schedule (current release only).

- **Phase 3 — Release Architecture.** Per-release. Stress-tests the release against constraints, resolves structural questions, and commits to an approach. Each release gets its own architecture; cross-release architecture is not tracked. _Artifact:_ `architecture.md`.

- **Phase 4 — Release Spec.** Per-release. Packages the architecture into executable form: design decisions, interface contracts, structural detail needed to build against. _Artifacts:_ `spec.md`, `design.md`, spec-level test plan (per release).

- **Phase 5 — Execution Planning.** Per-release. Decomposes the spec into a tree whose leaves are sized per the executors `CLAUDE.md` committed, and authors the evaluation set the execution loop will be gated against at every level. Produces dual artifacts from one underlying tree: a human-readable plan summarized for operator review, and a machine-readable plan carrying the full decomposition for Phase 6. Terminates at a review gate covering plan and evaluation set together. _Artifacts:_ human-readable plan, machine-readable plan, evaluation set.

- **Phase 6 — Execution.** Per-release. The executors named in `CLAUDE.md` run the leaves from Phase 5's machine-readable plan; the supervisor gates each against Phase 5's evaluation set, halting on failure and routing for rework or escalation. No planning or derivation happens here. _Artifacts:_ completed leaves, passing evaluations.

Phases 0–2 are framing work shared by the project as a whole. Phases 3–6 repeat per release, with each release treated as an independent, complete product rather than an increment of a single system.

## Boundaries

**In scope**

- Defining the phases, their terminal artifacts, and the templates that guide authoring.
- Defining the transition rules and validation criteria between phases.
- Defining the operational protocols that govern how a session conducts phase work.
- Allowing concept-level work to expand into supporting sub-documents when a single file becomes unwieldy, provided each sub-document remains Phase 0 in character.

**Out of scope**

- Tooling or automation that enforces phase transitions or executes phase work.
- IDE, editor, or platform integration.
- Domain-specific adaptation of the framework (engineering-only, design-only, etc.).
- Prescribing which LLM, which interface, or which auxiliary tools a session uses.

**Fixed constraints**

- Must generalize across project types — software, physical, organizational, creative. Ideas are not restricted to one domain; a framework for making them real cannot be either.
- Concept-level artifacts must be legible to a reader encountering the project cold; downstream artifacts may presuppose upstream ones.
- Framework version control is maintained in GitHub. Projects reference the framework version they were initiated against, and mid-flight projects are not required to absorb subsequent framework updates.

## Success Criteria

- A project can be picked up in a fresh session using only its artifacts and resume without the new session needing to re-litigate decisions already made.
- A reader outside the project can read `concept.md` and understand the project's stakes, intent, and shape without additional context.
- The same phase structure applies without modification across project types, including non-software projects.
- Phase transitions surface gaps, contradictions, or unresolved tensions before downstream phases begin, rather than during them.
- The framework is communicable to a third party — including a portfolio reviewer — without requiring the author to be present to narrate it.

## Open Questions

- How does the framework hold up under full end-to-end execution? Work packages have been run to completion and individual phases have produced usable artifacts, but no project has yet traversed the full pipeline from concept through release. The inter-phase handoffs, the handling of downstream information that invalidates upstream artifacts, and the retrospective or learning-capture mechanisms between releases are currently undefined or handled ad hoc. _Blocks:_ confidence that the framework is sufficient as a complete pipeline rather than a collection of useful stages. _Answer source:_ completing a first project end-to-end against the framework.

- How does the split between automated and human-judged testing vary across project types? Some artifacts have ground-truth outputs (data transforms, code with defined behavior); others are irreducibly matters of human judgment (framings, concepts, experiences). Most projects land in between. The framework names test plans at every level as universal but does not specify how to allocate them. _Blocks:_ Phase 3 lacks a decision framework for the split, leaving it to ad-hoc resolution per release. _Answer source:_ Phase 3 work across multiple project types, accumulating the pattern empirically. The phased decomposition skill itself is one data point — human judgment dominates because no ground-truth output exists to grade a concept, northstar, or mockup against.

- Which phase artifacts require templates, and which are better served by prose guidance alone? Templates authored against a single instance risk baking that instance's shape into the framework's general form — a hazard already visible in Phase 1 (visual-surface assumptions) and Phase 2 (degenerate single-release roadmap). _Blocks:_ template drafting across all phases. _Answer source:_ authoring each phase against dogfooding projects and observing whether templates reduce friction or introduce it.


## Precedent & Alternatives

**Precedent**

Disciplines for making ideas real are old. Every field that turns intent into artifact has developed its own version — engineering's design process, research methodology's progression from hypothesis to experiment, architecture's arc from program through schematic to construction documents, film production's movement from treatment through shooting script to cut. Each encodes the same instinct: resolve the cheap questions before the expensive ones, and carry decisions forward in artifacts that survive the people who made them. Phased decomposition descends from that lineage.

Within software and product practice specifically, four threads shaped the instincts behind this version:

- Traditional software development lifecycle stages establish the discipline of moving from intent to execution through defined handoffs.
- Agile and its descendants contribute the principle that plans should be revisable as understanding deepens, and that releases are the natural unit of progress.
- Design thinking's double-diamond model informs the upstream-heavy weighting — the idea that framing work earns its keep by reducing rework downstream.
- Spec-driven development in agent frameworks demonstrates that artifact handoff between automated steps is tractable when the artifacts themselves are structured and self-describing.

Phased decomposition is the LLM-collaborator instance of this lineage. The framework's novelty is not that it stages work — staged work is old — but that it shapes LLM collaboration into something that can run the full pipeline with one operator at the center, where the same pipeline would previously have required a team.

**Alternatives considered**

- _Reliance on Claude memory and Projects features alone._ Set aside because reasoning history is summarized into memory rather than preserved, and the goal of the framework is that decisions and their rationale remain readable and revisable in their original form.
- _A single living project document updated continuously._ Set aside because consolidating upstream framing and downstream execution detail into one file makes it difficult to keep them distinct; separating them by phase preserves the discipline of resolving framing before execution.
- _Tool-enforced phase gates._ Set aside as premature. The phase structure itself is still settling, and enforcement is only worth building once the structure it would enforce is stable.

## Stakeholders

- _Primary user._ The framework's author and operator. Cares about the framework reducing friction on real project work and producing artifacts worth keeping. Holds full authority over framework direction.
- _Portfolio audience._ Prospective employers and collaborators in the AI and product space who may encounter the framework as evidence of how the author works. Cares about whether the framework demonstrates rigor, original thinking, and practical judgment about working with LLMs. Influences the framework's communicability and external legibility, but not its internal structure.
- _Future project collaborators._ Anyone — human or model — who picks up an in-flight project built using the framework. Cares about being able to resume work without the original author present. Influences the teach-from-cold principle of Phase 0 and the discipline of carrying rationale forward in every artifact.

## Changelog

- **2026-04-13 — Phase 5 renamed to Execution Planning; testing split surfaced as open question.** Phase 5 renamed from "Task Breakdown" to "Execution Planning" and its description expanded to cover both task breakdown and evaluation-set authoring, with the evaluation set authored against the spec before breakdown begins and incorporated as the breakdown's first work package. Artifacts list updated to include the evaluation set. Review gate clarified as covering both breakdown and test plan together. _Why:_ Phase 2 research into Anthropic's canonical skill-authoring methodology surfaced that skill evaluation is an iterative build-time activity rather than a post-implementation validation, and that the evaluation set should be authored before extensive skill content. The original "Task Breakdown" framing assumed the spec decomposes directly into tasks, leaving no home for eval authoring. Execution Planning covers both jobs and pairs semantically with Phase 6's Work Packages (plan → package → execute). Separately, a new open question was added naming the project-dependent split between automated and human-judged testing, prompted by recognizing that the phased decomposition skill itself is a project where human judgment dominates because its outputs (framings, concepts, experiences) have no ground truth a grader can check against. _Affects:_ Phase 5 bullet in `## The Phases` (updated); `## Open Questions` (new entry on testing split).
    
- **2026-04-12 — Phase 1 definition sharpened.** Refined what `northstar.md` and `mockup.md` are and how they relate. Northstar is now defined as the scene at the level of abstraction (re-instantiable; no specific operator, project, or filenames). Mockup is one concrete instantiation of those scenes in whatever medium the project's external surface calls for. Added the directory-expansion rule for mockup when components need to be independently consumable downstream. _Why:_ the original Phase 1 description ("moments that matter most as small scenes" / "first-stab content for each moment") was ambiguous about the decomposition between the two artifacts, especially for projects whose external surface is not visual. Sharpening the split by abstraction-vs-instantiation generalizes cleanly across project types and resolves the ambiguity without introducing a meta-project variant. _Affects:_ Phase 1 bullet in `## The Phases` (updated in this revision); future `northstar.md` and `mockup.md` artifacts for this project; framework template definitions for northstar and mockup (captured in `framework-notes.md` pending skill draft).
    
- **2026-04-12 — Introduction rewritten to surface the kernel.** The introduction previously opened from the session-gap problem and framed phased decomposition as a response to LLM session ephemerality. This left the project's kernel implicit and inverted the relationship between the framework and LLM collaboration: the framework exists to make ideas real, and LLM collaboration is the enabling condition that makes one-operator execution of that work viable, not the problem the framework was created to solve. Rewritten to open from the kernel — _making ideas real_ — and recast session continuity as one of several forces the framework manages inside the harness rather than as the reason the framework exists. _Affects:_ Introduction only.
    
- **2026-04-12 — Downstream sections aligned to kernel.** Ran Thesis, Boundaries, and Precedent & Alternatives against the kernel (_making ideas real_) committed in the Introduction rewrite earlier today. Three edits landed. Thesis opening paragraph flipped polarity: the first sentence now descends from the kernel rather than from LLM-collaborator framing, and LLM collaboration is repositioned as the enabling condition that makes one-operator execution viable. Boundaries fixed-constraints bullet on generalization now carries a one-line gloss deriving domain-generality from the kernel rather than imposing it as a constraint. Precedent subsection rewritten to acknowledge the broader making-ideas-real lineage across disciplines (engineering, research, architecture, film) before narrowing to the four software-and-product threads that directly shaped this version; closing paragraph reframed so phased decomposition's novelty is LLM-enabled one-operator execution of an old discipline rather than a response to LLM limitations. _Why:_ with the kernel committed in the Introduction, downstream sections needed to be checked for descent from it. Three sections were descending from implicit older framings (LLM-first in Thesis, imposed-constraint in Boundaries, software-only lineage in Precedent) and have been corrected. _Affects:_ Thesis (first paragraph), Boundaries (fixed-constraints generalization bullet), Precedent & Alternatives (Precedent subsection).
    
- **2026-04-12 — Kernel surfaced in Phase 0 description.** Added kernel to the Phase 0 bullet in The Phases, defining it inline and naming it as the load-bearing element every downstream phase descends from. _Why:_ framework-notes establishes kernel as Phase 0-critical and the single highest-leverage move in the pipeline, but the concept doc's Phase 0 description did not mention kernel at all. A cold reader would not have learned that a surfaced kernel is required. _Affects:_ The Phases (Phase 0 bullet).

- **2026-04-13 — Phase 5/6 split corrected; `CLAUDE.md` added as Phase 3 artifact; Phase 1 condensed.** Phase 5 and Phase 6 were conflated: derivation and classification were assigned to Phase 6, and Phase 5 was framed as breakdown plus an eval set incorporated as a first work package. Corrected: all decomposition and eval authoring belong to Phase 5; Phase 6 collapses to pure execution against Phase 5's artifacts. Phase 5 now produces dual artifacts — a human-readable plan for operator review and a machine-readable plan for Phase 6's supervisor — derived from one underlying tree whose leaves are sized per-executor. Phase 3 gains `CLAUDE.md` as a third terminal artifact, defining the executors that will run the release; Phase 6 renamed from "Work Packages" to "Execution" to reflect the collapsed scope. Phase 1 bullet condensed in the same pass, dropping the directory-expansion rule (graduated in `framework-notes.md`) and the divergence-logging clause as operational detail a cold reader does not need. _Why:_ the prior framing left two structural confusions unresolved — where eval authoring belongs (planning, not execution) and how per-executor granularity gets tuned (requires a named role consulting an artifact that names the executors). The calibrator framework role and the universal role-references mechanism, both added to `framework-notes.md` in this session, are the upstream entries this amendment descends from. _Affects:_ Phase 1, Phase 3, Phase 5, and Phase 6 bullets in `§The Phases`. Roadmap.md descends from these changes and is the next amendment in the queue.
- **2026-04-14 — Release-level artifacts relocated to Phase 2.** `CLAUDE.md`, release-level test plan, and release schedule moved to Phase 2 as release-activation artifacts. Phase 2 reframed to carry two jobs: mapping the release sequence and activating the current release. Phase 3 narrowed to pure structural resolution; terminal artifact is `architecture.md` alone. _Why:_ `CLAUDE.md` and release-level test plan were miscategorized as architecture artifacts when they are release-activation artifacts; the schedule entry queued for Phase 0 was the same error in a different direction. Both descend from a new framework-notes rule: release-level artifacts belong to the phase that defines the release. _Affects:_ Phase 2 and Phase 3 bullets in `§The Phases`. Downstream: `roadmap.md` amendment, `architecture.md` full rewrite.