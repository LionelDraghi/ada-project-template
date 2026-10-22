# ada-project-template

Shared reference repository for Lionel's Ada projects (bbt, smk,
ArchiCheck, TopoProof, and the next ones).

```
ada-project-template/
├── AGENTS.template.md   shared agent instructions: non-negotiable
│                        git rules, procedures, task triage,
│                        reference policy, skills, definition of done
├── release_procedure.md the common release procedure (10 steps),
│                        parameterized by each project
└── skills/
    └── refactoring/     the refactoring and bad-smell skill
```

## How the projects use it

Each project's `AGENTS.md` references this repository instead of
duplicating it:

> The shared rules for Lionel's Ada projects live in
> `../ada-project-template/AGENTS.template.md` and apply here in full.

Each project's `docs/dev/release_procedure.md` gives the parameter
values and the project-specific additions; the common procedure lives
in `release_procedure.md` here.

## Skills

Locally-owned skills live in `skills/` here and are installed by
symlinking them into `~/.agents/skills/`:

```sh
ln -s ~/prj/ada-project-template/skills/refactoring ~/.agents/skills/refactoring
```

Third-party skills are installed from their GitHub sources with the
skills CLI (`npx skills`). The bbt skill is referenced from GitHub,
never copied here: `npx skills add LionelDraghi/bbt --skill bbt-skill`.

Change here, apply everywhere: a fix to a shared rule, to the common
release procedure, or to a locally-owned skill only needs one commit,
and every project picks it up on its next session.
