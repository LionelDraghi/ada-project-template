---
name: propose
description: >-
  Use when the user asks to formalize, specify, propose, or prepare a
  change, or to write or update its bbt scenarios. Produces a candidate
  specification under docs/proposed/.
---

# Skill: propose

## Purpose

Turn a sufficiently understood need into a candidate specification under `docs/proposed/`.

## Trigger

Use when the user asks to formalize, specify, propose, prepare, or add scenarios for a change.

## Required companion skill

Before writing or editing executable steps, load the current bbt skill from its installed or referenced source.

## Load

- exploration outcome, if any;
- relevant durable feature documents;
- related design decisions and authoritative issue/discussion;
- `documentation_structure.md`;
- capability configuration from local `AGENTS.md`.

## Procedure

1. Select a descriptive file name under `docs/proposed/`.
2. Start from the project's `docs/proposed/proposal_template.md` (from the reference repository's `project/` tree).
3. Describe the need and intended outcome without duplicating issue history.
4. Define scope, non-goals, and acceptance criteria.
5. Add bbt scenarios for observable behavior that is understood.
6. If no bbt scenario applies, state why.
7. Identify applicable unit, SPARK, proof, or other verification.
8. Link design questions to design decisions rather than resolving them invisibly.
9. Run `bbt explain` or the configured equivalent on executable content when available.
10. Report open questions and whether the proposal is ready for Implement.

## Rules

- A proposal may concern a feature, bug, robustness issue, incompatible change, or executable documentation.
- A proposal may begin as prose and acquire scenarios as behavior becomes clear.
- A bug-reproduction scenario lives in `docs/proposed/` until Finalize moves it into the affected feature document as a permanent non-regression scenario.
- Do not call every change a feature.
- Do not include implementation details unless they are externally constraining or part of an accepted design decision.
- Do not make scenarios pass by describing current incorrect behavior.
- Keep one authoritative description of each expected behavior.

## Output

A candidate specification in `docs/proposed/`, plus a short readiness statement.
