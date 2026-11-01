# <project-name> <!-- omit from toc -->

[![image](https://img.shields.io/badge/-inside-blue?logo=ada&logoColor=white&labelColor=grey&logoSize=auto&style=flat-square)](https://ada-lang.io/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=flat-square)](https://opensource.org/licenses/Apache-2.0)
[![bbt](https://img.shields.io/badge/bbt-0_tests_passed_|_0_failed_|_0_empty-blue.svg?style=flat-square)](docs/tests_results/test_results.md)
<!-- replaced by the generated docs/tests_results/badge.svg after the first bbt run -->

<One sentence: what the project is, for whom.>

<One paragraph: the use case it serves, the boilerplate or problem it
removes.>

## Documentation

- As a curious reader: the [roadmap](docs/roadmap.md) — what the
  project is and plans;
- As a user: the [user documentation index](docs/README.md), which
  contains in particular the [changelog](docs/changelog.md) and the
  [feature pages](docs/features/) — the executable documentation;
  <!-- a project adds its user guide and its error codes here when
       it has them -->
- As a developer: the [maintainer documentation
  index](docs/dev/README.md), which contains in particular the
  [development workflow](docs/dev/development_workflow.md) and the
  [design discussions](docs/dev/design_discussions.md);
  <!-- when the project has a contributing guide, reference it here
       first -->

## How to get it

<!-- Adapt to the project's distribution: Alire crate, release
     download, AppImage, or build from source only. This section
     precedes Build and test. -->

Build from source:

```sh
git clone https://github.com/LionelDraghi/<project-name>
cd <project-name>
make build
```

## Build and test

```sh
make build   # build
make check   # build + bbt executable documentation
```

## Licence

Apache-2.0.
