---
name: release
description: >-
  Use only on explicit intent to release, publish, tag, or bump a
  release version. Drives the shared release procedure with the
  project parameters.
---

# Skill: release

## Purpose

Execute the shared release procedure using project-specific parameters.

## Trigger

Use only on explicit intent to release, publish, tag, or bump a release version. A positive answer to the post-Finalize release question is explicit intent to prepare the release, but publication steps still follow the user's authorization and local procedure.

## Load

- `release_procedure.md`;
- local `AGENTS.md` release capabilities and parameters;
- local `docs/dev/release_procedure.md`;
- changelog and version sources;
- current validation and Git state.

## Procedure

Follow `release_procedure.md` in order. Stop on disagreement between version sources, failed validation, missing credentials, an existing tag, or unclear publication authorization.

## Output

Release version, completed steps, created artifacts and links, skipped targets, and remaining manual actions.
