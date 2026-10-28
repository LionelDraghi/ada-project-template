# Shared development lifecycle

This file details the shared lifecycle sketched in
[`AGENTS.md`](AGENTS.md): the phases, the skill to load in each, and
the completion criteria per phase. Read the phase you are entering,
not the whole file. Release has its own shared procedure,
[`release_procedure.md`](release_procedure.md).

## Entering a phase

Interpret intent, not only exact wording:

- "Let's discuss…", "I have an idea…", "How could…?" -> Explore.
- "Formalize it", "Write the spec", "Make a proposal" -> Propose.
- "Implement it", "Fix this bug", "Make the scenario pass" -> Implement.
- "Clean up the code", "Remove duplication", "Refactor" -> Refactor.
- "Integrate it", "Everything is green, finish it" -> Finalize.
- "Release", "Publish", "Bump the version" -> Release.
- "What do we do now?" -> use `skills/next-task/SKILL.md`.

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

Use `skills/implement/SKILL.md`. Load `ada-pro` when writing,
reviewing or debugging Ada code, and the `gnatprove` skill before
changing proof strategy.

Verification responsibilities:

- **bbt** for observable and end-to-end behavior. A behavior change
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
