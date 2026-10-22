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

## Build pointers (shared)

Every project Makefile is self-documented: `make help` lists the
targets with a short description, and is the documentation of record
for project-specific targets. The standard targets and their
semantics:

- `make help`: list the targets.
- `make build`: validation-mode build (`alr build --validation`);
  warnings, including style ones, are errors.
- `make release`: release-mode build (`alr build --release`) — a pure
  build: it runs no test and installs nothing.
- `make check`: run the project's tests — it depends on the tools and
  on the executable. The executable may be the validation one or the
  release one: the tests must pass with both.
- `make doc`: regenerate the generated documentation (the fixme index
  in `docs/dev/fixme_index.md`, cmd_line.md, badges, ...). The
  generated results are part of the repository state, not published
  by the CI.
- `make all`: build, check and doc. Do not list the tools in `all`:
  `check` already depends on them.
- `make clean`: remove the build and test artifacts.
- `make distclean`: remove everything that can be rebuilt, binaries
  included.
- `make install`: copy the binaries to `~/bin`.

A project deviates from these semantics only where the deviation is
explicitly stated in its own `AGENTS.md`.

## Standard project tree (shared)

Every project follows this layout, stating its deviations in its own
`AGENTS.md`:

- root: `AGENTS.md`, `README.md`, `LICENSE`, `Makefile`, `alire.toml`,
  `<project>.gpr`;
- `docs/`: user documentation, with:
  - `docs/changelog.md` — the changelog; its development section is
    `[x.y.z-dev]`;
  - `docs/features/` — the bbt scenario pages, the executable
    documentation: a must-have;
  - `docs/tests_results/` — the test results and badges, generated and
    committed; the tests themselves are not (necessarily) there;
  - `docs/dev/` — `development_workflow.md`,
    `release_procedure.md` (parameters and project additions),
    `design_discussions.md`, `fixme_index.md`;
- `src/` — the sources, with the generated `src/Alire_config/`
  version-controlled;
- `tests/` — the bbt run directory; it may also host optional suites
  (`sanity/`, `non_reg/`) and the unit tests;
- `tools/` — the tool sub-projects, when any;
- `.github/workflows/` — the build-and-test workflow
  (`build-test.yml`), and `appimage.yml`, a continuous AppImage
  workflow for every project that ships an executable; a library
  publishes no AppImage.

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

Before considering a change complete, the project's build and test
targets must pass (`make build`, `make check`, and `make prove` where
the project carries proofs). Never claim success without having run
and read the corresponding commands.
