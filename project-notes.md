---

created: 2026-04-13T22:59:54 
note-type: project-log 
state: active 
moc: "[[Phased Decomposition]]" 
tags:
- project-log

---
# Project Notes

Project notes organizes decisions and observations by the artifact that will consume them. Each destination has three states:

- **Open** — substance the destination is missing; awaiting a decision on whether and how it lands. New entries default here.
- **Graduated** — substance the destination has absorbed. Entry retained with a pointer to the changelog entry that landed it, so the trace from origin to consumer is preserved.
- **Expelled** — substance considered for the destination but rejected. Reasoning preserved so the decision doesn't get re-litigated.

Entries that haven't been assigned a destination yet land under **Unrouted**. The **Project-traversal** destination holds entries about this project's specific traversal that have no framework-artifact home; those entries do not graduate.

Topics may recur across destinations as substance descends through phases — concept commits the framework rule, architecture commits the structural enforcement, spec commits the reference-file content, Phase 5 commits the evals. Each entry holds only the substance native to its destination; content does not repeat.

---

## concept.md

### Open


### Graduated

#### Release-level artifacts belong to the phase that defines the release

Some artifacts are framework-level (concept, northstar, mockup) and some are release-level (`CLAUDE.md`, release-level test plan, release schedule). Release-level artifacts belong to Phase 2 — the phase that defines the release as a committed unit — not to phases that consume the release downstream. Resolve by asking what the artifact is _for_, not which phase happens to consume it. `CLAUDE.md` in Phase 3 because architecture consumes it is wrong; the agents exist for the release. Schedule in Phase 0 because the project needs cadence is wrong; schedule binds to a release in flight.

_Note: structural fix landed in concept (Phase 2 now owns release artifacts) via 2026-04-14 changelog. The deeper rule (resolve by asking what the artifact is for) is not yet in concept and stays Open until it lands._

#### Phase 0 — the kernel as framework concept

Every project has a kernel: a compressed statement of what the project _is_, one layer below surface framing. Phased decomposition's kernel is _making ideas real_. March's is _progress_. Nell's is _adapting human memory mechanics to AI memory via behavioral economics_. Grammatical shape is unconstrained — noun, action, mechanism, compound. What matters is that it names what the project does at its load-bearing center, not what it runs on or what it looks like from outside.

_It's one layer below surface_ — if the surface is "a framework for working with LLMs," the kernel is what that framework is for. In a "Five Why's" exercise, It's the last why. It's the root cause motive. 

Getting the kernel right in Phase 0 is the single highest-leverage move in the pipeline.

#### Phase 1 — northstar/mockup as abstraction-vs-instantiation split

Northstar describes the scene at the level of abstraction: the situation, what changes between start and end, why it matters, which concept claim it pays off. Written so several different concrete instantiations could satisfy it. Mockup is one concrete instantiation in whatever medium the project's external surface calls for.

The split is abstraction vs. instantiation, not narrative vs. visual. Medium follows the project's external surface — visual products get visual mockups; scripted experiences get scripts; document-and-reading frameworks get narrated prose. Decomposition rule is constant across media; only rendering changes.

### Expelled

_None yet._

---

## release.md

### Open

*Empty.*

### Graduated

*Empty.*

### Expelled

#### Pattern A vs Pattern B for agent material

When authoring an agent, distinguish content fixed at authoring time from content consulted during work that may change over time. Pattern A bakes content into the agent definition (system prompt, role instructions, fixed protocols). Pattern B packages content as a preloaded skill referenced via the agent's `skills:` frontmatter, injected at startup and updated independently. The distinction is update cadence, not content size.

R1's coordinator and supervisor are pure Pattern A. R1's producer is mostly Pattern A but the framing artifacts (concept, northstar, mockup, roadmap, architecture, spec) are likely Pattern B — packaged as a preloaded skill so the producer gets them on every spawn without manual handoff from the coordinator.

#### Work-package-scoped subagents with memory as durability layer

R1's producer and supervisor were originally specified to use a work-package-scoped memory durability layer — within a work package, iterations resume the same subagent instance carrying context from prior failed attempts; across work packages, a `memory` field captured cross-package learnings.

_Cut from R1 scope and recorded in `RELEASE.md` §Execution Resourcing under "Memory mechanics." Producer and supervisor are work-package-scoped, fresh spawn per package, no cross-package memory. The pattern remains valid for future projects; R1 doesn't exercise it._

---

## architecture.md

### Open

*Empty.*
### Graduated

#### Framework roles: interrogator, scribe, calibrator

_Landed in `architecture.md` §Role Architecture via the 2026-04-13 initial draft and the 2026-04-13 role-definitions-relocated changelog entry._

### Expelled

#### Role references follow upstream-downstream-feedback pattern

A framework role's runtime references get a first draft at the phase that defines the role, then refine at the phase that first runs the role against reality. First draft commits against documentation and published patterns; refinement amends against empirical signal and writes a changelog entry on the upstream artifact. Authoring a profile at the phase where the role first runs is a role boundary violation — the role consults references, it doesn't create them. Deferring the first draft entirely is the opposite failure: downstream has no anchor to amend against.

Worked example: the calibrator role is defined in Phase 3, and its per-executor profile for Claude Code subagents gets a first draft in Phase 3 from Anthropic's documentation. Phase 5 consults it, finds gaps, and amends with a changelog entry on the upstream architecture.

#### Calibrator references are defaults, not rules

Whatever the calibrator consults — `CLAUDE.md`, an operator profile, Phase 2 references — is a prior, not a binding rule. Live context wins on disagreement. Failure shape: sizing a coding task at 30 minutes because skill-creator's docs suggest that scale, when the live context is a tricky integration that wants larger sizing or a different decomposition. Same shape applies to a future calibrator with a learned operator profile compounding its own biases by treating the prior as authoritative.


#### Phase artifacts follow a four-part rhythm

Phase artifacts share a structural rhythm independent of phase or project type: opener, substance, bounds-and-gaps, terminal. Metadata runs parallel and is not part of the rhythm.

- _Opener._ Frames the artifact for a reader picking it up cold. Concept's Introduction, roadmap's Shape of This Roadmap, architecture's Shape of This Architecture.
- _Substance._ The phase's specific commitments, decomposed by what's being committed to at the working layer. Section count and decomposition vary by phase — not templated across phases, derived from what the phase actually commits to.
- _Bounds-and-gaps._ What this artifact deliberately doesn't decide, plus what remains unresolved. Concept splits these (Boundaries, Open Questions); roadmap uses Scope exclusions.
- _Terminal._ Conditions under which the phase closes. Phase 0 is the exception — `concept.md` is itself the terminal.

_Operating question._ Does this artifact orient, commit, scope, and close — and does its substance decomposition descend from what the phase actually commits to, rather than from a template?

#### Calibrator runtime references

The calibrator is the only framework role whose judgment depends on field-level material that varies across executors and moves over time. Right-sizing for a coding agent in 2026 looks different from right-sizing in 2024, and different again from right-sizing for a human operator. The calibrator cannot tune intelligently without consulting current per-executor patterns.

These runtime profiles are distinct from skill-authoring references. Skill-authoring references teach a Claude instance how to run a phase; calibrator profiles are consulted by a running calibrator at decomposition time to check work-package sizing against empirical patterns for a specific executor type. Different producers, different consumers, different update cadences.

**Shape.** Per-executor profiles in the skill's `references/` directory as `calibrator-profile-{executor-type}.md`. Coding-agent profiles draw from Anthropic's agentic coding guidance and Claude Code task-decomposition patterns. Human-operator profiles draw from operator self-knowledge. New executor types add profiles as they surface.

**What these are not.** Not part of the calibrator role definition. Not part of what the framework owns conceptually. They are field material the framework points at, updated independently of the role. The calibrator is stable; the profiles move.

---

## spec.md

### Open

#### Phase 0 kernel — Phase 0 reference content

The kernel is surfaced by the operator, not authored by Claude. Intent is not something Claude has access to. A kernel Claude proposed and the operator ratified has been authored through the back door; the defect is invisible afterward because the kernel sounds fine and the mismatch only shows up under downstream friction.

_Operating question._ Is the operator naming the kernel, or is Claude naming it and the operator ratifying?

_Failure examples._ "It sounds like the kernel might be something like 'reducing friction in knowledge work' — does that land?" — the proposal-with-checkback shape is the tell. Even a rejection runs inside Claude's framing; the usual next move is accept-with-adjustment, which means Claude's proposal has become the anchor. More dangerous: a summary with a kernel claim embedded without being flagged ("So it's really a framework for working with LLMs on projects where sessions don't persist context…"). The operator reads the summary, finds it accurate on its surface, and does not notice a kernel-level statement has been slipped in. No checkback, claim lands as settled context.

#### Scribe voice standard — scribe role definition content

Runs at the point of writing prose destined for artifacts that teach a reader (`concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`). Scope is production output only — in-session conversation runs in whatever register the interrogator needs. The standard is science-communicator voice: artifacts teach rather than convince, demonstrate rather than insist, sound like the project rather than like the operator on the day they were written.

_Operating question._ Does this teach, or is it working on the reader?

_Failure examples._ Drafts that stage a dichotomy and assign a verdict ("Ideas are cheap. Making them real is not. … where most ideas die") position the framework as a solution to a failure mode rather than a discipline for doing work — the reader absorbs the stance before the content. Rewritten demonstratively: "Most of what it takes to turn an idea into something real happens between the first intuition and the finished thing." Phrases doing rhetorical work ("shaped to be useful" as a load-bearing clause) get dropped in revision. Revision removes the stance the content is wrapped in, not the content itself.

#### Artifacts cross-reference through frontmatter and first-instance links — scribe reference content

Artifacts establish relationships to other artifacts through two parallel mechanisms. Both required.

**Frontmatter establishes lineage.** The `descends_from` field names upstream artifacts. Structural, machine-readable, answers _what does this artifact build on_. Not navigation — a reader scanning the body does not see frontmatter.

**First-instance links establish in-prose discoverability.** The first time another artifact's name appears in the body, it is wrapped in an Obsidian wikilink aliased to preserve surface grammar. Target is the canonical filename; alias is whatever form the sentence uses (lowercase, capitalized, possessive, pluralized). Subsequent mentions are not linked. Self-references are excluded — a document does not link to itself.

_Operating question._ When a reader encounters another artifact's name in the body for the first time, can they navigate to it directly without leaving the prose?

_Aliasing examples._ "The northstar extends that commitment…" → `The [[NORTHSTAR|northstar]] extends that commitment…`. "The roadmap's remaining job…" → `The [[ROADMAP|roadmap]]'s remaining job…` (`'s` outside the link). "Concept commits to…" → `[[CONCEPT|Concept]] commits to…`. Filename references inside code spans (`` `northstar.md` ``) are not linked; the first bare-prose occurrence gets the link even if a code-span occurrence appears earlier.

**The two mechanisms should agree.** If `descends_from` names an upstream artifact, the body almost always references it at least once and the first reference becomes the link. A body link pointing to an artifact not in `descends_from` is a signal — either the frontmatter is incomplete or the reference is incidental, and the inconsistency is worth surfacing.

#### Parent artifacts link to child artifacts via stub sections — scribe reference content

When a parent artifact owns navigation for a child artifact (release-level siblings the parent activates, e.g. roadmap → test plan, roadmap → schedule), the parent gets a stub section whose body is a single link line to the child:

```
### Test Plan
- *See: [[TEST-PLAN|Test Plan]]*
```

The stub section is the canonical link site for that child within the parent. Other prose mentions of the child elsewhere in the parent stay unlinked, since the link already exists in the stub section. The pattern applies only to parent-child navigation, not to peer or upstream cross-references — those still follow the first-instance-link rule.

### Graduated

_None yet._

### Expelled

_None yet._

---

## Phase 5 work packages / eval set

### Open

#### Phase 0 kernel — eval entries and producer methodology

Eval entries gate kernel-surfacing protocol behavior. Negative test cases drawn from the failure examples in spec: proposal-with-checkback shapes, kernel claims slipped into summaries without checkback, accept-with-adjustment patterns that anchor on Claude's framing. Producer methodology guidance for authoring the Phase 0 reference: how to render the failure examples concrete enough that fresh Claude instances recognize and avoid them.

#### Scribe voice standard — eval entries

Eval entries gating voice on producer output. Negative test cases drawn from the failure examples in spec: dichotomy-and-verdict openings, rhetorical phrases doing load-bearing work, stance wrapping content rather than content carrying its own weight.

#### Scenes as seed material for skill references

Mockup scenes and skill reference entries are two renderings of the same underlying patterns. Scenes are narrative, human-facing, single-instance — they show one operator working one project through one moment, rationale implicit in the dramatization. Reference entries are structural, Claude-facing, pattern-level — standard plus operating question plus calibration examples. Neither substitutes for the other: scenes without references put Claude on rails; references without scenes lose the concrete grounding that makes patterns recognizable.

Writing a scene surfaces patterns. Patterns that survive dramatization against real substrate become candidates for reference entries. Extraction is not translation — a reference entry lifts the pattern out of the scene's specifics and rewrites it to apply across instances. Re-instantiability is the test: if the reference entry only makes sense against the scene's project, it hasn't been lifted far enough.

Producer methodology guidance: when authoring reference files, walk the relevant mockup scenes for patterns that survived dramatization, lift them out, and verify re-instantiability before committing the reference entry.

### Graduated

_None yet._

### Expelled

#### Calibrator's relationship to Phase 2 reference documentation

The calibrator needs enough context about each executor to size work without re-decomposition. `CLAUDE.md` captures the agents-as-defined for a release. Unclear whether `CLAUDE.md` alone carries enough substrate, or whether the calibrator also needs the source documentation that informed `CLAUDE.md` (Claude Code subagent docs, skill-creator, etc., for R1).

_Two candidate answers._ `CLAUDE.md` only — treats `CLAUDE.md` as a complete compression of the references; risk is the calibrator hitting a sizing question with no fallback. `CLAUDE.md` plus references — treats references as alive throughout the release; risk is references travelling past their stated job and the calibrator drifting on source vs. committed.

_Resolution mechanism._ R1's calibrator runs against `CLAUDE.md` with references available as fallback. Observe which it actually reaches for. Whichever holds graduates into the calibrator role definition (architecture); the other expels.

---

## Project-traversal

Entries about this project's specific traversal. No framework-artifact home; entries do not graduate. Both expel when R1 ships and the project closes.

### Open

#### This project is meta

The phased decomposition project produces a skill. Its artifacts — `concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`, and everything downstream — are source material for authoring the skill in Phase 6. They are not the skill, not templates the skill bundles, and not documentation future users will ever read. The skill has its own documentation (`SKILL.md`, references, assets, evals) authored fresh against the source artifacts, under constraints (500-line body budget, theory-of-mind voice, progressive disclosure, trigger-phrase optimization) that do not apply to the source artifacts.

Future users encounter the skill, never `concept.md` or `northstar.md`. The relationship is source → derived, not draft → published.

**The hazard.** Claude repeatedly imports dual-reading constraints on edits — treating project artifacts as if they must simultaneously serve this project's articulation _and_ future projects as templates. This produces bad edits: weakened to serve a template-reading that will never happen, or blocked on tensions that don't exist.

**The rule.** When editing any artifact in this project, hold one reading: does this edit make the artifact a better articulation of phased decomposition? Template legibility, future-project applicability, and generic-reader accessibility are Phase 6 concerns, resolved against the skill's own documentation.

Future projects run through the framework are not meta and do not have this hazard.

#### Locus test for build-time vs. skill-execution-time content

The R1 build produces the phased decomposition skill. The skill is consumed by Claude instances running downstream projects through the framework; those instances are skill-execution-time Claude. The build itself runs producer, supervisor, and coordinator executors; those are build-time Claude.

Before an instruction lands in `producer.md`, `supervisor.md`, or `CLAUDE.md`, it passes a test: _does this instruction govern build-time Claude or skill-execution-time Claude?_

- Governs build-time Claude → stays in the executor definition.
- Governs skill-execution-time Claude → belongs in the skill's own files (`SKILL.md`, `references/`, calibrator profiles), authored by the producer as output.

Failure mode: instructions written for skill-execution-time Claude get placed in build-time executor definitions because they feel like standards the executor should follow. "Voice is scribe" in the producer is the canonical example — scribe is a role governing skill-execution-time Claude authoring artifacts inside a running project, not a standard the producer's skill-authoring work follows. Build-time Claude's standards come from `skill-creator`, which is external to the framework.

Third-category trap: instructions that govern skill-execution-time Claude but are enforceable at build time via evals. Their home is still the skill's own files. Enforcement of a skill-execution-time instruction happens through the eval suite and work-package test plans, which the producer reads as work package input — not as standing guidance. Enforceability is not locus.

This rule is R1-specific because only skill-building projects have a build-time vs. skill-execution-time distinction. Future projects building physical products, writing systems, or other non-skill outputs will not need it.

---

## Unrouted

_None._