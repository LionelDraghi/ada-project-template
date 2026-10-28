---
name: implement
description: >-
  Use when the user asks to implement, develop, fix, or make a scenario
  pass. Makes the smallest coherent change against the accepted
  proposal, and runs the project validations.
---

# Skill: implement

## Purpose

Make the code satisfy an accepted candidate specification or an unambiguous corrective task.

## Trigger

Use when the user asks to implement, develop, fix, or make a scenario pass.

## Load

- the active proposal or authoritative corrective source;
- affected feature specifications;
- local project commands and enabled capabilities;
- only the source files needed for the change;
- `ada-pro` when writing, reviewing or debugging Ada code, and the
  `gnatprove` skill before changing proof strategy.

## Procedure

1. Identify the active proposal and expected behavior.
2. Run the narrowest relevant check and establish its current result.
3. Confirm that a failure, when expected, represents the intended gap rather than a broken test.
4. Implement the smallest coherent change.
5. Run affected bbt scenarios.
6. Run applicable unit tests, SPARK checks, proofs, build checks, and static analysis.
7. Iterate without silently changing expected behavior.
8. If behavior is ambiguous or the proposal is wrong, return to Propose and make the change visible.
9. When behavior is correct, assess whether Refactor is warranted; otherwise proceed to Finalize.

## Rules

- Do not mix unrelated cleanup into implementation.
- Do not weaken checks to obtain green results.
- Preserve user changes and inspect the diff.
- Generated reports must be regenerated, not hand-edited.

## Verification responsibilities

- **bbt** for observable and end-to-end behavior: a behavior change
  should normally have at least one relevant scenario before
  implementation is treated as complete. The Markdown specification is
  the durable source.
- **Unit tests** for internal components, when they provide fast,
  focused evidence not duplicated meaninglessly by acceptance
  scenarios.
- **SPARK and formal proof**, when enabled: preserve the project's
  proof level and switches; run the configured GNATprove command for
  affected units; do not replace an unproved obligation with a test
  without an explicit decision; document justified limitations in the
  authoritative source.
- **Build, static analysis, and metrics**: run the configured project
  checks for affected code; generated reports are evidence, not
  hand-edited sources.

Implementation is complete only when the affected bbt scenarios, the
enabled unit tests, the SPARK analysis and proofs, and the build
checks pass — and never before `make build`, `make check`, and `make
prove` (where the project carries proofs) have been run and their
output read.

## Output

Code and focused verification evidence, followed by the recommended next phase.
