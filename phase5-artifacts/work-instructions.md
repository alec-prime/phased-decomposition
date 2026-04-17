---
artifact: work-instructions
phase: 5
project: "[[Phased Decomposition]]"
release: R1
status: draft
created: 2026-04-16
framework_version: 0.1.0
descends_from:
  - "[[SPEC]]"
---
# Work Instructions — Phased Decomposition R1

## Shape of This Plan

R1 ships one skill with 18 files: `SKILL.md`, 7 reference files, 10 asset templates. Plus a build-time eval suite at `evals/evals.json`. The plan decomposes into 8 tasks containing 23 work packages — 22 machine packages (producer/supervisor loop) and 1 human package (Todoist load). The operator reviews this document and `execution-plan.json` together at the Phase 5 gate. Approval triggers Phase 6.

Executor types present:

- **Producer** — Claude Code subagent, skill-creator-class. Authors skill content. Fresh spawn per iteration.
- **Supervisor** — Claude Code subagent. Gates each producer iteration against the eval suite and work-package-level acceptance criteria. Fresh spawn per iteration.
- **Coordinator** — main Claude Code session. Runs the producer↔supervisor loop per `CLAUDE.md`.
- **Human** — operator. One package: Todoist load confirmation post-gate.

## Task List

### T-01: Eval Suite Authoring

The eval suite is Phase 5's terminal artifact alongside this plan. It gates every Phase 6 iteration. Authored before skill content so the supervisor has a standard to gate against from the first producer spawn.

**Work packages:**

1. **WP-01-01: Core behavioral evals** (machine, producer)
   Author eval entries for four categories: phase routing, reference-file discovery, interrogator-before-production, and kernel-surfacing protocol. Each category gets positive cases (prompts that should produce the behavior) and negative cases (prompts that should trigger failure modes from `SPEC.md § Interrogator` and `§ Scribe`). Format per skill-creator's `evals/evals.json` schema.

2. **WP-01-02: Description trigger-rate evals** (machine, producer)
   Author 20 trigger-rate eval queries — mix of should-trigger and should-not-trigger — per skill-creator's description optimization format. Queries must be realistic, concrete, edge-case-heavy per skill-creator guidance. Saved separately for the description optimization loop in T-06.

### T-02: SKILL.md Body

The root document. Routes to everything else. Under 500 lines.

**Work packages:**

3. **WP-02-01: SKILL.md body** (machine, producer)
   Author the complete SKILL.md body: frontmatter (name committed, description placeholder), overview paragraph, three role definitions (interrogator with kernel-surfacing protocol, scribe, calibrator — each with operating question and failure examples), phase pipeline table with reference-file links, routing table mapping user intent to phase invocations, loading instructions. Theory-of-mind voice throughout. Target ~160 lines of committed content, well under 500 total.

### T-03: Phase Reference Files

Seven files. Authoring order follows pipeline sequence. Each is independently specifiable but phase-0 establishes patterns later files echo. No dependency chains between files.

**Work packages:**

4. **WP-03-01: phase-0-concept.md** (machine, producer)
   Kernel-surfacing protocol (three moves + recognition criterion), section-sweep pattern, ungrounded-section handling, scribe register for concept authoring, concept template field rules. Must-address and must-not-prescribe per `SPEC.md § Reference File Specifications`.

5. **WP-03-02: phase-1-northstar-mockup-roadmap.md** (machine, producer)
   Abstraction-vs-instantiation split, medium-follows-surface rule, scene-to-scene correspondence, re-instantiability test, descent from concept, roadmap as Phase 1 sibling. Must-address and must-not-prescribe per spec.

6. **WP-03-03: phase-2-release-activation.md** (machine, producer)
   Phase 2 purpose (committing release artifacts), release as standalone product, binding quality standard as Phase 3 input, release-level test plan as operator-observable outcomes, release schedule as target, executor roster pointing at `.claude/agents/` and `CLAUDE.md`, locus rule. Must-address and must-not-prescribe per spec.

7. **WP-03-04: phase-3-architecture.md** (machine, producer)
   Phase 3 purpose (stress-test against binding standard), inward-facing register, interrogator translation pattern, descent from release, open questions format, out-of-scope section. Must-address and must-not-prescribe per spec.

8. **WP-03-05: phase-4-spec.md** (machine, producer)
   Phase 4 purpose (package architecture into executable form), inward-facing register, descent from architecture, asset and reference coverage, Execution Configuration section, spec-level test plan, Phase 5 terminal artifact structure specification. Must-address and must-not-prescribe per spec.

9. **WP-03-06: phase-5-execution-planning.md** (machine, producer)
   Phase 5 purpose (decompose + author eval suite), calibrator role, dual-artifact gate, eval suite as terminal artifact, work package file authoring, Todoist load operation. Must-address and must-not-prescribe per spec.

10. **WP-03-07: phase-6-execution.md** (machine, producer)
    Phase 6 purpose (pure execution), supervisor gating, verdict routing, escalation routing, session walls, release-level UAT boundary, operator touchpoints. Must-address and must-not-prescribe per spec.

### T-04: Outward-Facing Asset Templates

Six structural templates stamped into downstream projects. Each independently specifiable. Authored in scribe register — these are operator-facing substrate.

**Work packages:**

11. **WP-04-01: concept-template.md** (machine, producer)
    Frontmatter schema, nine required body sections with field guidance, field rules (kernel in Introduction, changelog naming downstream artifacts, no ungrounded sections drafted). Per `SPEC.md § Asset Template Specifications`.

12. **WP-04-02: northstar-template.md** (machine, producer)
    Frontmatter schema, four required body sections, field rules (abstraction level, concept claim per scene, re-instantiability follows scenes). Per spec.

13. **WP-04-03: mockup-template.md** (machine, producer)
    Frontmatter schema, one section per northstar scene with four subsections each, field rules (scene-to-scene correspondence, blockquote dialogue, narration at decision points). Per spec.

14. **WP-04-04: roadmap-template.md** (machine, producer)
    Frontmatter schema, three required body sections, field rules (no activation content, degenerate single-release structure present). Per spec.

15. **WP-04-05: release-template.md** (machine, producer)
    Frontmatter schema, seven required body sections plus two stub sections, field rules. Per spec.

16. **WP-04-06: test-plan-template.md** (machine, producer)
    Frontmatter schema, three required body sections, table schema (id, check, traces_to, status, operator_signoff), field rules (operator-observable outcomes, mechanism checks belong in Phase 5 eval suite). Per spec.

### T-05: Inward-Facing Asset Authoring Instructions

Four authoring guides. These are instructions for downstream Claude instances, not structural templates. Theory-of-mind voice. Each independently specifiable.

**Work packages:**

17. **WP-05-01: claude-md-template.md** (machine, producer)
    Authoring guide for downstream `CLAUDE.md`. Must address: purpose, walls, operator touchpoints, escalation handling, credential dependencies, locus rule. Must not prescribe: section structure, executor roster shape, loop mechanics. Per spec.

18. **WP-05-02: subagent-template.md** (machine, producer)
    Authoring guide for downstream `.claude/agents/` files. Must address: YAML frontmatter fields, description as invocation mechanism, tools (least privilege), model field, system prompt conventions, locus test, memory and state. Must not prescribe: spawn pattern, role-specific procedures. Per spec.

19. **WP-05-03: architecture-template.md** (machine, producer)
    Authoring guide for downstream `architecture.md`. Must address: purpose, inward-facing register, descent from release, open questions format, out-of-scope section, Phase 3 closes when. Must not prescribe: structural commitments, directory structures, section structure beyond four-part rhythm. Per spec.

20. **WP-05-04: spec-template.md** (machine, producer)
    Authoring guide for downstream `spec.md`. Must address: purpose, inward-facing register, descent from architecture, asset coverage, Execution Configuration section, Phase 5 terminal artifact structure, Phase 4 closes when. Must not prescribe: what the assets are, section structure beyond four-part rhythm. Per spec.

### T-06: Description Optimization

Runs after SKILL.md body exists. Uses the trigger-rate evals from WP-01-02 and skill-creator's `run_loop.py`.

**Work packages:**

21. **WP-06-01: Description optimization loop** (machine, producer)
    Run skill-creator's description optimization against the trigger-rate eval set. Input: current SKILL.md with placeholder description + trigger-rate evals. Output: optimized description meeting spec constraints (third person, pushy, under 1024 chars). Updates SKILL.md frontmatter with `best_description`.

### T-08: Cross-Reference Conventions Pass

Final holistic pass after all skill files are authored. Cross-cutting concern that applies across every produced file.

**Work packages:**

22. **WP-08-01: Cross-reference conventions pass** (machine, producer)
    Review and correct cross-reference conventions across all produced skill files — first-instance wikilinks, frontmatter lineage, stub sections, aliasing. Per `SPEC.md § Cross-Reference Conventions`.

### T-07: Todoist Load

Post-gate. Human-executed.

**Work packages:**

23. **WP-07-01: Todoist load confirmation** (human)
    Confirm Todoist project name at session start. Verify `todoist-tasks.json` entries match approved plan. Coordinator posts entries to Todoist REST API using confirmed API key. Operator verifies tasks appear in Todoist.

## Sequencing Notes

- **T-01 before all other tasks.** The eval suite must exist before any producer spawn so the supervisor has a gating standard from iteration one.
- **T-02 before T-03.** SKILL.md body establishes role definitions and the routing table that reference files are loaded from. Reference files can be authored without SKILL.md existing on disk (they don't chain from it), but the producer needs the body's role definitions and routing table as context to author reference files that assume them correctly.
- **T-03 before T-04 and T-05.** Reference files define what each phase does; asset templates and authoring instructions must be consistent with them. Authoring assets before reference files risks content that contradicts what the reference file will eventually say.
- **Within T-03:** WP-03-01 (phase-0) first. It establishes the kernel-surfacing protocol and interrogator patterns that later phase files reference by name. Remaining phases in pipeline order: 03-02 through 03-07.
- **T-04 and T-05 are parallel.** No dependencies between outward-facing templates and inward-facing authoring instructions. Both depend on T-03 completion.
- **T-06 after T-02.** Description optimization requires the SKILL.md body to exist. Can run in parallel with T-04/T-05 since it only touches SKILL.md frontmatter.
- **T-08 after T-02 through T-06.** Cross-reference conventions pass requires all skill files to exist. Depends on completion of T-02, T-03, T-04, T-05, and T-06.
- **T-07 after gate approval.** Todoist load is post-gate; it does not begin until the operator approves both Phase 5 artifacts.

## Gate Conditions

The operator approves at this gate by reviewing both artifacts together:

1. **This document (`work-instructions.md`):** task decomposition covers every file in the committed skill directory; sequencing respects dependencies; work package sizing fits single-spawn producer context.
2. **`execution-plan.json`:** machine-readable representation matches this document; every work package has a file at `work-packages/<id>.md`; schema conforms to spec.

Both artifacts must pass before Phase 6 begins. Gate approval triggers the Todoist load operation (T-07) and coordinator startup.
