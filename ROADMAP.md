---
artifact: roadmap
phase: 0
project: "[[Phased Decomposition]]"
release:
status: approved
created: 2026-04-13
framework_version: 0.1.0
descends_from:
  - "[[CONCEPT]]"
---
# Phased Decomposition — Release Roadmap

## Shape of This Roadmap

The roadmap maps the route from concept to completion as a sequence of releases. Each release is an independent, complete product — not an increment of a running system. The roadmap commits how many releases, what each one is, and in what order they ship. It does not activate any release; activation is Phase 2's job, carried by each release's own release artifact.

## Release Sequence

One release. R1 is the only release the phased decomposition project ships. Its terminal artifact is the phased decomposition skill in the form Anthropic's skill-authoring documentation specifies. There is no R2. The framework is complete when R1 ships.

A single release for this project has no bearing on release quantities for projects that use the skill.

- **R1 — The Phased Decomposition Skill.** An Anthropic-conforming skill encoding the framework. Binding standard, execution resourcing, scope, and validation are committed in [[RELEASE|R1's release artifact]].

## Changelog

- **2026-04-14 — Slimmed to Phase 0 scope.** Content that activated R1 — binding quality standard, execution resourcing, scope exclusions, definition of done, schedule stub, test plan stub — relocated to `RELEASE.md` as Phase 2 release-activation artifacts per the structural rule that release-level artifacts belong to the phase that defines the release. The roadmap retains the release sequence and points at the release artifact for activation detail. Phase field in frontmatter changed from unscoped to `0`. `descends_from` changed from `[[NORTHSTAR]]` to `[[CONCEPT]]` — the roadmap descends from the concept (what the project is) not from the northstar (what the experience looks like). Prior changelog entries below document the file's history before the slim-down.

- **2026-04-14 — Execution Resourcing corrected; release-level test plan and release schedule authored.** [Superseded by slim-down. Entry preserved for history.] Two changes in one amendment, both descending from project-notes entries graduated this week and the prior week's structural correction that release-level artifacts belong to Phase 2. Producer reframed from generic Claude Code subagent to skill-creator-class. Reference documentation shrunk from five sources to two. Release-level test plan and release schedule authored as sibling artifacts (`TEST-PLAN.md`, `SCHEDULE.md`).