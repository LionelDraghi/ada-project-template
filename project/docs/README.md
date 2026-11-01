# docs — the user-facing documentation <!-- omit from toc -->

This directory holds what the users need to know about
`<project-name>`: the roadmap, the specifications, and the executable
documentation. It is not maintainer-facing documentation: that lives
in [`docs/dev/`](dev/) — the two indexes point to each other, so if
you got lost, follow that way.

## In this directory

| File | Role |
|---|---|
| [roadmap.md](roadmap.md) | the prioritised development plan |
| [specifications.md](specifications.md) | the specification structure and the index of features |
| [features/](features/) | the feature pages: the executable documentation, exercised by their bbt scenarios |
| [proposed/](proposed/) | candidate changes awaiting implementation or integration |
| [tests_results/](tests_results/) | the test results and badges, generated and committed |
| [changelog.md](changelog.md) | notable changes from a user perspective, newest first |

<!-- a project adds its user guide and its error codes here when it
     has them -->

The shared rules — responsibilities, naming conventions, proposal
lifecycle, changelog conventions — live in the reference repository,
[`documentation_structure.md`](https://github.com/LionelDraghi/ada-project-template/blob/main/documentation_structure.md),
and apply here in full.
