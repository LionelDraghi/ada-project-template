---
name: explore
description: >-
  Use when the user wants to discuss an idea, problem, behavior,
  opportunity, scope, or unresolved requirement — before any
  specification or implementation.
---

# Skill: explore

## Contents <!-- omit from toc -->

- [Purpose](#purpose)
- [Trigger](#trigger)
- [Load](#load)
- [Procedure](#procedure)
- [Rules](#rules)
- [Output](#output)

## Purpose

Clarify a need before formal specification or implementation. Exploration is about the problem and desired outcome, not premature code design.

## Trigger

Use when the user wants to discuss an idea, problem, behavior, opportunity, scope, or unresolved requirement.

## Load

- the user's current request;
- relevant existing feature documents;
- related concerns, roadmap entries, issues, and open design decisions;
- no broad source-code scan unless needed to test a stated constraint.

## Procedure

1. Restate the need in outcome-oriented terms.
2. Identify users or systems affected.
3. Clarify current behavior and desired behavior.
4. Define scope and non-goals.
5. Identify success criteria and observable evidence.
6. Surface constraints and open questions.
7. Separate behavioral questions from later design choices.
8. Recommend Propose when the need is sufficiently clear.

## Rules

- Do not modify production code.
- Do not favor a solution only because it is easier to implement.
- Technical feasibility may be recorded as a constraint, not used to redefine the need silently.
- Do not create a document for trivial exploration unless persistence is useful.

## Output

A concise exploration summary containing need, outcome, scope, non-goals, success criteria, constraints, and open questions. If requested, use it to seed `project/docs/proposed/proposal_template.md`.
