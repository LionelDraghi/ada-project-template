---
name: finalize
description: >-
  Use when the user asks to integrate, finish, close, or finalize
  completed green work. Merges the proposal into the durable
  documentation, and refreshes the indexes.
---

# Skill: finalize

## Contents <!-- omit from toc -->

- [Purpose](#purpose)
- [Trigger](#trigger)
- [Load](#load)
- [Procedure](#procedure)
- [Rules](#rules)
- [Output](#output)

## Purpose

Turn successfully implemented work into integrated, durable project state.

## Trigger

Use when the user asks to integrate, finish, close, or finalize completed green work.

## Load

- active proposal;
- target feature documents;
- changelog, concerns, roadmap, and related design decisions;
- local phase completion criteria (shared lifecycle) and enabled validation commands;
- Git diff and status.

## Procedure

1. Confirm applicable validation is green.
2. Determine the durable destination of every piece of proposal content.
3. Merge behavior and scenarios into existing `docs/features/` documents or create an appropriate feature document.
4. For bug fixes, retain the reproducing scenario as a permanent non-regression scenario in the affected feature.
5. Update related design-decision status.
6. Update or remove the relevant concern and roadmap entries.
7. Update the Unreleased changelog when user-visible.
8. Remove the proposal after all durable content has been integrated.
9. Regenerate reports affected by the change.
10. Review documentation links, Git status, and complete diff.
11. Check the completion criteria of the completed phases (shared lifecycle).
12. Ask whether the user wants a release or to remain on the development version.

## Rules

- Do not simply move every proposal unchanged.
- Do not lose rationale that remains useful, but do remove temporary discussion already preserved by issues or Git history.
- Do not bump, tag, push, or publish during Finalize.

## Output

A finalization report listing integrated specifications, closed or
updated indexes, validation evidence, the Refactor arbitration for the
completed work (phase run, or explicitly dismissed and why), and
release readiness.
