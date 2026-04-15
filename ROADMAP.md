---
artifact: roadmap
phase: 2
project: "[[Phased Decomposition]]"
release: R1
status: in-review
created: 2026-04-13
framework_version: 0.1.0
descends_from:
  - "[[NORTHSTAR]]"
---
# Phased Decomposition — Release Roadmap

## Shape of This Roadmap

The phased decomposition project ships a single release. R1 is the only release, and its terminal artifact is the phased decomposition skill in the form Anthropic's skill-authoring documentation specifies. There is no R2, no R3, no sketch of releases to come. The framework is complete when R1 ships.

A single release for this project has no bearing on release quantities for projects that use the skill created from phased decomposition's framework.

The roadmap's remaining job is to commit R1's boundary, forward the binding constraints that must hold in Phase 3 architecture, and name the gates R1 must clear before it closes.

## R1 — The Phased Decomposition Skill

### What R1 ships

An Anthropic-conforming skill encoding the phased decomposition framework. The directory structure, at minimum:

- `SKILL.md` — routing document with YAML frontmatter (name, description) and body under Anthropic's 500-line target, authored in the user's voice against the skill's audience (fresh Claude instances running other projects through the framework).
- `references/` — phase-specific guidance files linked one level deep from `SKILL.md`, covering the five phases and the universal roles (interrogator, scribe, calibrator) that run across them.
- `assets/` — the phase templates the skill stamps into projects: `concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`, `architecture.md`, `spec.md`, and whatever test-plan templates Phase 3 architecture commits to.
- `evals/` — the evaluation set Phase 5 authors as a terminal artifact, in Anthropic's `evals.json` format, against which Phase 6's supervisor gates each iteration of the draft→test→refine loop.
- `scripts/` — if Phase 3 architecture determines scripts are required for deterministic operations within the skill; otherwise omitted.

All contents are authored fresh in Phase 6 against the source artifacts and evals produced in Phases 0–5 of this project. Source artifacts are not reused verbatim; the skill's documentation is derived, not republished.

### Schedule
- *See: [[SCHEDULE|Schedule]]*

### Test Plan
- *See: [[TEST-PLAN|Test Plan]]*

### Binding quality standard

What R1 commits to being and doing. Phase 3 architecture receives each of the following as a constraint and resolves it to a specific structural commitment.

- **Anthropic skill-authoring methodology.** R1 conforms to Anthropic's documented skill-authoring practice as the binding external standard, captured at `platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices` and operationalized in the `skill-creator` skill bundled with Claude.
    
    - **Progressive disclosure through three loading levels.** Metadata in frontmatter, body in `SKILL.md` (target under 500 lines), deeper content in bundled resources loaded only when referenced. The body is a router, not a repository.
        
    - **Description as primary trigger.** The `description` field is the mechanism by which the skill loads at all. Phase 3 commits an initial description; Phase 5 authors the matching eval entries; Phase 6 runs the optimization loop against them. Third person, includes what the skill does and when to use it, "pushy" phrasing to counter undertriggering, under 1024 characters.
        
    - **Three resource categories.** `scripts/` for executable code, `references/` for docs loaded into context as needed, `assets/` for files used in output (templates stamp into projects and belong here, not in `references/`). Phase 3 commits the categorization of every planned file.
        
    - **References one level deep.** All reference files link directly from `SKILL.md`. No nested reference chains. Phase 3 commits the full reference topology and verifies depth.
        
    - **Theory-of-mind voice over imperative MUSTs.** Skill content explains _why_ reasoning is important rather than imposing rules, so the skill generalizes to unanticipated edge cases. This is a voice constraint on Phase 5 authoring, not a structural decision, but Phase 3 architecture notes it as binding.
        
    - **Gerund-shaped naming.** The skill name is authored in gerund form (verb + -ing) as Anthropic's preferred convention. The current project name "phased decomposition" is a noun phrase and does not become the skill name unchanged. Phase 3 commits a specific skill name.
        
    - **Model-tier targeting decision.** Anthropic recommends testing across Haiku, Sonnet, and Opus because guidance that reads as under-explained to Haiku may be over-explained to Opus. Phase 3 commits whether the skill targets a single tier or multiple; this affects voice calibration and content density throughout.
        
- **Draft→test→refine execution loop.** R1's Phase 6 produces skill content iteratively rather than in a single pass, because skill quality is only observable by running fresh Claude instances against the skill itself. A producer agent drafts or refines content; a supervisor agent runs the eval suite and gates each iteration. Failures route for rework within the same work package or escalate upstream when the failure indicates Phase 5's plan is incomplete. Phase 3 architecture accounts for the loop's structural requirements: how the eval suite is staged as Phase 5's terminal artifact, how work packages are sized to fit single iterations, how termination is decided, and how escalation paths are defined.
    
- **Content quality is not mechanically checkable.** Most of the skill's outputs — concept docs, northstars, mockups, kernels surfaced from operator intent — have no ground-truth form a grader can check against. A fresh Claude asked to ideate a project and run it through Phase 0 is hallucinating both the project and the operator, so grading the output tests Claude's roleplay ability rather than the skill's guidance quality. Automated evaluation can therefore cover only mechanical behavior — description triggering, phase routing, reference-file discovery, interrogator-before-production, kernel-surfacing protocol. These run during Phase 6 as the build-time signal for the draft→test→refine loop. The evaluation suite that gates them is itself an R1 deliverable, alongside the skill.
    
- **Dogfooding against real non-framework projects.** Because automated evals cannot cover content quality, R1 does not close until the skill has been used to run at least one non-framework project through Phase 0, with the resulting artifacts judged against the mockup scenes in `mockup.md` as the reference standard. The committed dogfooding targets are March and Nell — both pre-existing projects outside the framework's authoring scope, each at a stage where a fresh Phase 0 pass is independently useful and not retrofitted ceremony. These runs produce the artifact-quality signal the automated evals cannot, and simultaneously produce the first evidence that the framework's claim of generalizing across project types actually holds. The dogfooding runs are not "nice to have" — they are the only source of the validation signal that matters most for this release. Without them, R1 ships blind to its most important failure mode.


### Execution Resourcing

R1 commits three agents and one human operator. Their full definitions — tool access, handoff protocols, preloading, iteration boundaries — are authored in R1's `CLAUDE.md` as a Phase 2 release-activation artifact. The roadmap commits the roster; `CLAUDE.md` commits the agents.

- **Coordinator** — main Claude Code session. Orchestrates producer and supervisor, holds project-level state across the work package queue, routes handoffs. Does not author skill content or run evals.
- **Producer** — Claude Code subagent, skill-creator-class. Authors skill content (SKILL.md, references/, assets/, evals/, scripts/) against the framing artifacts from Phases 0–4. Anthropic's bundled `skill-creator` skill is the producer's primary reference base — it is self-contained for R1's purposes, covering skill-writing mechanics, eval authoring, the draft→test→refine loop, and runnable tooling for all three. Runs inside the loop; each iteration is a fresh spawn.
- **Supervisor** — Claude Code subagent. Runs the release-level test plan against each producer iteration, returns pass / fail-with-specifics / escalate, reports back to the coordinator. Fresh context per spawn.
- **Operator** — human. Closes release-level UAT by running the skill against the committed dogfooding projects (March and Nell) and judging resulting artifacts against the mockup scenes in `mockup.md`. Carries the content-quality validation that mechanical evals cannot.

**Reference documentation consulted during `CLAUDE.md` authoring.** Two sources, both Anthropic-published. Anthropic's Claude Code subagent documentation informs the coordinator-and-subagent shape — hub-and-spoke coordination, context isolation, preloaded skills, permission hygiene. Anthropic's `skill-creator` skill informs the producer's working substrate — skill structure, eval format, iteration loop mechanics. Whether these references are absorbed into `CLAUDE.md` and discarded, or remain consulted by the calibrator at Phase 5 alongside `CLAUDE.md`, is an open question routed through R1's execution as the resolution mechanism (see `project-notes.md` → Open → _Calibrator's relationship to Phase 2 reference documentation_). The roadmap commits the references as authoring-time material; their downstream lifespan is deliberately unresolved.

Producer class is per-release. R1's producer is skill-creator-class because R1 ships a skill. A future project's producer might be a coding-agent class, a writing class, or a class that does not yet exist; the reference documentation that informs `CLAUDE.md` for that project would be different. The framework's calibrator must work across producer classes, which is why per-class reference material is per-release rather than framework-shipped.

### Scope exclusions

To prevent scope creep, R1 excludes the following explicitly:

- **Tooling beyond the skill itself.** No IDE integration, no runtime enforcement tools, no wrapper applications. The skill is markdown and structured files read by Claude at session time.
- **Framework version-tracking infrastructure beyond GitHub.** The canonical-state-in-GitHub arrangement committed in `concept.md` stands; R1 does not introduce additional version-management tooling.
- **Generalization beyond what the dogfooding runs stress-test.** R1 ships a skill that has been validated against the specific non-framework projects it was dogfooded against (March and Nell, at minimum). Claims that the skill generalizes to project types not represented in dogfooding are out of scope for R1 and represent risks the framework accepts rather than resolves.

### Definition of R1 done

R1 is complete when all of the following hold:

- The skill exists in the committed directory structure, conforming to Anthropic's skill-authoring methodology per §Binding quality standard as resolved by Phase 3 architecture.
- `[[TEST-PLAN]]` closes per its §Test Plan Closure conditions.
- Any failures surfaced during execution have been either resolved in the skill or logged as accepted limitations in `SKILL.md` or its references, with the logging itself reviewed by the operator.

R1 ships when the above holds. No R2 exists to defer issues into.

## Changelog

- **2026-04-14 — Execution Resourcing corrected; release-level test plan and release schedule authored as sibling artifacts.** Two changes in one amendment, both descending from project-notes entries graduated this week and the prior week's structural correction that release-level artifacts belong to Phase 2.
    
    _Execution Resourcing corrected._ Producer reframed from generic Claude Code subagent to skill-creator-class, with Anthropic's bundled `skill-creator` skill named as the producer's primary reference base. The architectural precedents subsection shrunk from five sources to two: Anthropic's Claude Code subagent documentation and Anthropic's `skill-creator`. The dropped sources (`obra/subagent-driven-development`, `NeoLabHQ/sadd`, and the `hamelsmu` eval trio) were either confirmed irrelevant to a skill-creator-class producer (obra) or never walked through and no longer needed (the others). The "does not travel past `CLAUDE.md`" framing was replaced with an explicit acknowledgment that whether references travel past `CLAUDE.md` is an open question routed through R1's execution as the resolution mechanism. A new paragraph names producer class as per-release rather than framework-wide. _Why:_ the corrections descend from the new `Calibrator references are defaults, not rules` graduated entry, the new `Calibrator's relationship to Phase 2 reference documentation` open entry, and the session's reframing of the producer based on the skill-creator skill being self-contained for R1's needs. _Affects:_ §Execution Resourcing.
    
    _Release-level test plan and release schedule authored._ Two new sibling artifacts created at the project root: `TEST-PLAN.md` and `SCHEDULE.md`. Both are Phase 2 release-activation artifacts per the structural rule that release-level artifacts belong to the phase that defines the release. Filename convention is unscoped because R1 is the only release; the convention for projects with multiple releases would be `TEST-PLAN_R{N}.md` and `SCHEDULE_R{N}.md`. _Test plan contents:_ one outcome (dogfooding artifact quality, judged by the operator against the mockup scenes); mechanism-level checks deliberately not bound at this level, leaving Phase 3 and Phase 5 free to author whatever mechanism gates the build loop benefits from. _Schedule contents:_ two-week window targeting end of April 2026 with daily working sessions; consequences of the tight cadence named explicitly. _Why:_ both artifacts are committed for the first time in this amendment. The roadmap commits R1's boundary and binding constraints; the test plan commits how R1 is validated; the schedule commits when. They are siblings, not subsections, because they have distinct lifecycles — the schedule may slip without the roadmap moving, and the test plan may be amended without the roadmap being re-derived. _Affects:_ new files `TEST-PLAN.md` and `SCHEDULE.md` at project root.
    
    _Downstream:_ `architecture.md` full rewrite is the next contact point. The rewrite descends from the corrected Execution Resourcing roster and the operator-level test plan in `TEST-PLAN.md`; the prior architecture.md draft over-committed at the test plan level and needs to be re-derived against the corrected Phase 2 commitments.