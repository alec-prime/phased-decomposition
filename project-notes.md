---

created: 2026-04-13T22:59:54 
note-type: project-log 
state: active 
moc: "[[Phased Decomposition]]" 
tags:
- project-log

---
# Project Notes

Rules and observations surfaced while traversing phased decomposition. Notes are written as minimally as possible. 

Entries are grouped by state:

- **Open** — live, unresolved, or awaiting a graduation trigger.
- **Graduated** — settled. Skill authoring will draw from these.
- **Expelled** — superseded or stale. Kept in quarantine so the reasoning isn't lost but doesn't mislead future reads.

---
## Graduated

### Parent artifacts link to child artifacts via stub sections

When a parent artifact owns navigation for a child artifact (release-level siblings the parent activates, e.g. roadmap → test plan, roadmap → schedule), the parent gets a stub section whose body is a single link line to the child:

```
### Test Plan
- *See: [[TEST-PLAN|Test Plan]]*
```

The stub section is the canonical link site for that child within the parent. Other prose mentions of the child elsewhere in the parent stay unlinked, since the link already exists in the stub section. The pattern applies only to parent-child navigation, not to peer or upstream cross-references — those still follow the first-instance-link rule from `Artifacts cross-reference through frontmatter and first-instance links`. The first-instance rule and this pattern coexist; they cover different relationship shapes.

### Calibrator references are defaults, not rules

Whatever the calibrator consults — `CLAUDE.md`, an operator profile, Phase 2 references — is a prior, not a binding rule. Live context wins on disagreement. Failure shape: sizing a coding task at 30 minutes because skill-creator's docs suggest that scale, when the live context is a tricky integration that wants larger sizing or a different decomposition. Same shape applies to a future calibrator with a learned operator profile compounding its own biases by treating the prior as authoritative.

### Pattern A vs Pattern B for agent material

When authoring an agent, distinguish content fixed at authoring time from content consulted during work that may change over time. Pattern A bakes content into the agent definition (system prompt, role instructions, fixed protocols). Pattern B packages content as a preloaded skill referenced via the agent's `skills:` frontmatter, injected at startup and updated independently. The distinction is update cadence, not content size.

R1's coordinator and supervisor are pure Pattern A. R1's producer is mostly Pattern A but the framing artifacts (concept, northstar, mockup, roadmap, architecture, spec) are likely Pattern B — packaged as a preloaded skill so the producer gets them on every spawn without manual handoff from the coordinator.

### Role references follow upstream-downstream-feedback pattern

A framework role's runtime references get a first draft at the phase that defines the role, then refine at the phase that first runs the role against reality. First draft commits against documentation and published patterns; refinement amends against empirical signal and writes a changelog entry on the upstream artifact. Authoring a profile at the phase where the role first runs is a role boundary violation — the role consults references, it doesn't create them. Deferring the first draft entirely is the opposite failure: downstream has no anchor to amend against.

Worked example: the calibrator role is defined in Phase 3, and its per-executor profile for Claude Code subagents gets a first draft in Phase 3 from Anthropic's documentation. Phase 5 consults it, finds gaps, and amends with a changelog entry on the upstream architecture.

### Release-level artifacts belong to the phase that defines the release

Some artifacts are framework-level (concept, northstar, mockup) and some are release-level (`CLAUDE.md`, release-level test plan, release schedule). Release-level artifacts belong to Phase 2 — the phase that defines the release as a committed unit — not to phases that consume the release downstream. Resolve by asking what the artifact is _for_, not which phase happens to consume it. `CLAUDE.md` in Phase 3 because architecture consumes it is wrong; the agents exist for the release. Schedule in Phase 0 because the project needs cadence is wrong; schedule binds to a release in flight.

### Framework roles: interrogator, scribe, calibrator

The framework uses three latent roles. Latent because the operator experiences one coherent Claude, not a switching cast; internally the skill instructs Claude to occupy the relevant role at the relevant moment. Separating them gives each role a single standard to hold, which is what prevents the roles from contaminating each other (interrogator mirrors operator register; scribe filters it out).

**Interrogator.** Runs first in every phase. Verifies that context — upstream artifacts plus current-session operator input — is sufficient for the phase's production work before production runs. When context is insufficient, it asks the operator in the operator's native register (never in production-layer vocabulary), or flags that an upstream artifact needs amendment. The translation from idea-space answer into production commitment is Claude's job, not the operator's. This mechanism is the causal chain behind the northstar's operator-in-idea-space claim.

_Operating question._ Can Claude do this phase's production work on the available context without inventing substance the operator or prior phases should have provided?

Calibration is per-phase. Phase 0's interrogator gates kernel surfacing and the section sweep. Phase 1's interrogator gates two input routes, each with its own minimum-viability bar:

- _Scene-first_ (concrete → abstract). Operator brings a scene; Claude extracts northstar abstraction from it. Minimum: enough concrete texture that the re-instantiability test can be applied. "The operator opens the project and feels good" fails — _good how, triggered by what, contrasted against what_.
- _Feeling-first_ (abstract → concrete). Operator brings a phenomenological claim; Claude generates a mockup scene instantiating it, using substrate from outside the project to avoid self-reference. Minimum: enough specificity to generate a recognizable scene. "It feels easier" fails — _easier than what, in which moment, because what friction is absent_.

The standard is the operating question, not a checklist. Checklists invite ceremony — the interrogator stops interrogating and starts form-filling. Phase 3 gates architecture commitments; Phase 4 gates spec commitments; each phase's failure modes get calibrated when the phase is worked in practice.

**Scribe.** Runs at the point of writing prose destined for artifacts that teach a reader (`concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`, `architecture.md`, `spec.md`). Scope is production output only — in-session conversation runs in whatever register the interrogator needs. The standard is science-communicator voice: artifacts teach rather than convince, demonstrate rather than insist, sound like the project rather than like the operator on the day they were written.

_Operating question._ Does this teach, or is it working on the reader?

_Failure examples._ Drafts that stage a dichotomy and assign a verdict ("Ideas are cheap. Making them real is not. … where most ideas die") position the framework as a solution to a failure mode rather than a discipline for doing work — the reader absorbs the stance before the content. Rewritten demonstratively: "Most of what it takes to turn an idea into something real happens between the first intuition and the finished thing." Phrases doing rhetorical work ("shaped to be useful" as a load-bearing clause) get dropped in revision. Revision removes the stance the content is wrapped in, not the content itself.

**Calibrator.** Runs during Phase 5 decomposition, after the interrogator clears and before the scribe writes. Tunes the granularity of leaves in the decomposition tree so each leaf is sized for the executor it's assigned to. A leaf right-sized for a coding agent is too coarse for a human operator, and vice versa; granularity is per-executor, per-task, per-project.

_Operating question._ Can the assigned executor execute this leaf without re-decomposing it, and can the eval at this level definitively answer "done correctly"?

_Failure examples._ _Too coarse for a coding agent:_ "implement the email connector" forces the agent to make a dozen interface and error-handling decisions before writing code, and the eval can't gate any of them. _Too fine for a human:_ "open Gmail settings, click Forwarding tab, enable IMAP, click Save" is three clicks the operator does in fifteen seconds; the eval could gate the parent. _Right size, wrong eval:_ granularity-of-work and granularity-of-eval have to match.

The calibrator reads `CLAUDE.md` (per-release, Phase 2) to know which executor each branch is assigned to, and consults per-executor profiles (Open: calibrator runtime references) to know what right-sized means for that executor. Without `CLAUDE.md` it doesn't know who it's sizing for; without profiles it doesn't know what right-sized looks like.

**Shared shape.** Each role has a standard, an operating question, and failure examples that calibrate judgment. The operating question is the standard; failure examples calibrate it. Future roles follow the same shape when they surface.

### Phase 0 — the kernel

Every project has a kernel: a compressed statement of what the project _is_, one layer below surface framing. Phased decomposition's kernel is _making ideas real_. March's is _progress_. Nell's is _adapting human memory mechanics to AI memory via behavioral economics_. Grammatical shape is unconstrained — noun, action, mechanism, compound. What matters is that it names what the project does at its load-bearing center, not what it runs on or what it looks like from outside.

Three properties. _Compressive_ — fits in the mouth, held as a single thought. _Generative_ — every major decision traces back to it; decisions that don't are suspect. _One layer below surface_ — if the surface is "a framework for working with LLMs," the kernel is what that framework is for.

Phase 0 production cannot clear without a kernel the operator has named in their own words. A kernel mismatch does not surface in Phase 0 — it surfaces in Phase 3 or later, when architecture starts failing to cohere against the stated concept, and by then the cost of revision cascades through every descendant artifact. Getting the kernel right in Phase 0 is the single highest-leverage move in the pipeline.

The kernel is surfaced by the operator, not authored by Claude. Intent is not something Claude has access to. A kernel Claude proposed and the operator ratified has been authored through the back door; the defect is invisible afterward because the kernel sounds fine and the mismatch only shows up under downstream friction.

_Operating question._ Is the operator naming the kernel, or is Claude naming it and the operator ratifying?

_Failure examples._ "It sounds like the kernel might be something like 'reducing friction in knowledge work' — does that land?" — the proposal-with-checkback shape is the tell. Even a rejection runs inside Claude's framing; the usual next move is accept-with-adjustment, which means Claude's proposal has become the anchor. More dangerous: a summary with a kernel claim embedded without being flagged ("So it's really a framework for working with LLMs on projects where sessions don't persist context…"). The operator reads the summary, finds it accurate on its surface, and does not notice a kernel-level statement has been slipped in. No checkback, claim lands as settled context.

### Phase 1 — northstar/mockup decomposition and mockup expansion

**The split is abstraction vs. instantiation, not narrative vs. visual.** Northstar describes the scene at the level of abstraction: the situation, what changes between start and end, why it matters, which concept claim it pays off. Written so several different concrete instantiations could satisfy it — no specific operator names, project names, filenames. Mockup is one concrete instantiation in whatever medium the project's external surface calls for (script, deck, visual, narrated prose). Fictional specifics are the equivalent of pixels in a UI mockup: not the point, but without them the thing isn't tangible.

_Re-instantiability test._ A second mockup with different specifics should still satisfy the northstar. If swapping the specifics breaks the scene, those specifics belong in northstar. If the scene survives the swap, they belong in mockup.

Medium follows the project's external surface. Visual products get visual mockups; scripted experiences get scripts; document-and-reading frameworks get narrated prose. Decomposition rule is constant across media; only rendering changes.

**Mockup expands from file to directory** when components need to be handled independently downstream — fed to different tools, generated in different sessions, iterated on different cycles. When expanded, `mockup.md` becomes a manifest describing the artifact and pointing at sibling component files (`mockup/slides.md`, `mockup/prompts/slide-03-hero.txt`, `mockup/construction.md`). Single file sufficient for narrated prose or a short script; directory required for a deck or a video. This mirrors Phase 0's allowance for `concept.md` to expand into sub-documents, with a sharper trigger specific to mockup's role as input to downstream production tools.

### Test plans parallel production at every level

Every phase that produces a production artifact also produces a test plan artifact at the same level. Test plans are not a separate phase and not a downstream addition — they are generated alongside the artifact they validate, by the same interrogator that gated the production work. They are the concrete form the interrogator's "does production descend from context" check takes for execution-layer artifacts.

Per current phase structure:

- Phase 2 produces `roadmap.md` and the release-level test plan (as a release-activation artifact for the active release).
- Phase 3 produces `architecture.md`. Release-level test plan authored in Phase 2 is inherited and may be amended if architecture surfaces gaps.
- Phase 4 produces `spec.md` and a spec-level test plan.
- Phase 5 produces the execution plan (human-readable and machine-readable), the eval set, and task/work-package-level test plans.

**Traceability.** Tests at each level trace upward. A work-package test traces to its task test, which traces to the spec test, which traces to the release test, which traces to the northstar. A failing work-package test surfaces whether the upstream chain is still coherent. A release-level test that cannot be decomposed downward is a signal the artifact at the level below is under-resolved.

**UAT lands at release level by default.** The operator validates the release as a whole against northstar and mockup. Pushing UAT downward is a signal upstream artifacts did not successfully pre-resolve the questions that would have made higher-level testing sufficient. UAT at work-package level is the failure mode — the operator is being asked to validate at a granularity too fine to hold meaningful judgment about.

Phases 0–2 framing artifacts (concept, northstar, mockup, roadmap) do not have test plans in the same sense because their correctness is not checkable by derivation — only by operator judgment and cold-reader legibility.

### Scenes as seed material for skill references

Mockup scenes and skill reference entries are two renderings of the same underlying patterns. Scenes are narrative, human-facing, single-instance — they show one operator working one project through one moment, rationale implicit in the dramatization. Reference entries are structural, Claude-facing, pattern-level — standard plus operating question plus calibration examples. Neither substitutes for the other: scenes without references put Claude on rails; references without scenes lose the concrete grounding that makes patterns recognizable.

Writing a scene surfaces patterns. Patterns that survive dramatization against real substrate become candidates for reference entries. Extraction is not translation — a reference entry lifts the pattern out of the scene's specifics and rewrites it to apply across instances. Re-instantiability is the test: if the reference entry only makes sense against the scene's project, it hasn't been lifted far enough. Extraction happens at skill-authoring time, not at scene-writing time.

---
## Open

### Calibrator's relationship to Phase 2 reference documentation

The calibrator needs enough context about each executor to size work without re-decomposition. `CLAUDE.md` captures the agents-as-defined for a release. Unclear whether `CLAUDE.md` alone carries enough substrate, or whether the calibrator also needs the source documentation that informed `CLAUDE.md` (Claude Code subagent docs, skill-creator, etc., for R1).

_Two candidate answers._ `CLAUDE.md` only — treats `CLAUDE.md` as a complete compression of the references; risk is the calibrator hitting a sizing question with no fallback. `CLAUDE.md` plus references — treats references as alive throughout the release; risk is references travelling past their stated job and the calibrator drifting on source vs. committed.

_Graduation trigger._ R1's calibrator runs against `CLAUDE.md` with references available as fallback. Observe which it actually reaches for. Whichever holds graduates; the other expels.

### Work-package-scoped subagents with memory as durability layer (R1)

R1's producer and supervisor are Claude Code subagents scoped to a single work package. Within a work package, iterations resume the same subagent instance so the producer carries context from prior failed attempts into the next. A token-budget threshold acts as a safety valve: if accumulated context bloats mid-package, the session ends and a fresh subagent spawns for the remaining work. Across work packages, the `memory` field captures learnings — producer notes on resolved ambiguities, supervisor calibration notes on failure modes — for future packages to consume.

_What's committed._ The scoping rule (work-package-bounded) and the durability layer (`memory` for cross-package state). The architectural shape is firm.

_What's deferred._ Exact mechanics: token threshold value, what specifically gets written to memory and by which agent, how mid-package handoffs structure their state between the ending and the fresh subagent, whether the supervisor has its own memory or shares the producer's. All of this is `CLAUDE.md`-authoring detail.

_Graduation trigger._ `CLAUDE.md` is authored with the mechanics committed, and R1 execution holds against the committed mechanics without requiring mid-execution revisions to the agent definitions.

### This project is meta

The phased decomposition project produces a skill. Its artifacts — `concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`, and everything downstream — are source material for authoring the skill in Phase 6. They are not the skill, not templates the skill bundles, and not documentation future users will ever read. The skill has its own documentation (`SKILL.md`, references, assets, evals) authored fresh against the source artifacts, under constraints (500-line body budget, theory-of-mind voice, progressive disclosure, trigger-phrase optimization) that do not apply to the source artifacts.

Future users encounter the skill, never `concept.md` or `northstar.md`. The relationship is source → derived, not draft → published.

**The hazard.** Claude repeatedly imports dual-reading constraints on edits — treating project artifacts as if they must simultaneously serve this project's articulation _and_ future projects as templates. This produces bad edits: weakened to serve a template-reading that will never happen, or blocked on tensions that don't exist.

**The rule.** When editing any artifact in this project, hold one reading: does this edit make the artifact a better articulation of phased decomposition? Template legibility, future-project applicability, and generic-reader accessibility are Phase 6 concerns, resolved against the skill's own documentation.

This entry does not graduate. Future projects run through the framework are not meta and do not have this hazard.

### Calibrator runtime references

The calibrator is the only framework role whose judgment depends on field-level material that varies across executors and moves over time. Right-sizing for a coding agent in 2026 looks different from right-sizing in 2024, and different again from right-sizing for a human operator. The calibrator cannot tune intelligently without consulting current per-executor patterns.

These runtime profiles are distinct from skill-authoring references. Skill-authoring references teach a Claude instance how to run a phase; calibrator profiles are consulted by a running calibrator at decomposition time to check work-package sizing against empirical patterns for a specific executor type. Different producers, different consumers, different update cadences.

**Shape.** Per-executor profiles capturing current empirical patterns for what right-sized work looks like. Coding-agent profiles draw from Anthropic's agentic coding guidance, Claude Code task-decomposition patterns, and published agentic workflow material. Human-operator profiles draw from operator self-knowledge about personal working patterns. New executor types add profiles as they surface.

**What these are not.** Not part of the calibrator role definition. Not part of what the framework owns conceptually. They are field material the framework points at, updated independently of the role. The calibrator is stable; the profiles move.

**Shape.** Per-executor profiles in the skill's `references/` directory as `calibrator-profile-{executor-type}.md`. Coding-agent profiles draw from Anthropic's agentic coding guidance and Claude Code task-decomposition patterns. Human-operator profiles draw from operator self-knowledge. New executor types add profiles as they surface.

### Artifacts cross-reference through frontmatter and first-instance links

Artifacts establish relationships to other artifacts through two parallel mechanisms. Both required.

**Frontmatter establishes lineage.** The `descends_from` field names upstream artifacts. Structural, machine-readable, answers _what does this artifact build on_. Not navigation — a reader scanning the body does not see frontmatter.

**First-instance links establish in-prose discoverability.** The first time another artifact's name appears in the body, it is wrapped in an Obsidian wikilink aliased to preserve surface grammar. Target is the canonical filename; alias is whatever form the sentence uses (lowercase, capitalized, possessive, pluralized). Subsequent mentions are not linked. Self-references are excluded — a document does not link to itself.

_Operating question._ When a reader encounters another artifact's name in the body for the first time, can they navigate to it directly without leaving the prose?

_Aliasing examples._ "The northstar extends that commitment…" → `The [[NORTHSTAR|northstar]] extends that commitment…`. "The roadmap's remaining job…" → `The [[ROADMAP|roadmap]]'s remaining job…` (`'s` outside the link). "Concept commits to…" → `[[CONCEPT|Concept]] commits to…`. Filename references inside code spans (`` `northstar.md` ``) are not linked; the first bare-prose occurrence gets the link even if a code-span occurrence appears earlier.

**The two mechanisms should agree.** If `descends_from` names an upstream artifact, the body almost always references it at least once and the first reference becomes the link. A body link pointing to an artifact not in `descends_from` is a signal — either the frontmatter is incomplete or the reference is incidental, and the inconsistency is worth surfacing.

_Graduation trigger._ Phase 3 drafting uses this rule in practice and it survives contact with content.

### Phase artifacts follow a four-part rhythm

Phase artifacts share a structural rhythm independent of phase or project type: opener, substance, bounds-and-gaps, terminal. Metadata runs parallel and is not part of the rhythm.

- _Opener._ Frames the artifact for a reader picking it up cold. Concept's Introduction, roadmap's Shape of This Roadmap, architecture's Shape of This Architecture.
- _Substance._ The phase's specific commitments, decomposed by what's being committed to at the working layer. Section count and decomposition vary by phase — not templated across phases, derived from what the phase actually commits to.
- _Bounds-and-gaps._ What this artifact deliberately doesn't decide, plus what remains unresolved. Concept splits these (Boundaries, Open Questions); roadmap uses Scope exclusions.
- _Terminal._ Conditions under which the phase closes. Phase 0 is the exception — `concept.md` is itself the terminal.

_Operating question._ Does this artifact orient, commit, scope, and close — and does its substance decomposition descend from what the phase actually commits to, rather than from a template?

_Graduation trigger._ `architecture.md` is drafted against a section list descending from this rule and the structure survives contact with content. If drafting forces section merges, splits, or additions the rule didn't predict, the rule gets revised before graduating.

---

## Expelled

### Phase 0: the schedule section

_Expelled 2026-04-14._ Proposed a Schedule section in `concept.md` committing cadence, next contact, and horizon. Superseded by the decision that release schedules are release-activation artifacts belonging to Phase 2, not Phase 0. The underlying concern (projects rotting because nothing forces contact with the operator's calendar) is now handled at the release level where the commitment actually binds to work, rather than at the project level where it would float above the execution layer.