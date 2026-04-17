# Phase 6 Session Start Guide

## Before opening Claude Code

1. **Ensure the repo is up to date.** Pull latest from GitHub (`--ff-only`). All Phase 5 artifacts should be committed.

2. **Verify directory structure.** The repo should contain:
   ```
   project-root/
   ├── CLAUDE.md                           # Coordinator instructions (base + addendum appended)
   ├── .claude/agents/
   │   ├── producer.md                     # Producer subagent definition
   │   └── supervisor.md                   # Supervisor subagent definition
   ├── phased-decomposition-source/        # Preloaded skill for producer
   │   ├── SKILL.md
   │   └── references/                     # Byte-for-byte framing artifacts
   │       ├── CONCEPT.md
   │       ├── NORTHSTAR.md
   │       ├── MOCKUP.md
   │       ├── ROADMAP.md
   │       ├── RELEASE.md
   │       ├── ARCHITECTURE.md
   │       ├── SPEC.md
   │       ├── TEST-PLAN.md
   │       ├── SCHEDULE.md
   │       └── project-notes.md
   ├── phase5-artifacts/                   # Phase 5 terminal artifacts
   │   ├── work-instructions.md
   │   ├── execution-plan.json
   │   ├── todoist-tasks.json
   │   ├── evals/
   │   │   └── evals.json
   │   └── work-packages/
   │       ├── WP-01-01.md
   │       ├── WP-01-02.md
   │       ├── ... (23 files total)
   │       └── WP-08-01.md
   └── [all Phase 0–4 artifacts at root]
   ```

3. **Append the CLAUDE.md addendum.** The file `CLAUDE-addendum.md` contains a new section ("Phase 5 integration — how to read the work queue") that must be appended to the end of `CLAUDE.md`. This tells the coordinator how to read Phase 5's concrete output. Append it:
   ```bash
   cat CLAUDE-addendum.md >> CLAUDE.md
   ```
   Then remove the standalone addendum file.

4. **Install the preloaded skill.** The `phased-decomposition-source/` directory needs to be accessible as a Claude Code skill. The producer's `skills:` frontmatter already lists `skill-creator` (Anthropic-bundled). The source artifacts are delivered by the coordinator per-spawn via the work package inputs — but having them as a skill means the producer can `view` them directly.

   For Claude Code, place the skill directory where Claude Code discovers skills (check your Claude Code configuration — typically the project root or a configured skills path).

## Opening Claude Code

When you start the session, Claude (as coordinator) will:

1. Read `CLAUDE.md` (including the appended Phase 5 integration section)
2. Ask you to set walls:
   - Iteration bound per work package (suggested: match the `iteration_bound` in `execution-plan.json` per package, or set a blanket cap)
   - Session length bound (suggested: number of work packages to close before returning for review)
   - Escalation mode (halt-on-escalation or queue-and-continue)
3. Confirm Todoist project name for the human work package
4. Initialize or resume `phase6-state.json`
5. Begin spawning against the first eligible work package

## Suggested wall settings for first session

- **Iteration bound:** Use per-package bounds from `execution-plan.json` (range: 3–5)
- **Session length:** 5 work packages (covers T-01 and T-02 — eval suite and SKILL.md body)
- **Escalation mode:** Halt on escalation (first session, want to see failure shapes)

## What to expect

The dependency graph sequences work as:
1. WP-01-01, WP-01-02 (eval suite — no dependencies, can run in parallel)
2. WP-02-01 (SKILL.md body — depends on eval suite)
3. WP-03-01 (phase-0 reference — depends on SKILL.md body)
4. WP-03-02 through WP-03-07 (remaining references — depend on WP-03-01, can run in sequence)
5. WP-04-01 through WP-04-06, WP-05-01 through WP-05-04 (assets — depend on respective reference files, largely parallel)
6. WP-06-01 (description optimization — depends on SKILL.md body + trigger evals)
7. WP-08-01 (cross-reference pass — depends on all content being done)
8. WP-07-01 (Todoist load — post-gate, human)

First session should comfortably close T-01 and start T-02. If the SKILL.md body closes in the first session too, that's a good stopping point — it establishes the foundation everything else builds on.

## Todoist API key

Default: `8df1ae04dfe2257231be1df93671bb23dc178774`

Confirmed during Phase 5 planning.
