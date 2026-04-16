---
artifact: spec
phase: 4
project: "[[Phased Decomposition]]"
release: R1
status: in-review
created: 2026-04-15
framework_version: 0.1.0
descends_from:
  - "[[ARCHITECTURE]]"
---
# Spec — Phased Decomposition R1

## Shape of This Spec

Phase 4 packages architecture into executable form. This document commits:
- `SKILL.md` body structure and section layout
- Per-phase role application (interrogator, scribe, calibrator) with operating questions and failure examples
- Asset template specifications — structure and field-level guidance sufficient for the producer to author each correctly
- Phase 5 terminal artifact structure — `work-instructions.md`, `execution-plan.json`, work package files, and Todoist load operation
- Execution configuration — runtime inputs the coordinator confirms with the operator at session start

Phase 4 stops before: prose authoring of reference files, eval entry authoring (Phase 5), description field optimization (Phase 5).

---

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

**What the interrogator does:** Runs before any artifact production in every phase. Checks whether the operator's input resolves the current phase's open commitments. Where upstream artifacts already ground a decision, pulls it forward without asking. Where a gap exists, asks one targeted question in the operator's register — never in production vocabulary.

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

| Phase | Interrogator | Scribe | Calibrator |
|---|---|---|---|
| 0 | Runs first. Kernel check before section sweep. Kernel-surfacing protocol applies — never names the framework term. | Runs at production. Renders operator utterances into concept/roadmap register. | Not active. |
| 1 | Runs first. Checks northstar scenes against concept grounding. | Runs at production. Northstar in abstraction register; mockup in instantiation register. | Not active. |
| 2 | Runs first. Checks release commitments against roadmap grounding. | Runs at production. Release artifact in operator-facing register. | Not active. |
| 3 | Runs first. Checks each architectural question against upstream grounding. Translates architecture gaps into operator-register questions. | Not active. Architecture artifacts are inward-facing; scribe register does not apply. | Not active. |
| 4 | Runs first. Checks spec commitments against architecture grounding. | Not active. Spec is inward-facing. | Not active. |
| 5 | Runs first against spec before decomposition begins. | Not active. | Runs during decomposition. Sizes every work package against executor definitions in `CLAUDE.md` and `.claude/agents/`. |
| 6 | Not active in coordinator. Active in supervisor — checks each producer iteration against work package test plan before advance verdict. | Not active. | Not active. |

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

## Phase 4 Closes When

- Every architectural commitment in `[[ARCHITECTURE]]` maps to a spec entry. Auditable: a reader checking each architecture section finds the spec section that packages it.
- Asset template specifications cover every file in `[[ARCHITECTURE]]` § Skill directory.
- Execution Configuration section is complete with all runtime inputs enumerated.
- Phase 5 terminal artifact structure is committed with schema and field rules sufficient for the calibrator to size work packages against it.
- Status flips from `draft` to `approved` after operator review.

Phase 5 begins on approval. First Phase 5 work: eval set authoring against § Role Definitions failure examples, then decomposition against § Phase 5 Terminal Artifact Structure.

## Changelog

- **2026-04-15 — Initial authoring.** Descends from `[[ARCHITECTURE]]`. Covers SKILL.md body structure, per-phase role application with operating questions and failure examples for all three universal roles including the kernel-surfacing protocol and recognition criterion for the interrogator, asset template specifications for all ten assets in `assets/` (six structural templates, four authoring-instruction assets with instance-specific content removed), Execution Configuration section with Todoist fields, Phase 5 terminal artifact structure (`work-instructions.md`, `execution-plan.json`, work package template, Todoist load operation), and cross-reference conventions. Open questions section dropped — Phase 5 deliverables belong in Phase 5; architecture open questions belong in architecture.