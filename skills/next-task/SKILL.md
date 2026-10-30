---
name: next-task
description: >-
  Use for "What next?", "What should we work on?", "What is the next
  step?", or equivalent. Reports the active work, or recommends the
  next task from the concerns and the roadmap.
---

# Skill: next task

## Contents <!-- omit from toc -->

- [Purpose](#purpose)
- [Trigger](#trigger)
- [Procedure](#procedure)
  - [A. Determine whether work is active](#a-determine-whether-work-is-active)
  - [B. Initial state](#b-initial-state)
- [Rules](#rules)

## Purpose

Answer “What do we do now?” without losing active work or mixing concerns and roadmap additions.

## Trigger

Use for requests such as “What next?”, “What should we work on?”, “What's the next step?”, or equivalent wording.

## Procedure

### A. Determine whether work is active

Inspect the conversation, working tree, active proposal, unfinished checks, and current branch context.

If work is active, report:

```markdown
## Current work

- Task: ...
- Phase: ...
- Latest verified result: ...
- Next concrete action: ...
- Remaining validation: ...
```

Recommend completing or explicitly suspending that task before selecting another. Do not rank unrelated work unless the current task is explicitly suspended or blocked.

### B. Initial state

If no work is active, inspect authoritative sources.

Concerns, in order of relevance:

1. failing CI, build, tests, or proofs;
2. P1/P2 entries in `docs/dev/concerns.md`;
3. open blocking issues and pull requests;
4. generated FIXME report;
5. open design decisions and recorded technical debt;
6. the project's limitations document.

Roadmap additions:

1. P1/P2 entries in `docs/roadmap.md`;
2. ready proposals in `docs/proposed/`;
3. remaining authoritative development plan;
4. future design discussions.

Present:

```markdown
## Recommended next work

### 1. Priority concerns

- <item>: <why now> — <authoritative link>

### 2. Priority roadmap additions

- <item>: <why now> — <authoritative link>
```

## Rules

- Point to source material; do not copy entire discussions.
- Explain priority using severity, blockage, user value, dependency, or readiness.
- Never invent priority when none is recorded; label your recommendation as an assessment.
- Concerns are presented before roadmap additions, but the user chooses the task.
