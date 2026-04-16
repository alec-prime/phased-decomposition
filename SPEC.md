---
artifact: spec
phase: 4
project: "[[Phased Decomposition]]"
release: R1
status: approved
created: 2026-04-15
framework_version: 0.1.0
descends_from:
  - "[[ARCHITECTURE]]"
---
# Spec — Phased Decomposition R1

## SKILL.md Body Structure

Target: under 500 lines. The body is a router, not a repository. Every section below is required; order is fixed.

### Sections

| Section | Purpose | Approximate budget |
|---|---|---|
| Frontmatter | `name`, `description` fields per Anthropic convention | ~10 lines |
| Overview | One paragraph. What the skill does, who it's for, when it loads. Theory-of-mind register — explains why the framework exists, not what it is. | ~10 lines |
| Role Definitions | Three universal roles. Each: name, operating question, failure examples (2–3 per role). Inline in body — roles must be in context from session start, not loaded on demand. | ~80 lines |
| Phase Pipeline | Table: phase number, name, terminal artifacts, primary mode. One-line description per phase. Links to reference files. | ~30 lines |
| Routing Table | Trigger patterns → phase reference file. Maps user intent language to phase invocations. | ~20 lines |
| Loading Instructions | How Claude loads reference files when a phase is invoked. One level deep from SKILL.md — no chains. | ~10 lines |

Total body budget: ~160 lines of committed content. Remaining ~340 lines absorbed by elaboration within sections, especially Role Definitions and Routing Table. Producer must not exceed 500 lines total.

### Frontmatter

```yaml
name: building-projects-with-claude
description: <Phase 5 authors against eval trigger-rate optimization>
```

`name` is committed. `description` is a Phase 5 artifact — the spec commits its constraints only:
- Third person
- Names what the skill does and when to use it
- "Pushy" phrasing to counter undertriggering
- Under 1024 characters
- Signals project-shaped work with structure; distinguishes from general Claude usage and lighter-weight planning

---

## Role Definitions — Content Specification

Role definitions in SKILL.md body follow the shape: name → operating question → failure examples. The producer authors prose from this spec. Spec commits the substance; producer commits the register.

### Interrogator

**Operating question:** Is this upstream-grounded, or does it require operator input before production runs?

**What the interrogator does:** Runs before any artifact production in phases 0-5. Checks whether the operator's input resolves the current phase's open commitments. Where upstream artifacts already ground a decision, pulls it forward without asking. Where a gap exists, asks one targeted question in the operator's register — never in production vocabulary.

**Kernel-surfacing protocol (Phase 0 specific):** The interrogator surfaces the kernel through compression, not by naming it. The word "kernel" never appears in operator-facing questions — it is internal framework vocabulary that lives in artifacts, changelogs, and reference files, not in sessions. The operator is never asked to produce "the kernel." They are asked to go one layer deeper on their own words.

The protocol is three moves:

1. **Surface framing check.** After the operator's opening, ask what the project is one layer below its surface description. Not the mechanism, not the name — the load-bearing center. Phrasing examples: "what's this actually about, one layer below the surface," "if someone asked what this is really for, what's the answer."

2. **Why-does-it-matter push.** If the operator's answer names a mechanism or proximate effect, redirect: "why does that matter?" or "what's that in service of?" Each push moves from mechanism toward the load-bearing center. Stop pushing when the answer names what the project is for rather than what it does or how it works.

3. **Compression check.** Ask the operator to compress their answer until it fits as one thought. "Can you say that shorter?" or "short enough to hold as one thought." The kernel is not a sentence — it is a phrase, sometimes a single word.

**Recognition criterion:** The kernel has landed when all three hold: the answer names what the project is for (not what it does or how it works); a "why does it matter" push produces no further movement; the answer fits as one thought. The interrogator does not announce that the kernel has been found. It writes the kernel to the concept document — in framework vocabulary, internally — and moves to the section sweep.

**Failure examples:**
- _Naming the framework term:_ "What's the kernel of this project?" The operator now tries to produce something kernel-shaped rather than reaching the actual load-bearing center. The question anchors on the framework's vocabulary instead of the operator's.
- _Proposal-with-checkback:_ "It sounds like the kernel might be 'reducing friction in knowledge work' — does that land?" The proposal shape is the tell. Even a rejection runs inside Claude's framing; the usual next move is accept-with-adjustment, anchoring on Claude's candidate.
- _Kernel slipped into summary:_ "So it's really a framework for working with LLMs on projects where sessions don't persist context…" stated without flagging it as a kernel claim. The operator reads the summary, finds it accurate on its surface, and does not notice a kernel-level statement has landed as settled context.
- _Stopping at mechanism:_ The operator says "somebody else drives, you just work" and the interrogator accepts it as the kernel. That is the mechanism. The why-does-it-matter push surfaces what the mechanism is in service of — in the March example, progress. The interrogator pushes until the answer is one layer below mechanism, not one layer below surface framing.
- _Production without interrogation:_ Claude begins drafting a concept section before the operator has grounded its substance. The draft reads plausibly but descends from Claude's inference rather than operator intent.
- _Architecture vocabulary in operator-register questions:_ "Do you want an event-driven or polling architecture?" asked directly. The interrogator translates: "When you picture this — is it always watching quietly in the background, or does it wake up when something happens?"

### Scribe

**Operating question:** Does this teach, or is it working on the reader?

**What the scribe does:** Runs at the point of writing prose destined for outward-facing artifacts — `concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`. Renders operator utterances into artifact register. Scope is production output only; in-session conversation runs in whatever register the interrogator needs.

**Voice standard:** Science-communicator register. Artifacts teach rather than convince, demonstrate rather than insist, sound like the project rather than like the operator on the day they were written.

**Failure examples:**
- _Dichotomy-and-verdict opening:_ "Ideas are cheap. Making them real is not. … where most ideas die." Positions the framework as a solution to a failure mode before the reader has context. Rewritten demonstratively: "Most of what it takes to turn an idea into something real happens between the first intuition and the finished thing."
- _Rhetorical load-bearing phrases:_ "shaped to be useful" as a clause doing argumentative work rather than conveying content. Revision removes the stance, not the content.
- _Operator-register leakage:_ Rendering the operator's exact phrasing verbatim when it was thinking-aloud register, not artifact register. The scribe rewrites into the register a cold reader needs without losing the substance.

### Calibrator

**Operating question:** Is this work package sized for the executor who will run it, against the context the executor will have?

**What the calibrator does:** Runs during Phase 5 decomposition. Sizes work packages against per-executor context committed in the release's `CLAUDE.md` and `.claude/agents/` definitions. Does not invent executor context — consults what the release committed.

**Failure examples:**
- _Wrong executor sizing:_ Sizing a work package for a human operator (one Todoist session, ~30–90 minutes) when the executor is a subagent spawn (one atomic operation, bounded context window).
- _Context overload:_ A machine work package that requires the subagent to hold the entire upstream artifact set in working context simultaneously. Decompose until each package requires only what the executor definition preloads.
- _Prior as rule:_ Sizing a coding task at 30 minutes because a reference document suggests that scale, when the live spec commits a tricky integration that wants larger sizing or different decomposition. Live context wins on disagreement with any prior.

---

## Per-Phase Role Application

Each phase reference file commits which roles run, in what order, and what their output for that phase looks like. Spec commits the per-phase application; reference files author the prose.

| Phase | Interrogator                                                                                                                              | Scribe                                                                                   | Calibrator                                                                                                             |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 0     | Runs first. Kernel check before section sweep. Kernel-surfacing protocol applies — never names the framework term.                        | Runs at production. Renders operator utterances into concept/roadmap register.           | Not active.                                                                                                            |
| 1     | Runs first. Checks northstar scenes against concept grounding.                                                                            | Runs at production. Northstar in abstraction register; mockup in instantiation register. | Not active.                                                                                                            |
| 2     | Runs first. Checks release commitments against roadmap grounding.                                                                         | Runs at production. Release artifact in operator-facing register.                        | Not active.                                                                                                            |
| 3     | Runs first. Checks each architectural question against upstream grounding. Translates architecture gaps into operator-register questions. | Not active. Architecture artifacts are inward-facing; scribe register does not apply.    | Not active.                                                                                                            |
| 4     | Runs first. Checks spec commitments against architecture grounding.                                                                       | Not active. Spec is inward-facing.                                                       | Not active.                                                                                                            |
| 5     | Runs first against spec before decomposition begins.                                                                                      | Not active.                                                                              | Runs during decomposition. Sizes every work package against executor definitions in `CLAUDE.md` and `.claude/agents/`. |
| 6     | Not active.                                                                                                                               | Not active.                                                                              | Not active.                                                                                                            |

---

## Asset Template Specifications

### Outward-facing templates

Authored in scribe register. Cold-reader legible. Field guidance below is producer instruction, not body text.

#### `concept-template.md`

**Frontmatter:**
```yaml
artifact: concept
phase: 0
project: "[[<Project Name>]]"
release:
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from: []
```

**Body sections — required:**

| Section | Guidance |
|---|---|
| Introduction | Frames the project from the kernel outward. Teaches a cold reader why this exists. Not a summary — an entry point. Kernel must appear here before any other framing. |
| The Thesis | Compressed statement of what the project is and does. First sentence descends from the kernel. LLM collaboration is the enabling condition, not the subject. |
| The Phases | Pipeline overview. One bullet per phase: name, primary mode, terminal artifacts. |
| Boundaries | In scope / Out of scope / Fixed constraints. Fixed constraints carry a one-line gloss deriving each from the kernel. |
| Success Criteria | Observable conditions. Written so a reader outside the project can evaluate them. |
| Open Questions | Unresolved tensions with resolution mechanism named. Format: question → what it blocks → where the answer comes from. |
| Precedent & Alternatives | Precedent: lineage and what informed this version. Alternatives: what was considered and why set aside. |
| Stakeholders | Who cares, what they care about, what they influence. |
| Changelog | Required once any downstream phase has begun. Format: date — what changed — why — downstream artifacts affected. |

**Field rules:**
- Kernel must appear in Introduction before any other framing. If it doesn't, the interrogator hasn't cleared.
- Changelog entries must name downstream artifacts affected. Entries without downstream impact naming are incomplete.
- No section is drafted from substance Claude had to invent. Sections the operator did not ground are stubbed.

---

#### `northstar-template.md`

**Frontmatter:**
```yaml
artifact: northstar
phase: 1
project: "[[<Project Name>]]"
release:
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[CONCEPT]]"
```

**Body sections — required:**

| Section | Guidance |
|---|---|
| The Felt Experience | One to two paragraphs. The operator's cognitive mode throughout the pipeline. Written at abstraction level — no project names, no filenames. |
| The Claim This Pays Off | Which concept commitment this northstar extends into phenomenological territory. Names the interrogator as the causal mechanism. |
| Scenes | Two to four scenes. Each: setting, how it goes, what it produced, what makes this the scene. Written so a different project could satisfy the same scene description. |
| Re-instantiability | One paragraph. Confirms scenes survive substitution of project, operator, filenames, tools. |

**Field rules:**
- Scenes are at abstraction level. No specific project names, operator names, or filenames inside scene bodies.
- Each scene names which concept claim it pays off.
- Re-instantiability section must follow scenes, not precede them.

---

#### `mockup-template.md`

**Frontmatter:**
```yaml
artifact: mockup
phase: 1
project: "[[<Project Name>]]"
release:
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[NORTHSTAR]]"
```

**Body sections — required:**

One section per northstar scene. Each section:

| Subsection | Guidance |
|---|---|
| Setting | Concrete project, concrete moment, concrete artifacts in play. This is where abstraction collapses into instance. |
| How it goes | Dramatized prose. Operator utterances in blockquotes. Claude responses in blockquotes. Narration explains what Claude is doing and why at decision points. |
| What the session produced | One paragraph. Artifacts written, decisions committed, questions answered. |
| What makes this the scene | One paragraph. Names the pattern the scene dramatizes. The claim the scene earns. |

**Field rules:**
- One mockup scene per northstar scene. Scenes are in the same order.
- Operator utterances and Claude responses in blockquotes, not prose summary.
- Narration at decision points explains the interrogator's logic, not just its output.

---

#### `roadmap-template.md`

**Frontmatter:**
```yaml
artifact: roadmap
phase: 0
project: "[[<Project Name>]]"
release:
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[CONCEPT]]"
```

**Body sections — required:**

| Section | Guidance |
|---|---|
| Shape of This Roadmap | One paragraph. What the roadmap commits and what it does not activate. Points at release artifacts for activation detail. |
| Release Sequence | One bullet per release. Name, one-line description, link to release artifact. |
| Changelog | Same format as concept changelog. |

**Field rules:**
- Roadmap does not activate releases. Activation is Phase 2. Any activation-flavored content (scope, quality standard, definition of done) belongs in the release artifact, not here.
- Single-release projects still have a Release Sequence section — structure is present even when degenerate.

---

#### `release-template.md`

**Frontmatter:**
```yaml
artifact: release
phase: 2
project: "[[<Project Name>]]"
release: <R1 | R2 | ...>
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[ROADMAP]]"
```

**Body sections — required:**

| Section | Guidance |
|---|---|
| Shape of This Release | What this release is within the project sequence. Standalone product framing — not an increment. |
| What This Release Ships | Concrete artifacts and capabilities. Specific enough that a reader can determine whether they are in scope. |
| Binding Quality Standard | Each constraint from the release's external standard mapped to a specific commitment. Phase 3 receives each as an input constraint. |
| Execution Resourcing | Executor roster: name, role, tool access, spawn pattern. References `.claude/agents/` definitions and `CLAUDE.md` for session-start instructions. |
| Scope Exclusions | What is explicitly not in this release. Prevents scope creep. |
| Definition of Done | Observable conditions that close the release. Each condition traceable to a test plan row or an upstream artifact section. |
| Changelog | Same format as concept changelog. |

**Stub sections (links only):**

```
### Schedule

- _See: [[SCHEDULE]]_

### Test Plan

- _See: [[TEST-PLAN]]_
```

---

#### `test-plan-template.md`

**Frontmatter:**
```yaml
artifact: test-plan
phase: 2
project: "[[<Project Name>]]"
release: <R1 | R2 | ...>
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[RELEASE]]"
```

**Body sections — required:**

| Section | Guidance |
|---|---|
| Shape of This Test Plan | One paragraph. What the release is validated against and what it is not. The operator-level outcome dominates mechanism-level checks. |
| The Outcome | Table of test rows (schema below). |
| Test Plan Closure | Conditions under which the plan closes. References row statuses and operator signoff. |
| Changelog | Same format. |

**Table schema:**

```
| id | check | traces_to | status | operator_signoff |
```

| Field | Semantics |
|---|---|
| `id` | `{RELEASE}-TP-{NN}` — e.g. `R1-TP-01`. Sequential within release. |
| `check` | One sentence. Observable condition the operator validates. Written in the register the operator will use when performing it. |
| `traces_to` | Parent test row id. Release-level rows have no parent — field is `—`. Downstream plan rows name the release-level row they descend from. |
| `status` | `pending` \| `passed` \| `failed` \| `accepted-limitation` |
| `operator_signoff` | Empty until operator closes the row. |

**Field rules:**
- Release-level rows describe operator-observable outcomes, not mechanism checks.
- Mechanism checks (trigger rate, routing, reference discovery) belong in the Phase 5 eval suite, not here.
- Every Definition of Done condition in the release artifact maps to at least one test row.

---

### Inward-facing asset authoring instructions

These assets are authored as instructions for Claude, not as structural templates. The producer follows Anthropic's published standards. Spec commits what the instructions must address; producer authors the prose against Anthropic's documentation.

#### `claude-md-template.md`

This asset is a `CLAUDE.md` authoring guide for downstream project coordinators. It is not a template with blanks to fill — it is instructions a downstream project's Phase 2 follows when authoring its own `CLAUDE.md`.

**Must address:**
- What `CLAUDE.md` is for: session-start instructions for the coordinator, scoped to one release. Not a subagent definition — those live in `.claude/agents/`
- Walls: must be set before any spawning begins. What walls are, why they exist, that the minimum set is release-specific and committed in the release's architecture
- Operator touchpoints: the release's architecture commits how many and what kind. `CLAUDE.md` makes them explicit so the coordinator does not prompt outside them
- Escalation handling: what the coordinator surfaces to the operator, what it waits for before resuming
- Credential dependencies: enumerated in this release's `spec.md` § Execution Configuration. The coordinator confirms each field with the operator at session start. Fields with no default are blocking — coordinator does not spawn until they are confirmed
- Locus rule: instructions governing skill-execution-time Claude do not belong in `CLAUDE.md`. `CLAUDE.md` governs the coordinator; skill content governs downstream project instances

**Must not prescribe:**
- Section structure — follows from what the release's coordinator actually does
- Executor roster shape — follows from the release's execution resourcing
- Loop mechanics — follow from the release's architecture

**Source:** Anthropic's Claude Code documentation. Producer consults at authoring time. R1's `CLAUDE.md` at the project root is one instance — the template instructions abstract the pattern, not the instance.

---

#### `subagent-template.md`

This asset is a subagent definition authoring guide for downstream project Phase 2. Instructions for authoring `.claude/agents/{name}.md` files.

**Must address:**
- YAML frontmatter fields: `name`, `description`, `tools`, `model`, `skills` — field semantics and valid values per Anthropic's standard
- `description` field: the mechanism by which the coordinator invokes the subagent. Must be specific enough to distinguish the subagent from others in the same project
- `tools` field: principle of least privilege — subagent receives only the tools its work requires
- `model` field: `inherit` defers to coordinator; explicit tier follows the release's architecture decision on model-tier targeting. The template does not prescribe a tier
- System prompt conventions: role statement, substrate section (what the subagent has access to), operating procedure, working rules, explicit what-you-do-not-do section
- Locus test: instructions governing skill-execution-time Claude belong in the skill, not in build-time subagent definitions
- Memory and state: whether subagents are fresh-spawn or stateful is a release architecture decision. Any state needing to persist across spawns must be written to files — whether cross-package memory is exercised follows from the release's execution resourcing

**Must not prescribe:**
- Spawn pattern (fresh vs. stateful) — release architecture commits this
- Role-specific operating procedures — follow from what the release's subagent actually does

**Source:** Anthropic's Claude Code subagent documentation. Producer consults at authoring time.

---

#### `architecture-template.md`

This asset is an architecture document authoring guide for downstream project Phase 3. Instructions for a Claude instance running Phase 3 on a downstream project.

**Must address:**
- Purpose: stress-test the release against its binding quality standard, resolve structural questions, commit to an approach. Architecture receives each binding constraint from the release artifact and resolves it to a specific structural commitment
- Inward-facing register: architecture artifacts are consumed by build-time Claude, not operators. Structured fields and tables where substance allows; explanatory prose only where it calibrates judgment the consumer needs to make
- Descent from release: every binding quality standard constraint maps to a specific architecture commitment. Mapping must be auditable — a reader checking each release constraint finds the section and paragraph that resolves it
- Open questions format: question → resolution mechanism naming which phase closes it. Architecture does not leave questions open without naming who resolves them
- Out of scope section: what the architecture explicitly does not decide, with the downstream phase that decides each thing named
- Phase 3 closes when: all release constraints mapped, open questions logged with resolution phase named, status flipped to approved after operator review

**Must not prescribe:**
- What the structural commitments are — those follow from the release's binding quality standard
- Directory structures, topology rules, loop mechanics, tier targeting — all release-specific
- Section structure beyond the four-part rhythm: opener, substance, bounds-and-gaps, terminal

**Source:** This spec, `[[RELEASE]]` § Binding Quality Standard, `[[ARCHITECTURE]]` as the canonical instance of the pattern — not as the prescribed structure.

---

#### `spec-template.md`

This asset is a spec document authoring guide for downstream project Phase 4. Instructions for a Claude instance running Phase 4 on a downstream project.

**Must address:**
- Purpose: package architecture into executable form. Spec receives every architectural commitment and specifies it to the level of detail the producer needs to author correctly — structure, field semantics, conventions, failure modes. Stops before authoring the artifacts themselves
- Inward-facing register: same as architecture
- Descent from architecture: every architectural commitment maps to a spec entry. No spec content originates outside architecture. A reader checking each architecture section finds the spec section that packages it
- Asset coverage: spec commits specifications for every asset the release's architecture committed. What those assets are follows from the architecture — the template does not prescribe them
- Execution Configuration section: required in every spec. Commits runtime inputs the coordinator confirms at session start. Schema: field name, type (prompted), default if any, notes. Fields with no default are blocking
- Phase 5 terminal artifact structure: spec commits what Phase 5 produces with enough schema and field-level detail for the calibrator to size work packages against it
- Phase 4 closes when: architecture fully packaged into spec entries, status flipped to approved after operator review

**Must not prescribe:**
- What the assets are — follows from the release's architecture
- Section structure beyond the four-part rhythm

**Source:** This spec as the canonical instance, `[[ARCHITECTURE]]` as upstream, `[[RELEASE]]` § Binding Quality Standard as the constraint chain's origin.

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
- Supervisor gating: supervisor agent runs the eval suite against producer output and returns a verdict (advance / rework / escalate) per § Build Loop Mechanics. Build-time agent defined by the release's `.claude/agents/` files.
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

## Build Loop Mechanics

The Phase 6 loop executes per-unit work Phase 5 hands over. Unit shape (queue, tree, batch) is committed by Phase 5; the loop mechanics are constant across shapes.

**Loop structure:**

```
coordinator delivers unit → producer drafts/refines → supervisor evaluates → verdict
```

**Verdicts — three options, coordinator routes per:**

| Verdict | Coordinator action |
|---|---|
| `advance` | Mark unit closed, move on per Phase 5's structure |
| `rework` | Respawn producer with supervisor's failure characterization as input; increment iteration count |
| `escalate` | Halt work on the unit, package for operator per § Escalation Routing |

**Eval execution during loop:**

| Eval category | Run cadence |
|---|---|
| Mechanical checks | Every iteration |
| Held-out trigger-rate checks | When description or routing changes |

Held-out cases gate `advance`. Threshold committed in Phase 5 against the optimized description.

**Termination — two conditions required for Phase 6 to close:**

1. Mechanical evals pass at Phase 5's threshold on held-out cases
2. Release-level test plan closes through operator judgment per `[[TEST-PLAN]]`

**Coordinator constraints:**

- Does not interpret verdicts
- Does not override supervisor judgments
- Does not spawn against UAT work — handoff errors flag to operator
- Does not adapt loop mid-execution — adaptation is an escalation

---

## Escalation Routing

When the supervisor returns `escalate`, the coordinator halts and packages the escalation for the operator.

**Escalation conditions — supervisor escalates when any hold:**

- Iteration bound hit (operator-set per session)
- Characterized failure recurs without progress toward resolution
- Standard itself under-resolved — supervisor cannot form verdict without inventing criteria
- Producer output satisfies standard, but running it reveals an upstream artifact gap

**Upstream phase routing — supervisor names the candidate:**

| Failure characterization | Upstream phase |
|---|---|
| Eval or decomposition was wrong | Phase 5 |
| Spec was wrong | Phase 4 |
| Architecture was wrong | Phase 3 |
| Binding constraint in release artifact was wrong | Phase 2 |

**Escalation package contents:**

- Supervisor's failure characterization
- Candidate upstream phase
- Unit of work that triggered the escalation
- Iteration count at escalation

**Operator action on escalation:**

Operator decides which artifact gets amended and whether work resumes. Amendment produces a changelog entry on the amended artifact naming the trigger and affected downstream artifacts. Downstream artifacts re-derive from the amendment.

Escalation is the framework working as designed — a build-time signal surfacing that an upstream artifact needs amendment, routed to the only role empowered to amend it.

---

## Session Walls

Walls bound autonomous coordinator operation. Operator sets walls at session start; coordinator does not spawn until walls are confirmed.

**Minimum required walls:**

| Wall | Purpose |
|---|---|
| Iteration bound per unit of work | Catches runaway loops independent of failure shape |
| Session length bound | Returns to operator for review before session runs indefinitely |
| Escalation mode | `halt-on-escalation` (stop, surface, wait) or `queue-and-continue` (park escalated unit, continue, surface at session end); default `halt-on-escalation` |

**Additional walls are operator-discretionary per session.**

**Wall confirmation:**

- Confirmed explicitly at session start
- Prior-session values are not assumed — every session starts with fresh wall confirmation
- Wall hits trigger operator touchpoint: coordinator reports state (closed, pending, escalated) and waits for direction

---

## Test Plan Layers

Two layers, parallel rather than parent-child.

| Layer | Owner | Gate | Mechanically checkable | Traces upward to |
|---|---|---|---|---|
| Release validation | `[[TEST-PLAN]]` | Closes the release through operator judgment | No | Release Definition of Done |
| Build-loop gating | Phase 5 eval suite | Closes individual iterations through supervisor verdict | Yes | This spec § Role Definitions failure examples |

**Layer independence:** Eval suite entries do not trace to `[[TEST-PLAN]]` rows. The two layers check different properties against different gates at different times.

**Spec-level test plan placement:** Between layers. Spec-level rows (§ Spec-Level Test Plan) check reference-file and asset content correctness. They gate Phase 4 closure, not release validation and not build-loop iteration.

**Eval suite categories — Phase 5 commits specific entries:**

| Category                       | What it checks                                                                                                                          |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| Description trigger rate       | Held-out queries that should invoke the skill do; queries that should not, do not                                                       |
| Phase routing                  | Given a phase-mapped request, Claude reaches the correct phase reference file before producing                                          |
| Reference-file discovery       | Claude follows reference invocations from phase guides without user prompting                                                           |
| Interrogator-before-production | In phases where the interrogator runs (0–5), Claude runs the phase's interrogator before drafting any artifact content                  |
| Kernel-surfacing protocol      | Given a Phase 0 cold start, Claude resists proposing a kernel candidate; failure modes from § Interrogator serve as negative test cases |

---

## Reference Topology and Directory Constraints

**Topology — one level deep:**

Every file in `references/` is invoked directly from `SKILL.md`. No reference file points at another reference file as a continuation.

The constraint is on chains, not mentions. Phase guides may name role files, other phase files, or asset files as context without that constituting a chain. A chain exists when reading file A directs Claude to read file B as a continuation of A's instructions, requiring B to be loaded to complete A's work.

**Directory categorization:**

| Directory | Contents | Read pattern | Producer authors? |
|---|---|---|---|
| `references/` | Files Claude reads to learn how to do work | Loaded into context on phase invocation | Yes, fresh against source artifacts |
| `assets/` | Files Claude stamps into a project as operator substrate | Copied to project directory, never loaded as instructions | Yes, fresh against template specifications |

**Directory-scoping rule:** Templates that get stamped live in `assets/`. Instructions that get loaded live in `references/`. `subagent-template.md` and `claude-md-template.md` live in `assets/` because Phase 2 of a downstream project stamps them, not because they instruct Claude during the stamping project's runtime.

**Omitted directories:**

- `scripts/` — not shipped. Framework procedures are judgment-heavy. Phase 5 may add the directory if a script need surfaces
- `evals/` — not shipped in the distributable skill. Evals are build-time material living at project root during the build

---

## Model-Tier Targeting

Skill content is authored to serve the model tier running each phase. Tier assignment per phase:

| Phase | Tier |
|---|---|
| 0, 1, 2, 5 | Opus |
| 3, 4 | Sonnet |
| 6 | Per-release in `CLAUDE.md` |

**Producer implication:** Reference-file voice calibrates to the tier running the phase. Opus-targeted reference files may assume more inference; Sonnet-targeted reference files commit more explicit structure. Phase 6 tier follows from the release's subagent definitions — the producer authors `phase-6-execution.md` at a tier-agnostic register and lets per-release `CLAUDE.md` commit the specific tiers.

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

## Execution Configuration

Runtime inputs the coordinator confirms with the operator at session start. No spawning begins until all fields with no default are confirmed.

| Field | Type | Default | Notes |
|---|---|---|---|
| Todoist project name | Prompted at session start | "Personal" | Used by Todoist load operation to set task destination |
| Todoist API key | Prompted at session start | — | No default; operator must provision before Phase 6 |

---

## Phase 5 Terminal Artifact Structure

Phase 5 produces three outputs from one underlying decomposition tree. The operator reviews at a dual-artifact gate covering `work-instructions.md` and `execution-plan.json` together. Gate approval triggers the Todoist load operation and Phase 6.

### `work-instructions.md`

Operator-facing. Full decomposition tree in readable form. The artifact the operator reviews at the Phase 5 gate.

**Frontmatter:**
```yaml
artifact: work-instructions
phase: 5
project: "[[<Project Name>]]"
release: <R1 | R2 | ...>
status: draft
created: <ISO timestamp>
framework_version: <version>
descends_from:
  - "[[SPEC]]"
```

**Body sections — required:**

| Section | Guidance |
|---|---|
| Shape of This Plan | What the plan covers, what executor types are present, what the gate requires. |
| Task List | One subsection per task. Each task: id, description, executor type, ordered work packages with one-line descriptions. Enough detail for the operator to approve ordering, sizing, and coverage. |
| Sequencing Notes | Dependencies between tasks that constrain ordering. Explicit; no implicit ordering. |
| Gate Conditions | What the operator approves at review. Both artifacts (this doc + `execution-plan.json`) must pass before Phase 6 begins. |

---

### `execution-plan.json`

Machine-readable. The coordinator reads this to operate Phase 6. Authored in parallel with `work-instructions.md` from the same decomposition tree.

**Schema:**

```json
{
  "release": "R1",
  "project": "<project name>",
  "tasks": [
    {
      "id": "T-01",
      "description": "<task description>",
      "work_packages": [
        {
          "id": "WP-01-01",
          "type": "machine | human",
          "executor": "<subagent name> | human",
          "description": "<work package description>",
          "file": "work-packages/<id>.md",
          "dependencies": ["<WP-id>"],
          "iteration_bound": "<n — machine only>",
          "test_plan_entries": ["<TP-id>"]
        }
      ]
    }
  ]
}
```

**Field rules:**
- `id` format: tasks are `T-{NN}`, work packages are `WP-{task-nn}-{package-nn}`
- `type` determines downstream routing: `machine` → coordinator spawns producer against the file; `human` → Todoist load reads the file for task text
- `file` is the path to the work package file at the project root
- `dependencies` lists work package ids that must close before this package begins. Empty array if none
- `iteration_bound` is required for machine packages; omitted for human packages
- `test_plan_entries` lists the test plan row ids this work package pays off

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

| Section | Guidance |
|---|---|
| Work Package Scope | One paragraph. What this package produces. Observable at closure. |
| Inputs | Upstream artifacts the executor consults. Files, sections, specific upstream commitments. |
| Acceptance Criteria | Observable conditions that satisfy the package. Each criterion traces to a test plan row by id. |
| Test Plan Entries | Table with schema `id \| check \| traces_to \| status`. Rows are the specific checks the supervisor (machine) or operator (human) runs. |

**Additional sections — machine packages only:**

| Section | Guidance |
|---|---|
| Supervisor Standard | The standard the supervisor gates iterations against. Descends from Acceptance Criteria; makes the gating explicit. |
| Escalation Conditions | When the supervisor escalates instead of returning rework. Draws from § Escalation Routing; names specific conditions for this package. |

**Additional sections — human packages only:**

| Section | Guidance |
|---|---|
| Verification Steps | Ordered steps the operator performs to confirm the package closed. Each step observable. |

**Field rules:**
- Every acceptance criterion traces to a test plan row
- Machine packages commit iteration bound in frontmatter; human packages omit the field
- Work package files are read-only to the producer; status flips are written to `execution-plan.json`, not to the file
- Dependencies between packages are committed in `execution-plan.json`, not duplicated in work package files

---

### Todoist load operation

Triggered after operator approves both Phase 5 artifacts at the gate. Operation does not begin until gate approval is confirmed by the operator in the session.

**Configuration — confirmed at session start:**

| Field | Default | Notes |
|---|---|---|
| Todoist project name | "Personal" | No default; operator confirms before operation runs |
| Todoist API key | — | Blocking; coordinator does not proceed until confirmed |

**Phase 5 produces `todoist-tasks.json`:**

```json
[
  {
    "title": "[<PROJECT-NAME>] <one-line task name>",
    "project": "<confirmed at session start>"
  }
]
```

**Steps:**
1. Confirm configuration with operator at session start
2. Read `todoist-tasks.json`
3. Post each entry to Todoist REST API using confirmed API key

---

## Cross-Reference Conventions

Two mechanisms. Both required on every artifact.

**Frontmatter lineage:** `descends_from` names upstream artifacts. Structural, machine-readable.

**First-instance links:** First bare-prose occurrence of another artifact's name is wrapped in an Obsidian wikilink aliased to preserve surface grammar. Subsequent mentions unlinked. Self-references excluded.

Aliasing examples:
- `"The northstar extends…"` → `"The [[NORTHSTAR|northstar]] extends…"`
- `"The roadmap's remaining job…"` → `"The [[ROADMAP|roadmap]]'s remaining job…"` (`'s` outside the link)
- Filename references inside code spans (`` `northstar.md` ``) are not linked; first bare-prose occurrence gets the link even if a code-span occurrence appears earlier

**Parent-child stub sections:** When a parent artifact owns navigation for a child (e.g. release → test plan, release → schedule), the parent gets a stub section:

```
### Test Plan

- _See: [[TEST-PLAN|Test Plan]]_
```

Other prose mentions of the child in the parent stay unlinked — the stub is the canonical link site.

**Consistency rule:** Every artifact named in `descends_from` appears at least once in the body as a first-instance link. A body link pointing to an artifact not in `descends_from` is a signal — either frontmatter is incomplete or the reference is incidental.

---
## Spec-Level Test Plan

Spec-level rows gate individual reference-file and asset correctness. Each row traces to the architecture section or spec section that commits the standard being checked. Mechanism checks (trigger rate, routing, interrogator-before-production) belong to the Phase 5 eval suite, not here.

| id       | check                                                                                                                                                | traces_to                                                               | status  | operator_signoff |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ------- | ---------------- |
| S1-TP-01 | `phase-0-concept-roadmap.md` correctly implements the interrogator kernel-surfacing protocol including all three moves and the recognition criterion | `SPEC.md § Interrogator`                                                | pending |                  |
| S1-TP-02 | `phase-0-concept-roadmap.md` correctly implements the section sweep pattern — interrogator runs before production, ungrounded sections stubbed       | `SPEC.md § Interrogator`                                                | pending |                  |
| S1-TP-03 | `phase-1-northstar-mockup.md` correctly implements the abstraction-vs-instantiation split between northstar and mockup                               | `SPEC.md § Per-Phase Role Application`                                  | pending |                  |
| S1-TP-04 | `phase-2-release-activation.md` correctly implements release artifact descent from roadmap and the activation-vs-mapping distinction                 | `SPEC.md § Per-Phase Role Application`                                  | pending |                  |
| S1-TP-05 | `phase-3-architecture.md` correctly implements the inward-facing register constraint and the interrogator translation pattern                        | `SPEC.md § Per-Phase Role Application`                                  | pending |                  |
| S1-TP-06 | `phase-4-spec.md` correctly implements the architecture-to-spec packaging pattern and the locus rule                                                 | `SPEC.md § Per-Phase Role Application`                                  | pending |                  |
| S1-TP-07 | `phase-5-execution-planning.md` correctly implements calibrator sizing against executor definitions and the dual-artifact gate                       | `SPEC.md § Calibrator`, `SPEC.md § Phase 5 Terminal Artifact Structure` | pending |                  |
| S1-TP-08 | `phase-6-execution.md` correctly implements the supervisor gating pattern, coordinator escalation routing, and the release-level UAT boundary        | `SPEC.md § Per-Phase Role Application`                                  | pending |                  |
| S1-TP-09 | Role definitions in `SKILL.md` body conform to operating-question + failure-examples shape for all three roles                                       | `SPEC.md § Role Definitions`                                            | pending |                  |
| S1-TP-10 | All six outward-facing asset templates include required frontmatter fields and required body sections per spec                                       | `SPEC.md § Asset Template Specifications`                               | pending |                  |
| S1-TP-11 | All four inward-facing asset authoring instructions address required topics and omit prescribed-but-excluded content per spec                        | `SPEC.md § Asset Template Specifications`                               | pending |                  |
| S1-TP-12 | Cross-reference conventions are applied correctly across all produced artifacts — first-instance links, frontmatter lineage, stub sections           | `SPEC.md § Cross-Reference Conventions`                                 | pending |                  |

### Test Plan Closure

The spec-level test plan closes when every row is `passed` or `accepted-limitation` with operator signoff, and Phase 5 begins. Failures route to rework of the relevant reference file or asset; escalations route to spec amendment.

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

## Changelog

- **2026-04-15 — Phase 4 gate closure amendment.** Added seven new top-level sections packaging architectural commitments that had not previously been spec-committed: § Reference File Specifications (covers all seven phase reference files with must-address / must-not-prescribe / source per file), § Build Loop Mechanics, § Escalation Routing, § Session Walls, § Test Plan Layers, § Reference Topology and Directory Constraints, § Model-Tier Targeting, § Voice Constraint on Producer Output. Added § Work Package File Schema as a subsection of § Phase 5 Terminal Artifact Structure committing frontmatter, body sections, and field rules for `work-packages/<id>.md` files. Expanded § Phase 4 Closes When to cover the new sections. Status reverted from `approved` to `draft` pending operator review of the amendment. Release-level UAT boundary in `phase-6-execution.md` reference file spec generalized from R1-specific "dogfooding" language to universal "UAT" language — dogfooding is R1's UAT instantiation, not a framework-level concept. _Why:_ Phase 4 gate check found four architectural commitments unpackaged (reference topology, model-tier targeting, escalation routing, session walls), four partially packaged (file categorization, build loop, test plan layers, voice constraint), zero reference file content specifications, and no work package file schema. The gate required all gaps closed before Phase 5. _Affects:_ Reference file authoring in Phase 6 (now has explicit must-address lists per file), Phase 5 decomposition (now has work package file schema to decompose against), spec-level test plan rows S1-TP-01 through S1-TP-08 (now have content specifications to check against).

- **2026-04-15 — Initial authoring.** Descends from `[[ARCHITECTURE]]`. Covers SKILL.md body structure, per-phase role application with operating questions and failure examples for all three universal roles including the kernel-surfacing protocol and recognition criterion for the interrogator, asset template specifications for all ten assets in `assets/` (six structural templates, four authoring-instruction assets with instance-specific content removed), Execution Configuration section with Todoist fields, Phase 5 terminal artifact structure (`work-instructions.md`, `execution-plan.json`, work package template, Todoist load operation), and cross-reference conventions. Open questions section dropped — Phase 5 deliverables belong in Phase 5; architecture open questions belong in architecture.
