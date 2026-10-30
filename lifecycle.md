# Shared development lifecycle <!-- omit from toc -->

This file details the shared lifecycle sketched in
[`AGENTS.md`](AGENTS.md): the phases, the skill to load in each, and
the completion criteria per phase. Read the phase you are entering,
not the whole file. Release has its own shared procedure,
[`release_procedure.md`](release_procedure.md). The standard project
tree is given in the
[README](https://github.com/LionelDraghi/ada-project-template#standard-project-tree);
the documentation structure — responsibilities, naming conventions,
proposal lifecycle, concerns, roadmap and design discussions — is
detailed in [`documentation_structure.md`](documentation_structure.md).

## Contents <!-- omit from toc -->

- [Entering a phase](#entering-a-phase)
- [State discipline](#state-discipline)
- [Explore](#explore)
- [Propose](#propose)
- [Implement](#implement)
- [Refactor](#refactor)
- [Finalize](#finalize)
- [Release](#release)

## Entering a phase

Interpret intent, not only exact wording:

- "Let's discuss…", "I have an idea…", "How could…?" -> Explore.
- "Formalize it", "Write the spec", "Make a proposal" -> Propose.
- "Implement it", "Fix this bug", "Make the scenario pass" -> Implement.
- "Clean up the code", "Remove duplication", "Refactor" -> Refactor.
- "Integrate it", "Everything is green, finish it" -> Finalize.
- "Release", "Publish", "Bump the version" -> Release.
- "What do we do now?" -> use `skills/next-task/SKILL.md`.

## State discipline

The working state is one of:

```text
Initial | Exploring | Proposing | Implementing | Refactoring | Finalizing | Releasing
```

Infer it from the user's instruction, current changes, active proposal,
and recent validation evidence. Do not create a state file solely to
track this value.

- Finish or explicitly suspend current work before selecting unrelated work.
- A return from Implement to Propose is normal when the expected
  behavior needs correction or clarification.
- Never silently weaken a scenario to make an implementation pass.
- Never move a candidate specification into `docs/features/` before
  Finalize.
- Never publish, tag, or bump a release version without explicit
  release intent.

## Explore

Clarify the need, desired outcomes, scope, non-goals, success criteria,
constraints, and open questions. Do not choose a solution merely
because it is easier to implement. Do not modify production code.

Use `skills/explore/SKILL.md`.

Explore is done when need, outcome, scope, non-goals, success criteria,
constraints, and open questions are explicit, and no production code
has been modified.

## Propose

Create or update a candidate specification under `docs/proposed/`.
Describe the intended change and add executable bbt scenarios when the
proposal concerns observable behavior and is sufficiently understood.

Use `skills/propose/SKILL.md`. Load the bbt skill before writing or
editing bbt syntax.

A proposal may begin as prose and acquire scenarios as behavior
becomes clear. A bug-reproduction scenario belongs temporarily in
`docs/proposed/`, then becomes a permanent non-regression scenario in
the relevant document under `docs/features/` at Finalize. If no
executable scenario applies, record the reason in the proposal.

Propose is done when the proposal states the need, the intended
outcome, the scope, the acceptance criteria, and its scenarios (or why
there are none), and the open questions are explicit.

## Implement

Start from an accepted proposal or an unambiguous corrective task.
Establish the relevant failing check when applicable, make the
smallest coherent code change, and run the validations enabled for the
project. Return to Propose if implementation reveals an ambiguity or
changes the expected behavior.

Use `skills/implement/SKILL.md`; it details the procedure and the
verification responsibilities — build, tests, analysis, proofs — per
change.

Implement is done when the affected bbt scenarios pass, the enabled
unit tests, SPARK analysis, proofs, and build checks pass, and the Git
diff contains only intentional changes — and never before `make
build`, `make check`, and `make prove` (where the project carries
proofs) have been run and their output read.

## Refactor

Improve structure without changing specified behavior. This phase is
conditional but explicit, so that implementation and restructuring do
not compete for the agent's context.

Use `skills/refactor/SKILL.md`.

Refactor is done when the stated structural objective is met, behavior
is unchanged (the scenarios still pass), and the full validation is
green.

## Finalize

Integrate the completed change into the durable documentation. Merge
the candidate specification into `docs/features/`, close or update its
sources, refresh indexes.

Use `skills/finalize/SKILL.md`.

Finalize is done when proposal content is integrated into durable
specifications and the proposal is removed, concerns, roadmap, design
discussions and changelog are updated where applicable, temporary
files and obsolete TODO/FIXME markers are removed, and the working
tree and file system are clean.

## Release

A finalized change does not automatically cause a release. Ask whether
a release is wanted. Run the release procedure only after an explicit
user decision.

Use `skills/release/SKILL.md` and
[`release_procedure.md`](release_procedure.md).

Release is done when the procedure's ten steps are complete, the
repository is back in development mode, and the links, artifacts, and
remaining manual actions have been reported.
