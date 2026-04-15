---
artifact: mockup
phase: 1
project: "[[Phased Decomposition]]"
release:
status: approved
created: 2026-04-12T21:30:58-07:00
framework_version: 0.1.0
descends_from:
  - "[[NORTHSTAR]]"
---
# Mockup — Phased Decomposition

**Status:** Phase 1 draft completed.

---


## Scene 1 — Cold Start (March, Phase 0)

### Setting

Nothing exists. No [[Projects/Phased Decomposition/CONCEPT|concept]] doc. No notes. The operator has been chewing on an idea for a few weeks and has scheduled an hour to work on it with Claude. An empty `concept.md` sits in a fresh project folder. Claude has the framework skill loaded and nothing else.

### How it goes

The operator opens with what's in their head.

> "OK, I've been thinking about this for a while. Every productivity app kind of fails for people with ADHD — they all assume you can already manage yourself. They give you a place to put tasks and filters and projects, and the premise is that you'll drive the thing. But the whole problem with ADHD is that driving the thing is the part that's broken. So you set up the system, it works for two weeks on novelty, and then it rots. I'm thinking — what if the AI was the manager. Not a task list. A manager. The thing that tells you what to do next and notices when you drop something and follows up. You just do the work. That's the idea."

Claude does not start writing to `concept.md`. The interrogator runs, and its first move is the kernel check. Every section of the concept doc will descend from the kernel, and a kernel mis-stated here costs the most to correct downstream. The interrogator's job is to make the operator reach for the words themselves — not to offer a candidate and ask the operator to ratify.

> "Before I start drafting — one thing I need you to name. If somebody asked you what March _is_, one layer below the surface description, what's the answer? Not 'an app that helps people with ADHD' — that's the surface. Not 'AI-managed productivity' — that's the mechanism. What's the load-bearing center? What's March actually _about_?"

> "Hm. It's about... it's about not having to hold the management layer in your head. Everybody's got the work, the work is fine. The part that breaks is the part where you have to be the one running it. So March runs it. You just... do the thing."

> "Can you compress that further? Short enough to hold as one thought."

> "Somebody else drives. You just work."

> "That's the surface mechanism, not the kernel. What's the thing the mechanism is in service of? Why does somebody-else-drives matter?"

> "...progress. It's about progress. People who can't hold the management layer aren't lazy, they're stuck. March is about unsticking them. It's about progress."

The operator has reached the kernel. It was in the monologue — _you just do the work_ — but surfaced there as a consequence, not as the thing the project is about. The interrogator's three questions moved the operator from mechanism (somebody else drives) past proximate effect (you just work) to the load-bearing center (progress). Claude did not propose a candidate at any step; each question asked the operator to go one layer deeper on their own words.

Claude writes the kernel to `concept.md`: _The kernel of March is progress — unsticking people whose management layer is broken, so work that would otherwise rot into perpetual setup gets done._ The phrasing is the operator's own, with the gloss surfaced by the interrogator's "why does it matter" push.

Only now does the interrogator move to the five-section sweep. The operator's monologue has grounded context for parts of the concept doc — problem, idea, walk-aways — but the interrogator has questions shaped for what's missing.

> "Three things I want to pull on while it's fresh. First — when you picture the person this is for, is it you, is it the ADHD diagnosis specifically, or is it something broader? Second — is there anything that already sort of does this that you've tried or watched? I don't need a precedent pass, just what's in your head when you think _this doesn't exist yet_. Third — you said 'not a task list.' What else are you explicitly not building? What's the version of this you'd walk away from?"

> "Broader than ADHD. ADHD is the clearest case but I know a bunch of people who don't have a diagnosis and have the same problem. They've got ideas, they've got drive, they can't hold the management layer in their head. Adjacent stuff — there's Saner, closest thing, ADHD-first AI assistant. There's the Notion LifeOS category, which is the thing I hate the most because it's just another system to set up. There's Goblin Tools, little one-shot helpers. None of them are in the manager seat. What I'm not building — not another notes app, not another task manager, not a methodology people have to learn. If it ships with a prescribed workflow, it's not this."

Claude now has grounded context for four sections. Production runs. _The Problem Worth Solving_ descends from the opening paragraph, expanded into cold-reader register while preserving the framing. _The Idea_ descends from _what if the AI was the manager_ and _you just do the work_, traced to the kernel. _What This Isn't_ descends from the three walk-away conditions. _What Already Exists_ gets a short stub with the three named precedents; Claude flags that a proper precedent pass will want more entries and asks whether to do it now or later.

> "Later. I want to see the shape first."

Two sections are underspecified — _What Good Looks Like_ and _What We Don't Know Yet_. The interrogator surfaces both.

> "Two more before I have enough. What Good Looks Like — day ninety, what does using this feel like. And What We Don't Know Yet — forks and tensions you're aware of but haven't resolved. Same mode, or stub them and come back?"

> "I'll take a pass. Day ninety, they open their laptop in the morning and March already knows what they're supposed to be doing. They're not deciding what to work on. They're working. Unknowns — I don't know how much of this ships day one versus gets built later, I don't know whether people will actually open it tomorrow, and I don't know how to pull people toward concreteness without teaching them a framework, which is the thing I said I won't do."

Production runs on both sections, rendered from the operator's phrasings. The draft of `concept.md` now has every in-scope section populated, each section tracing to the kernel where relevant.

### What the session produced

A first draft of `concept.md` with six sections grounded in operator input. Every section was rendered by Claude from operator utterances the operator did not have to shape into document prose. The operator spoke in paragraphs about the problem, the idea, the adjacent landscape, the walk-away conditions, the payoff, and the unresolved questions. The concept doc contains those paragraphs rewritten into the register a cold reader needs.

### What makes this the scene

This is the baseline — the phase where operator-in-idea-space is easiest to believe, because Phase 0 is explicitly a thinking-and-talking phase. The scene establishes the register and shows the interrogator pattern at its most transparent: operator dumps unstructured thought; Claude identifies which concept sections the dump grounds and which it doesn't; Claude asks targeted questions in the same register to fill the gaps; production runs only on sections where context has cleared. Sections the operator did not ground are stubbed rather than invented. No section gets drafted from substance Claude had to author.

---

## Scene 2 — Execution-Phase Architecture (March R1, Phase 3)

### Setting

Six weeks into the March project. `concept.md`, `northstar.md`, `mockup.md`, and `roadmap.md` are written. R1 has been scoped in the roadmap as "the personal build" — a working March for the operator's own machine. This session is the first architecture pass on R1. A blank `architecture.md` is open. Claude has the upstream artifacts loaded. The operator has coffee.

### How it goes

The operator opens with the mode they've been in since Phase 0.

> "I don't want March to feel like a thing I have to open. It should feel like it's already watching. When I sit down at my laptop, there's already a next move waiting for me, and I either do it or I tell it why not."

Claude does not write anything to `architecture.md` yet. The interrogator runs first.

The check, invisible to the operator: is *active presence over destination app* grounded in upstream context? `concept.md §The Idea` commits to it. `northstar.md` scene 2 shows it. `roadmap.md` R1 scope confirms it. The *intent* is grounded. Is there enough to commit to a specific architectural pattern? No — the upstream says "active presence"; it doesn't specify the mechanism. Active presence could be an always-on background process, an event-triggered wake-up, a system-level integration, any of several patterns. All of them would satisfy "already watching" from the operator's side. They have different failure modes and different build costs.

The interrogator has a question now, and it's one the operator can answer in idea-space.

> "Active presence is already committed in concept and roadmap — that part's settled. One architecture-layer question I need closed before I write the decision. When you say 'already watching,' are you picturing something that's always running quietly in the background, or something that wakes up on triggers — a calendar event, a new email, you opening your laptop? Both satisfy the feel from your side. They're very different underneath."

The operator answers in their own register.

> "Always on, but quiet. Like it's always there and it only speaks up when it has something worth saying. I don't want it talking at me all day. I want it present."

The interrogator clears. The operator gave Claude what it needs without stepping out of idea-space. Production runs. `architecture.md` gets *Active presence over destination app — persistent background process with discretion-gated surfacing.* Traces: `concept.md §The Idea`, `northstar.md` scene 2, this session's resolution.

The operator keeps going.

> "I keep going back to the Limitless thing. I don't want to build something that dies when a vendor changes their mind."

Interrogator again. Substrate independence is committed in `concept.md §What Already Exists — Limitless precedent`. Intent grounded. Implementation unspecified — the upstream doesn't commit to local-first state, or a model abstraction layer, or connector independence. All three are architectural layers where vendor lock-in could land, and all three have different costs.

> "Substrate independence is in concept — this isn't a new call, it's an activation. Architecture question: there are three layers lock-in could apply to — where state lives, where model calls go, where connector integrations live. All three can be made substrate-independent but you pay different costs for each. You can hold the line on all three. You can say state is the hill to die on and the others are allowed to couple to vendors if it makes R1 easier. Which one?"

> "State. Definitely state. Model — don't care for R1, swap it later if it becomes a problem. Connectors are going to change anyway."

Interrogator clears. `architecture.md`: *Local-first state; model and connector coupling permitted for R1.* R1 tradeoff surfaced by Claude as a consequence: no multi-device sync. Operator confirms in one line. Deferred to R2+.

Third line.

> "I've been living inside this project for a while and I don't want March to make me live inside another app. Whatever capture surface it has — I want it to be the stuff I'm already using."

Interrogator. Grounded in `concept.md`? Yes — the concept is explicit that March reaches into existing surfaces. Grounded in `roadmap.md` R1 scope? Yes — no new capture UI is already committed. Does the current session need to resolve anything new? No. The decision is already made upstream; Phase 3's job is to pull it forward as a binding constraint on architecture.

No question to the operator. `architecture.md`: *Integration-first capture, no March-owned input UI.* Specific R1 surfaces deferred to Phase 4.

Claude asks one logistics question: enumerate specific surfaces now or save for task breakdown. The operator says save it. *"I want to stay on the shape for now."* Claude stays on shape.

### What the session produced

Three architectural commitments in `architecture.md`, each traced to upstream artifacts plus (where relevant) this session's resolution. An R1 scoping tradeoff logged. A deferral to Phase 4. Two targeted questions asked by Claude, both answered by the operator in the register they came in with.

### What makes this the scene

The mechanism is visible. The operator stays in idea-space throughout because the interrogator does the translation work — it converts architecture-layer gaps into idea-space-answerable questions. When the operator says *always on, but quiet*, that is not an architecture decision in the operator's mind; it is a description of how March should feel. Claude treats it as sufficient input because the interrogator has already done the work of establishing that *always-on versus event-triggered* is the gap, and that the operator's answer — however registered — resolves it. The framework absorbs the translation. The operator's cost of committing architecture decisions is answering questions about how things should feel, not describing system patterns.

---

## Scene 3 — Execution Handoff, Runtime, and Release Validation (March R1, Phases 5–6, runtime, UAT)

### Setting

`spec.md` and the spec-level test plan for March R1 landed yesterday. Upstream artifacts sit in the project folder: `concept.md`, `northstar.md`, `mockup.md`, `roadmap.md`, `architecture.md` plus its release-level test plan, `spec.md` plus its spec-level test plan. No task breakdown exists yet. The operator has coffee.

### How Phase 5 goes

Claude runs the interrogator against the spec before writing. Most of the breakdown falls out chronologically; one sequencing question surfaces.

> "State layer and model abstraction are both foundational — either order works or they run in parallel. State first means the model layer persists against something when it comes online. Model first means you sanity-check the abstraction before committing state schema. Parallel is faster but the first integration is messier. Preference?"

> "State first. I want to see data moving before I worry about the model boundary."

Claude drafts the breakdown — state layer, model abstraction, background process, email connector, decomposition engine, surfacing logic, R1 smoke test — and drafts the task-level test plan in parallel by decomposing the spec-level plan downward. Two flags surface. A spec-level entry about auth expiry handling has no task-level child: missing task, added. A spec-level entry about surfacing discretion resists decomposition — it is a felt-experience check and moves to release-level UAT.

### The review gate

The operator reviews breakdown and test plan together. Three edits.

> "IMAP polling is too big for one session. Split into auth, fetch, parse."

> "Decomposition wiring comes before surfacing logic. You can't surface what you haven't decomposed."

> "Email connector test only checks that fetch succeeds. Also check auth expiry — I'll hit that in week two and I want to know early."

Claude applies all three. The third adds a test entry tracing upward to the spec-level robustness criterion previously under-tested. Operator approves.

### How Phase 6 goes

Phase 5 approval triggers Phase 6. Automatic derivation, no operator input. Claude walks every task, classifies it, and decomposes the task-level test plan downward into a work-package-level test plan for each package. March R1 is largely wire-up work — most tasks are human-executable; a small number require glue code and classify as machine-executable. Each package carries its test plan.

Claude surfaces one message:

> "Phase 6 complete. 3 machine packages in `work-packages/r1/machine/` — Todoist connector, model abstraction, email fetch adapter. 11 human tasks in Todoist under March / R1. Test plans attached at every level. Ready when you are."

### The first execution day

The operator opens their laptop. March is running from yesterday's setup. They open Todoist. At the top: _Configure Gmail OAuth — follow `work-packages/r1/human/gmail-oauth.md`._ They follow the file, paste the token, mark done. Next task: _Verify March reads the last 24 hours of inbox — run `verify-email-access.sh` and confirm against the work package test criterion._ They run it, confirm, mark done. Four tasks before coffee.

The machine track runs separately. The operator loaded the Todoist connector work package into a coding environment before starting Todoist work. A second Claude runs as supervisor against the work package test plan.

The coding Claude proposes a connector that hardcodes the API endpoint. The supervisor halts.

> "Work package test §3: connector endpoints must be config-driven. Traces from `architecture.md §Local-first state, swappable model layer` through `spec.md §External service coupling`. Hardcoded endpoint fails the test. Rework to read from config, or escalate that the architectural decision needs revisiting."

The coding Claude reworks. Supervisor re-runs the work package test plan. Pass. Log written to the work package file. The operator, on their fifth Todoist task, glances at the log, sees the connector came in clean, keeps going.

### Two weeks later — release validation

R1 is code-complete. Machine-checkable entries in the release-level test plan have been passing in CI throughout execution; what remains is UAT. The operator opens the release-level test plan. Entries are written in the register they were surfaced in during Phase 3, each tracing to the northstar scene it pays off. _Does March feel present without feeling loud? Does the morning open with a next move already waiting? Does capture happen through surfaces the operator was already using?_

The operator uses March for a week and marks the entries. Present-without-loud: pass, though one surfacing rule wants tuning — logged against R2. Morning-next-move: pass. Integration-first capture: pass. The release-level test plan closes. R1 is validated.

### What the session produced

A working release, validated at every level. Phase 5 produced a breakdown and task-level test plan in parallel, both passing the review gate. Phase 6 produced work packages and work-package-level test plans without operator intervention. The first execution day ran five human packages and one machine package to completion. Two weeks later the operator closed the release-level test plan with a felt-experience check against the northstar, logging one R2 item and passing the rest.

### What makes this the scene

The pipeline terminates in the register it started in. The operator's first statements in Phase 0 were about how March should feel; the last check on R1 was whether March felt that way. Between those moments the framework produced architecture, spec, breakdown, work packages, and code — none authored by the operator in production vocabulary — and a test plan at every level that made each production artifact checkable against its upstream.

The supervisor beat is where the scene earns its claim. The supervisor holds the work package test plan as its standard, which descends by decomposition from task-level, spec-level, and release-level, which descends from the northstar. The chain from felt experience to running code is traceable in both directions without the operator leaving idea-space. The UAT beat is where the chain closes: the operator's last act on R1 is marking a checklist they recognize, in the register they wrote it in, checking an experience they are currently having against an experience they described weeks ago.