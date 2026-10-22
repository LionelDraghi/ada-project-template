# Lionel's Ada project template — shared agent instructions

This file is the shared instruction template for Lionel's Ada projects
(bbt, smk, ArchiCheck, TopoProof, and the next ones). Each project's own
`AGENTS.md` references this template and keeps only what is specific to
that project.

## How to use this template

In a new project's `AGENTS.md`, start with:

> The shared rules for Lionel's Ada projects live in the [reference
> repository on GitHub](https://github.com/LionelDraghi/ada-project-template/blob/main/AGENTS.template.md)
> and apply here in full.

Then keep only the project-specific sections (build pointers, testing
tools, domain rules, conditional readings). Do not copy the shared
sections below into the project: point to them, so that a fix here
applies everywhere.

---

## Non-negotiable rules (shared)

- Never stage, commit, push, tag, create a release or publish to Alire
  without the owner's explicit consent. `git add` is as forbidden as
  `git commit` and `git push`.
- The consent is a separate, explicit approval given by the owner AFTER
  reviewing the diffs; it may come in a later message. A request in the
  task ("let's push this") states the goal, not the consent. When in
  doubt, wait.
- An explicit "go" for commit or push approves all remaining modified
  files as well.
- If a build, test, documentation, cleanup or publication step fails,
  stop and report the failure.
- Do not include unrelated changes, temporary files or test artifacts.
- Check repository cleanliness on the file system, not only with
  `git status`: Git-ignored files (`obj/`, `*.o`, `*.ali`, test
  directories, symlinks rebuilt by the test scenarios...) remain
  invisible to Git.
- Never modify a golden file (`expected_*`, reference output) without
  the owner's agreement.

## Choose the right procedure (shared)

- Development work, bug fixes, doc updates, tests, cleanup, review:
  follow `docs/dev/development_workflow.md`
  (TopoProof: `docs/dev/development.md`).
- Release versioning, tagging, GitHub release, Alire publication:
  the **release procedure is common** and lives in this repository,
  `release_procedure.md`. Each project's `docs/dev/release_procedure.md`
  gives the parameter values (changelog, validation command, remote,
  CI checks, ...) and the project-specific additions.

## Task triage (shared)

When the user asks "what should we do now?" or "what is the priority
right now?", check the project's tracked work before proposing new work:

- the fixme index for actionable fixes and cleanup items;
- the limitations / todo document for known bugs and limitations;
- the design discussions for decisions that constrain the next step;
- the TDL / project document for the current directions.

Use these sources to ground the answer in the repository's existing
work. Do not invent a new task before checking whether the need is
already tracked elsewhere.

## Reference policy (shared)

References are reading conditions, not a reading list. Never open all
documents up front: identify which document the current task requires,
state why, then read only that one.

Shape each reference as: "To <goal>, read <file>." A project's
`AGENTS.md` must not contain an unconditional "read first" list; if a
document is truly always needed, it belongs in the mandatory rules
instead.

## Skills (shared)

Load the matching skill before the corresponding work:

- `ada-pro` when writing, reviewing or debugging any Ada code;
- `gnatprove` for SPARK contracts, proofs, and proof campaigns;
- `refactoring` when improving code structure or hunting bad smells;
- `code-review` to review changes since a fixed point.

Where the skills live:

- Locally-owned skills live in this repository, in `skills/<name>/`.
  Each machine installs them by symlinking the folder into
  `~/.agents/skills/` (for instance
  `ln -s ~/prj/ada-project-template/skills/refactoring ~/.agents/skills/refactoring`),
  so that an update here applies everywhere.
- Third-party skills are installed from their GitHub sources with the
  skills CLI (`npx skills`), and stay managed by it. The reference
  sources are:
  - `ada-pro`: https://github.com/RREE/Ada-Pro
    (`npx skills add RREE/Ada-Pro`)
  - `gnatprove`: https://github.com/adacore/skills
    (`npx skills add adacore/skills --skill gnatprove`)
  - `code-review`: https://github.com/mattpocock/skills
    (`npx skills add mattpocock/skills --skill code-review`)
  - `find-skills`: https://github.com/vercel-labs/skills
    (`npx skills add vercel-labs/skills --skill find-skills`)
- The **bbt skill** is referenced from GitHub, never copied here:
  `npx skills add LionelDraghi/bbt --skill bbt-skill`
  (source: https://github.com/LionelDraghi/bbt). Load it before
  writing, converting or debugging bbt scenarios.

## Definition of done (shared)

Before considering a change complete, the project's build, test and
check targets must pass (`make build`, `make test`, `make check`, and
`make prove` where the project carries proofs). Never claim success
without having run and read the corresponding commands.
