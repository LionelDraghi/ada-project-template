<!-- omit from toc -->
# Release procedure (shared)

This is the common release procedure for Lionel's Ada projects. Each
project's `docs/dev/release_procedure.md` provides the parameter values
and the project-specific additions; read both documents together. On
any conflict, the project file wins.

This document is the single reference for:

- selecting a release version;
- preparing and validating a release;
- committing the release state;
- creating the Git tag and GitHub release;
- publishing the release to Alire;
- returning the repository to development mode.

Do not commit, push, tag, create a GitHub release or publish to Alire
without the owner's explicit approval. No step below overrides this.

## Parameters

Every step below refers to the project's parameter table, which fixes:

- the changelog file, its development-section label, and its
  categories (`[Added]`/`[Changed]` or `### Added`/`### Changed`);
- the complete local validation command, and the release-mode build
  command when the project has one;
- the version-check command, and the file where `alr update`
  regenerates `Crate_Version`;
- the clean command;
- the remote name;
- the GitHub releases page;
- the CI platforms and the required GitHub Actions checks;
- the Alire platform availability (the `[available]` restriction in
  `alire.toml`, if any).

## 1. Release prerequisites

A release publishes what the current development section of the
changelog describes.

Before starting the release procedure, verify that:

- the intended development work is complete;
- the current development changelog section is complete and accurate;
- all development changes have been reviewed, committed and pushed;
- the working tree is clean, on the file system and not only through
  `git status`;
- the current branch is the intended release branch;
- a complete local validation has succeeded;
- the latest required GitHub Actions checks have succeeded on every
  platform where the crate is declared available.

Check the repository state:

```sh
git status
```

Also inspect the file system for Git-ignored build or test artefacts.
Do not start a release from a dirty or partially generated repository
state.

Platform availability, and the Alire index tests: `alr publish` exposes
the crate to the community index tests, which build the crate and run
its test action on **every platform where the crate is declared
available**. A crate published while a platform build or test fails is
published broken. In particular:

- never publish until the crate builds and its tests pass on every
  platform declared available;
- never extend the `[available]` restriction to another platform
  before the build and the tests have been validated on it (through
  CI or manual runs); keep `alire.toml` and the project docs in sync;
- check that the test action can run on the community index machines:
  it may require external tools that they do not have.

## 2. Select the release version

Do not select the release number by simply removing the development
status. Review the current development changelog section and propose
the release version according to SemVer:

- an Added entry requires a MINOR version bump;
- a backward-compatible Changed entry requires a MINOR version bump;
- a release containing only backward-compatible bug fixes keeps the
  PATCH bump;
- any incompatible change must be assessed explicitly according to
  SemVer.

Before changing any file:

1. identify the current development version;
2. review the current changelog entries;
3. propose the release version;
4. explain the SemVer reasoning;
5. request explicit owner confirmation.

Do not change the version before it has been confirmed. The prerelease
suffix must be dash-separated:

```text
x.y.z-dev
```

The dot-separated form (`x.y.z.dev`) prevents Alire from loading the
workspace.

## 3. Set the release version

After the owner confirms the version, update it in:

```text
alire.toml
```

Then run:

```sh
alr update
```

`alr update` regenerates `Crate_Version` in the generated Alire
configuration; running `alr build` alone does not regenerate it. The
generated configuration is version-controlled like the rest of the
sources. Verify that the regenerated `Crate_Version` matches the
confirmed release version, and that the project's version-check
command reports it.

Search for other version strings that must be updated, including where
applicable version strings in the sources, examples and generated
documentation. Do not replace version-independent expressions, regular
expressions or examples unnecessarily.

## 4. Close the release changelog section

In the changelog:

1. verify that the current development section accurately describes
   the release;
2. retitle that section with the confirmed release version;
3. set the release date;
4. preserve the existing changelog structure and categories;
5. keep entries concise and user-oriented;
6. ensure the latest entry appears at the top of its relevant list;
7. remove obsolete placeholders.

The release notes must reflect the actual content of this changelog
section.

## 5. Validate the release locally

Run the complete local validation. If any part fails:

1. stop;
2. do not commit, push or tag;
3. fix the issue through the normal development workflow;
4. restart release validation from a clean state.

Verify that the version-check command reports the confirmed release
version exactly, without the `-dev` suffix, and that the release-mode
build works when the project has one.

Then clean the repository and inspect both Git state and the file
system:

```sh
git status
git diff
git diff --staged
```

Verify that:

- no unwanted build or test artefact remains;
- no Git-ignored artefact remains;
- the release version is correct and `Crate_Version` was regenerated;
- the changelog section is closed correctly;
- all required generated results, badges, indexes and documentation
  are consistent;
- only intended release changes remain.

## 6. Confirm and commit the release state

Before staging, present a release-preparation report containing:

- the previous development version;
- the proposed release version;
- the SemVer reasoning;
- the changelog entries included;
- the result of `alr update` and of the version check;
- the result of the local validation;
- the result of the cleanup;
- the files to be committed;
- the proposed commit message.

Request explicit owner approval before staging, committing or
pushing.

After approval, stage the complete release state, including all
intended generated files, review the staged state, then commit and
push. Synchronise with the remote branch before pushing:

```sh
git pull --rebase
```

If a rebase conflict affects generated test results, badges or similar
generated files, keep the local version produced by the latest
complete run. If a conflict affects source code, documentation or
another non-generated file, stop and ask the owner to arbitrate.

## 7. Validate the release commit on GitHub

After pushing, verify that GitHub Actions is testing the intended
release commit, and that all required checks have succeeded on every
platform where the crate is declared available.

Do not create or push the release tag while a required check:

- is pending;
- has failed;
- has been cancelled;
- was run against a different commit: the pushed release commit must
  be exactly the locally validated one.

If a correction is required, return to the development workflow,
implement and validate the fix, then restart the release procedure
from a clean state.

## 8. Tag and publish the GitHub release

Once all required GitHub Actions checks are successful, prepare:

- the validated release commit;
- the confirmed release version;
- the proposed tag;
- the release notes derived from the changelog.

The tag format is:

```text
x.y.z
```

Request explicit owner approval before creating or publishing the tag.
After approval, create the tag, push it to the remote, and create the
GitHub release from that tag on the project's releases page,
reformatting the released changelog section as release notes.

Before considering the GitHub release complete, verify that:

- the tag name is correct;
- the tag points to the validated release commit;
- the release notes match the changelog;
- the GitHub release is published from the correct tag.

Attach the release assets (exe, AppImage) when the project's download
page expects them.

## 9. Publish to Alire

Publish to Alire only after the GitHub release and tag are available.

Before publication, verify that:

- `alire.toml` contains the released version, and the generated
  `Crate_Version` the same version;
- `alire.toml` pins nothing to a local path: verify that
  `grep "path =" alire.toml` returns nothing. During development some
  dependencies may be pinned to sibling directories; a crate
  published with a local pin is broken for its users. If a pin
  carries a fix not yet available in the index, release that crate
  first;
- the Git tag is available remotely;
- the GitHub release is published;
- the required GitHub Actions checks have succeeded, and the crate
  builds and its tests pass, on every platform declared available;
- the local repository is clean.

The publication command is:

```sh
alr publish
```

Refer to the
[Alire project publication documentation](https://alire.ada.dev/docs/#publishing-your-projects-in-alire)
when needed.

Before running `alr publish` or submitting any publication change,
present the intended action and request explicit owner approval.

After publication, report:

- the published version;
- the Git tag used;
- the publication or pull-request reference;
- the Alire validation result, including the community index build and
  test results on the available platforms;
- any requested correction.

Do not silently alter an already published GitHub release to resolve
an Alire publication issue.

## 10. Return to development mode

After the GitHub and Alire publication steps are complete, propose the
next development version. The new version must use the dash-separated
`-dev` suffix, for example:

```text
0.4.1 -> 0.4.2-dev
```

The appropriate next development version depends on the planned work.
Request explicit owner confirmation before changing it.

After confirmation:

1. update the version in `alire.toml`;
2. run `alr update` and verify the regenerated `Crate_Version`;
3. open a new development section in the changelog, with an undefined
   date;
4. update the other explicit development-version mentions;
5. run the complete local validation;
6. clean and inspect the repository;
7. request approval before staging, committing and pushing.

After approval, commit and push the complete development state.

The repository is back in development mode only when:

- the new development version has been committed;
- the commit has been pushed;
- the working tree and file system are clean.
