---

artifact: claude-md 
phase: 2 
project: "[[Phased Decomposition]]" 
release: R1 
status: draft 
created: 2026-04-14 
framework_version: 0.1.0 
descends_from:

- "[[ROADMAP]]"

---
# CLAUDE.md — Phased Decomposition R1

## Shape of This Document

`CLAUDE.md` commits the executors that will run R1. The [[ROADMAP|roadmap]]'s §Execution Resourcing names the roster — coordinator, producer, supervisor, operator — and gives one-paragraph descriptions of each. This document commits the full definitions: what each executor does, what it has access to, how it hands off to the others, what bounds its autonomy, and what its relationship to the rest of the release-activation cluster looks like in mechanical terms.

`CLAUDE.md` is a release-level artifact authored at Phase 2 alongside [[ROADMAP|roadmap]], [[TEST-PLAN|test plan]], and [[SCHEDULE|schedule]]. It binds to R1 specifically and would be authored fresh for any future release. It is not part of the phased decomposition skill itself; the skill ships a template for it (`assets/claude-md-template.md`) so future projects using the framework can stamp their own.

## The Roster

R1 commits four executors. Three are Claude instances; one is human. The Claude instances are layered: the coordinator persists across the full execution, while producer and supervisor are work-package-scoped subagents that spawn fresh for each work package and end when the package closes.

- _Coordinator._ Main Claude Code session. Persists across the full R1 execution.
- _Producer._ Claude Code subagent, skill-creator-class. Spawned per work package by the coordinator. Authors skill content against the framing artifacts.
- _Supervisor._ Claude Code subagent. Spawned per work package by the coordinator. Gates producer iterations against the work-package-level test plan.
- _Operator._ The framework author. Sets execution boundaries, fields escalations, and runs release-level dogfooding outside the producer/supervisor loop.

The roster is firm. Producer and supervisor classes may evolve as Anthropic ships better tooling, but the four-executor shape — persistent coordinator, scoped producer, scoped supervisor, human operator — is the architectural commitment for R1.

## Coordinator

### Responsibilities

The coordinator is thin. Its job is to hold the work package queue and route between producer, supervisor, and operator. It does not make project-level judgment calls. It does not decide when a work package is done — the supervisor does that. It does not decide when to escalate — the supervisor's verdict tells it. It does not re-decompose work packages, re-order the queue based on its own assessment, or notice cross-package patterns the supervisor cannot see from inside per-package context. Anything that requires judgment beyond "what does the queue say next" or "what verdict did the supervisor return" is the operator's call, surfaced via an escalation or a wall hit.

The thinness is deliberate. A thicker coordinator would absorb judgment surfaces that need their own calibration, and R1 has no budget for calibrating a project-management layer. The wall mechanism (§Operator-Set Walls below) is the operator's lever for staying in control of a thin agent that would otherwise run the queue to completion without check-in.

### State held

The coordinator holds three things across the full R1 execution.

- _The work package queue._ The ordered list of work packages produced by Phase 5, with each package's status (pending, in-flight, closed, escalated).
- _Wall configuration._ The bounds the operator sets at session start (§Operator-Set Walls). The coordinator checks against these on every queue advancement.
- _Escalation routing state._ When a supervisor verdict escalates, the coordinator holds the escalation context (which package, which work package test plan entries failed, the supervisor's failure characterization) until the operator addresses it.

Everything else the coordinator needs at any given moment is read fresh from the skill directory or from the spawned subagent's output. The coordinator does not cache framing artifacts, does not hold supervisor verdicts beyond their immediate routing decision, and does not maintain its own model of the project state independent of what the queue and the skill directory already say.

### Tool access

The coordinator has read access to the canonical artifact directory ([[ROADMAP]], [[TEST-PLAN]], [[SCHEDULE]], `architecture.md`, `spec.md`, Phase 5 outputs). It has spawn access to producer and supervisor subagents. It has no write access to the skill directory itself — only producer writes there. This separation is what makes the coordinator's thinness enforceable: it cannot accidentally absorb authoring responsibilities because it has no way to author.

### Spawn protocol

When the queue advances to the next pending package, the coordinator spawns a producer subagent with the work package definition as input. When the producer reports its iteration complete, the coordinator spawns a supervisor subagent with the producer's output and the work-package-level test plan as input. The supervisor returns a verdict. The coordinator routes the verdict per §Verdict Routing.

Producer and supervisor spawns are fresh per package. The coordinator does not resume a prior subagent instance; each spawn starts with a clean context window. The framing artifacts the producer needs are delivered via the preloaded skill (§Producer Substrate) rather than via spawn-time context injection.

### Verdict routing

- _Pass._ Coordinator marks the work package closed in the queue, advances to the next pending package (subject to wall checks), and spawns the next producer.
- _Rework._ Coordinator routes the supervisor's failure characterization back to the producer and respawns the producer for another iteration on the same work package. Increments the iteration counter for that package.
- _Escalate._ Coordinator marks the work package escalated, pauses queue advancement, surfaces the escalation context to the operator, and waits for operator direction before doing anything else.

## Producer

### Responsibilities

The producer authors skill content. Within a single work package, the producer drafts or refines the specific files the package targets — `SKILL.md` body, a reference file, an asset template, an eval suite entry. Each producer iteration is a single attempt at the work package, ending when the producer reports its draft complete and ready for supervisor gating.

The producer is skill-creator-class. Its primary working substrate beyond the framing artifacts is Anthropic's bundled `skill-creator` skill, which covers skill-writing mechanics, eval authoring, and the draft→test→refine loop. The producer does not need to be taught how to write a skill; it needs to be told what _this_ skill is and what _this_ package's output should be.

### Substrate

The producer's substrate is delivered via Pattern B (preloaded skill), not Pattern A (in-prompt). The framing artifacts that ground the producer's authoring — `concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`, `architecture.md`, `spec.md` — live as byte-for-byte copies inside a Claude Code skill that the producer subagent references via its `skills:` frontmatter. Every producer spawn loads the skill automatically and gets the framing artifacts in context without coordinator handoff.

The skill is packaged once at the start of R1 execution. The packaging step is a literal copy from canonical artifact paths into the skill directory; no transformation, no frontmatter stripping, no wikilink rewriting. If the operator amends a framing artifact between execution sessions, the operator manually re-runs the packaging step before kicking off the next session. Mid-session amendments to framing artifacts are not a concern for R1 because framing-artifact amendments and execution sessions are sequential, not concurrent — the operator does not edit `roadmap.md` while a producer iteration is running.

This decision descends from a documented constraint in Claude Code's skill loader: skill content is expected to live inside the skill directory and is addressed via skill-relative paths. Out-of-directory references are not a viable pattern. The byte-for-byte copy approach eliminates the second-order risk where a packaging transformation becomes a place semantics drift; the once-per-execution packaging cadence eliminates the staleness risk by collapsing it to "between sessions" rather than "during sessions."

### Tool access

Producer has read access to the preloaded skill (framing artifacts plus `skill-creator`). Producer has write access to the R1 skill directory — the directory where the phased decomposition skill itself is being authored. Producer has no access to the canonical artifact directory; the framing artifacts only reach producer via the preloaded skill copy. This forces every producer-visible version of the framing to flow through the packaging step, which is the only thing that makes the byte-for-byte fidelity guarantee enforceable.

### Iteration boundaries

Producer iterations are bounded by work package, not by token budget or wall-clock time. A producer iteration runs from spawn until the producer reports its draft complete. The producer does not carry context across iterations — the next iteration is a fresh spawn with the prior iteration's failure characterization (from supervisor) as input. There is no memory layer, no token-threshold safety valve, no mid-package handoff structure. R1 does not exercise the long-running execution patterns those mechanisms exist to manage; skill authoring iterations complete in single passes or small handfuls of passes, not in context-exhausting loops.

The graduated project-notes entry committing work-package-scoped subagents with memory as a durability layer remains valid for projects where the mechanics matter. R1 simply doesn't exercise that path. A future release on a different project type — long-running coding execution, multi-week build — is the graduation trigger for the deferred mechanics.

## Supervisor

### Responsibilities

Supervisor gates producer iterations against the work-package-level test plan. For each producer iteration, supervisor reads the producer's output and the work package's test plan entries, runs the gating check, and returns one of three verdicts: pass, rework, or escalate. Supervisor does not author content. Supervisor does not modify the producer's output. Supervisor does not write to the skill directory.

The supervisor's gate is the only thing standing between a producer iteration and a closed work package. The coordinator does not second-guess the supervisor; the operator does not see passed packages until a wall hit. The supervisor's calibration is therefore load-bearing for R1 quality, and §Verdict Logic below commits the rules that calibration runs against.

### Test plan held

Supervisor holds the work-package-level test plan as its primary gate. It also holds the parent task-level test plan entry that the work-package entry traces upward from, as upstream context. The work-package plan is what supervisor checks the producer's output against; the parent task-level entry is read to catch drift between what the work package is testing and what the task level was actually checking for.

This plus-parent shape is what makes the framework's test-plan traceability load-bearing rather than ceremonial. Every work-package test entry traces upward to a task-level entry by construction (per the test plan parallel-production rule). Supervisor reading both ends of that trace is what closes the loop: a producer draft can satisfy the work-package entry in isolation while drifting from the task-level intent, and the supervisor catches that drift by holding both.

Higher levels (release-level, framework-level) are not in supervisor context. Drift at those levels is caught by escalation, not by the supervisor's primary gate.

### Tool access

Supervisor has read access to the preloaded skill (same skill the producer references), read access to the producer's output for the current iteration, and read access to the work-package-level test plan and its parent task-level entry. Supervisor has no write access anywhere. Supervisor's only output is the verdict returned to the coordinator.

### Verdict logic

Three verdicts. The boundaries between them are not heuristic — they descend from a specific question.

- _Pass._ Producer's output satisfies every work-package test entry, and reading both the work-package plan and the parent task-level entry confirms the output is not drifting from upstream intent. Supervisor returns pass.
- _Rework._ Producer's output fails one or more work-package test entries, and supervisor can describe what a successful next draft would look like given the current work package definition. The failure is in the draft, not in the spec the draft is being checked against. Supervisor returns rework with the failure characterization as input for the next iteration.
- _Escalate._ Producer's output fails one or more work-package test entries, and supervisor cannot describe what a successful next draft would look like. The failure is in the work package definition, the parent task-level entry, the spec the task descends from, or further upstream. Supervisor returns escalate with the failure characterization and the supervisor's read on which level the failure traces to.

Escalation is also triggered by two safety signals, independent of the per-iteration verdict logic:

- _Iteration cap._ If a single work package has accumulated more than N rework iterations, supervisor returns escalate on the next iteration regardless of verdict logic. N is operator-set per session (§Operator-Set Walls). The cap catches runaway rework loops where the producer keeps swinging at the same package without converging — a failure mode the per-iteration verdict logic would miss because each iteration's failure characterization might be slightly different even though the producer is not getting closer.
- _Failure characterization recurrence._ Supervisor names the failure on each rework verdict. If the same characterization recurs across iterations within a single work package, supervisor escalates on the next iteration regardless of iteration count. Recurrence indicates the producer has converged on a draft satisfying its own reading of the work package but failing the same upstream check repeatedly — which is the signature of an upstream problem, not a draft problem. The cap catches mechanical loops; the characterization rule catches logic errors.

Both signals run together. The cap is the safety net; the characterization rule is the primary logic signal. Either firing escalates the work package.

## Operator

### Responsibilities

The operator is human. The operator's role in R1 execution is to set the bounds at session start, field the escalations and wall hits the bounds produce, and run dogfooding against the built skill once the producer/supervisor loop closes. The operator is not in the loop iteration-to-iteration; the operator is in the loop at the boundaries the operator themselves committed.

This is the load-bearing claim from the [[NORTHSTAR|northstar]]: the operator stays in idea-space throughout, including during execution, because the framework has front-loaded the decisions that would otherwise pull them out. The thin coordinator and bounded autonomy are the mechanical realization of that claim for R1. The operator commits the bounds at session start and trusts the supervisor's gating until the bounds fire.

### Loop touchpoints

Three. No others.

- _Session start._ Operator sets the coordinator's wall and the supervisor's iteration cap (§Operator-Set Walls). Operator confirms the queue position the session will start from, kicks off execution, and exits the loop.
- _Wall hits._ Coordinator pauses regardless of state when a wall fires. Operator reviews queue position, in-flight package status, and any closed packages the operator hasn't seen yet. Operator decides: resume with the same wall, resume with a new wall, or end the session. No per-package review pressure — the wall is the operator's chosen check-in cadence, not a decision review gate.
- _Escalations._ Coordinator surfaces escalation context (which package, supervisor's failure characterization, supervisor's read on which level the failure traces to). Operator decides which upstream artifact the failure traces to per `architecture.md`'s escalation paths (Phase 5 if the eval was wrong, Phase 4 if the spec was wrong, Phase 3 if the architecture was wrong, Phase 2 if a roadmap constraint was wrong). Operator amends the relevant artifact, re-runs whatever loop slice the amendment invalidates, and resumes.

The operator does not see passed packages between wall hits. Closed work packages flow into the skill directory, the queue advances, and the operator catches up at the next wall. This is the boundary-only review model, and it is the consistent thin-coordinator analog: per-package review would partly defeat the wall mechanism by making every package an implicit check-in.

### Dogfooding's relationship to the loop

Dogfooding is outside the producer/supervisor loop. The loop terminates when the queue closes and the supervisor's mechanical gates pass on every work package. Dogfooding then runs separately: operator runs the built skill against March and Nell per [[TEST-PLAN]]'s outcome, judges resulting Phase 0 artifacts against [[MOCKUP]] scenes as the reference standard, and either closes the test plan or routes failures back through normal escalation paths.

Dogfooding failures are operator-judged escalations. The operator amends the relevant upstream artifact (likely Phase 5 work-package definitions, possibly further upstream) and re-runs whatever loop slice the amendment invalidates. Dogfooding does not have its own loop; it has the same escalation paths as supervisor escalations, just with the operator instead of the supervisor as the gate that fires.

## Operator-Set Walls

The operator sets two values at session start. Both bound autonomous coordinator operation. Both are operator-set per session rather than committed in `CLAUDE.md` itself — `CLAUDE.md` commits that the values exist and that the coordinator and supervisor honor them; the operator commits the values when starting a session.

- _Coordinator wall._ The bound past which the coordinator pauses and surfaces state to the operator regardless of queue or verdict status. Shape options: package count (e.g., "pause after N packages closed"), wall-clock duration (e.g., "pause after 90 minutes"), or queue checkpoint (e.g., "pause after work package P closes"). The operator chooses the shape per session based on what the session is for.
- _Supervisor iteration cap._ The bound past which a single work package's accumulated rework iterations triggers automatic escalation regardless of supervisor verdict logic. Single integer; the operator chooses based on how complex the packages in the current session are expected to be.

Both walls are the operator's lever for staying in control of a thin agent. The thin coordinator's whole premise is that the operator has chosen not to insert judgment between the supervisor's gate and the queue's advancement — but that premise only holds if the operator has set bounds beyond which they want to be pulled back in. The walls are how the operator commits those bounds without micromanaging the loop.

## Boundaries

What `CLAUDE.md` deliberately doesn't commit, so later phases and execution sessions have room to commit them.

- _Specific wall values._ Per §Operator-Set Walls. Walls exist in `CLAUDE.md`; values are session-level commitments.
- _Producer model tier._ `CLAUDE.md` commits the producer is skill-creator-class. The actual model (Sonnet, Opus, future model) is set by the producer subagent's frontmatter at the time of execution and may change as Anthropic ships better models for skill authoring. `architecture.md`'s §Skill Shape commits model-tier targeting for the framework's own phases; producer model tier for R1 execution is a separate question committed at execution time.
- _Supervisor failure characterization format._ Supervisor returns failure characterizations as part of rework and escalate verdicts. The format the supervisor uses (free-form prose, structured fields, a specific schema) is left to the supervisor subagent's prompt and may be tuned during R1 execution as the operator sees what works. `CLAUDE.md` commits that characterizations exist and are used for both rework input and recurrence detection; the schema is not bound here.
- _Escalation handoff format._ When the coordinator surfaces an escalation to the operator, the format of that surfacing (terminal output, file write, notification) is implementation detail of the coordinator subagent and not bound here.
- _Memory mechanics._ Per §Producer §Iteration boundaries. R1 does not exercise the cross-package memory layer the graduated project-notes entry commits. A future release on a project type that does exercise it is the graduation trigger for committing the deferred mechanics.

## Closure

`CLAUDE.md` is approved when the operator has reviewed the executor definitions and confirmed they match the execution model the operator wants to run R1 against. Approval flips status from `draft` to `approved`. After approval, `CLAUDE.md` becomes upstream input to `architecture.md`'s rewrite — `architecture.md` consumes the executor definitions rather than authoring them, and `architecture.md`'s §Phase 3 Terminal is amended to remove the `CLAUDE.md` authoring requirement that was previously committed there.

`CLAUDE.md` does not have its own closure event during R1 execution. It is consulted throughout execution (by the operator setting walls, by anyone reviewing escalation routing) but it does not change. Amendments to `CLAUDE.md` mid-execution would be a signal that the executor model itself was wrong — at which point the right move is pausing execution, amending `CLAUDE.md` with a changelog entry, and resuming against the corrected model.

## Changelog

- **2026-04-14 — Initial authoring.** Authored as a Phase 2 release-activation sibling artifact alongside [[ROADMAP|roadmap]], [[TEST-PLAN|test plan]], and [[SCHEDULE|schedule]]. Five gaps closed via interrogator pass before drafting: producer Pattern B substrate (resolved to B2 with byte-for-byte copy and once-per-execution packaging), memory mechanics (cut from R1 scope; graduated entry remains valid for future projects), coordinator shape (thin, queue-and-router only), supervisor gating (work-package plan plus parent task-level entry; rework/escalate uses both iteration cap and characterization recurrence), operator touchpoints (boundary-only review, dogfooding outside the loop). The Pattern B sub-pattern decision (B2 over B1) descends from a Claude Code skill loader constraint confirmed via web search during the interrogator pass — skill content is expected to live inside the skill directory and out-of-directory references are not viable. Downstream: `architecture.md` requires amendment to relocate executor definitions out of §Phase 3 Terminal and treat `CLAUDE.md` as upstream input.