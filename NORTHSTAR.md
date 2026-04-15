---
artifact: northstar
phase: 1
project: "[[Phased Decomposition]]"
release:
status: approved
created: 2026-04-12T21:30:45-07:00
framework_version: 0.1.0
descends_from:
  - "[[Projects/Phased Decomposition/CONCEPT|CONCEPT]]"
---
# Northstar — Phased Decomposition

**Status:** Phase 1 draft completed

---

## The Felt Experience

The operator holds an end goal and sits down to work on it. The mode is sketching — starting general, staying general, not descending into specifics or technical detail until the shape wants to be rendered. The hard part of translation — rendering unstructured thought into structured, communicable language — is not the operator's job. The operator's notes become the substrate from which reality gets constructed.

This mode persists across every phase of the work, including execution-phase sessions where decisions are being committed to, and including runtime when code is being written or tasks are being executed against those decisions. The operator stays in idea-space throughout because the framework has front-loaded the decisions that would otherwise pull them out, and because the interrogator role — present in every phase — translates residual ambiguity into questions the operator can answer in their native register. By the time any production artifact is being written, the operator's contribution is answering questions about how things should feel, not describing system patterns, task sequences, or execution detail.

## The Claim This Pays Off

The [[Projects/Phased Decomposition/CONCEPT|concept]] doc commits to phased decomposition as a pipeline that front-loads framing work into discrete phases so that drift and contradiction surface upstream, where revision is cheap. The northstar extends that commitment into a phenomenological claim: front-loading is not just cost-efficient, it is operator-preserving. The operator entering the pipeline in idea-space stays in idea-space throughout, because there is nothing left for them to resolve in any other mode by the time later phases run. The interrogator mechanism is what makes this non-magical — it is the causal chain between upstream front-loading and downstream register preservation.

## Scenes That Pay This Off

Three scenes. Each lands in a different location along the pipeline. Taken together they establish that the felt experience holds at the easiest point (upstream, where it is most believable), the hardest point (execution-phase decisions, where the claim is most load-bearing), and the runtime extension (where the operator is not present and the role persists through a supervisor).

### Scene 1 — Cold Start

A session in Phase 0, before any artifact exists. The operator brings unstructured thought about a problem and the shape of a solution they have been chewing on. They speak in paragraphs, not sections. The interrogator's first move is the kernel check — every downstream section of the concept document will descend from the kernel, and a kernel mis-stated here costs the most to correct later. The interrogator does not propose a candidate. It asks the operator to name what the project is, one layer below the surface framing, and pushes the operator to compress until the answer fits as one thought. Where the operator offers a surface mechanism, the interrogator redirects with a "why does it matter" push that moves from mechanism to the load-bearing center. The operator reaches the kernel in their own words, extracted from substance already latent in their opening monologue but not yet surfaced as the thing the project is about. Claude writes the kernel to the concept document as the load-bearing element every other section descends from.

Only after the kernel is surfaced does the interrogator run the section sweep. The concept document has sections the operator has not yet grounded, and Claude asks targeted questions in the operator's register — who this is for, what already exists, what the walk-away version looks like, what day-ninety feels like, what remains unresolved. The operator answers in the register they opened in. Claude renders their answers into concept-document prose, each section tracing to the kernel where relevant. No section is drafted from substance Claude had to invent. Sections the operator did not ground are stubbed rather than authored. The session ends with a first-pass concept document that a cold reader could follow, produced from unstructured operator thought without the operator ever writing a concept-document sentence — and anchored by a kernel the operator named, not one Claude proposed and they ratified.

### Scene 2 — Execution-Phase Architecture

A session in Phase 3, producing the architecture document for a release. Upstream phases are complete. The operator opens the session with statements about how the thing should feel and what they don't want it to become. These are phenomenological descriptions, not architectural patterns. The interrogator runs: for each operator statement, Claude checks whether upstream context already grounds a specific architectural commitment, or whether the statement leaves a pattern-level question open. Where upstream grounds the decision, Claude pulls it forward into the architecture document with traces back to its origin. Where upstream leaves a gap, the interrogator asks the operator a targeted question framed in the operator's own register — never in architecture vocabulary. The operator answers by describing how something should feel or what they don't want to happen; Claude translates the answer into the architectural commitment it implies. The session ends with a set of architectural decisions committed to the document, each traced to its upstream origin, each produced without the operator ever describing a system pattern.

### Scene 3 — Execution Handoff, Runtime, and Release Validation

A sequence spanning Phase 5, Phase 6, runtime, and release-level UAT. Claude derives a chronological task breakdown from the spec and drafts a task-level test plan in parallel by decomposing the spec-level plan downward: every task-level entry traces upward to a spec-level entry, and failures to decompose are flagged as missing tasks or as felt-experience checks belonging at release-level UAT. The operator reviews breakdown and test plan together at a holistic gate, editing scope, ordering, and missing failure modes; approval of both triggers Phase 6.

Phase 6 runs without operator input. Claude derives work packages from the approved breakdown, classifies each per the project's work composition, and decomposes the task-level test plan downward into a work-package-level test plan for each package. Human-executable packages flow into the operator's existing task tool with verification steps attached. Machine-executable packages execute in a coding environment under a supervising Claude instance that holds the work package test plan as its explicit standard. When the supervisor detects a failure — coupling where swappability was required, a missing error path where robustness was specified — it halts the coding instance and routes for rework or escalates upstream. The standard is traceable end-to-end: work package test decomposes from task-level, which decomposes from spec-level, which decomposes from release-level, which decomposes from the northstar. A line of code walks upward to the felt-experience claim it pays off; none of the walk requires the operator to be in the session.

Release completion triggers UAT. Machine-checkable release-level entries have been passing in CI throughout execution; what remains is the entries that require the operator to validate the release against the northstar experience in their own register. The operator closes each entry — pass, fail, or pass-with-followup logged against a later release — and the release-level test plan closes. The release is validated not by Claude and not by the supervisor but by the operator checking the felt experience against the artifact that committed to it at the top of the pipeline, in the register they committed it in.

## Re-instantiability

These scenes should survive substitution of the specific project, the specific operator, the specific file names, the specific tools. A cold start for any project — software, physical, organizational, creative — should satisfy Scene 1 if the interrogator correctly routes unstructured operator thought into a concept document in their register. An execution-phase architecture session on any project should satisfy Scene 2 if upstream commitments are being pulled forward and interrogator-gated ambiguities are being asked in the operator's register. An execution handoff on any project should satisfy Scene 3 if task breakdown is chronologically ordered with a holistic review gate, work packages are classified by project-specific work composition, and the interrogator role extends into runtime supervision. None of the scenes depend on the project being software, on the operator being any particular person, or on the tools being any particular tools.