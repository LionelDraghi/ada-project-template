# ------------------------------------------------------------------------------
# <project name> (<project url>)
# SPDX-License-Identifier: <license declared in alire.toml>
# SPDX-FileCopyrightText: <year>, <author>
# ------------------------------------------------------------------------------
#
# Template Makefile of Lionel's Ada projects. Copy it at the project
# root, replace the placeholders, and adapt the project-specific parts.
# The target semantics are fixed by the shared AGENTS.md (Build
# pointers): make help lists all the targets with a short description,
# and is the documentation of record — this header does not duplicate
# it, it only states where the template applies the shared rules.

PLATFORM := $(shell uname -s)

ifeq ($(OS), Windows_NT)
  # uname reports MSYS_NT-<version> on Windows: the platform directory
  # name must not change with the MSYS or Windows version number, so
  # the platform is normalized here.
  PLATFORM := Windows
  EXE_SUFFIX := .exe
else
  EXE_SUFFIX :=
endif

# The local test results (JUnit XML, results indexes, badges) land in
# a profile-suffixed directory, to be diffed in the editor against the
# last committed run of the same profile (cf. AGENTS.md, Test results).
# This whole level is git-ignored: only CI runs are recorded, staged
# by the CI workflows with git add -f.
RESULTS_ROOT := docs/tests_results/$(PLATFORM)
RESULTS_DIR  ?= $(RESULTS_ROOT)/validation

# The executable under test is a selection parameter, not a constant:
# the same test replay applies to any built variant — validation,
# release or instrumented — and its results land in the matching
# profile results directory.
EXE_UNDER_TEST ?= <path/to/the-built-main>$(EXE_SUFFIX)

.SILENT:

.PHONY : help all build check test-run build-release release doc clean

all: build check doc

help:
	echo "Usage: make [target]"
	echo ""
	echo "Targets:"
	echo "  all        : build, check and doc (default when no target given)"
	echo "  build      : build in validation mode (warnings, including"
	echo "               style ones, are errors)"
	echo "  check      : build, then run the test suites on"
	echo "               $(EXE_UNDER_TEST), writing the results under"
	echo "               $(RESULTS_DIR)"
	echo "  <id>       : replay a single test scenario by id, delegated"
	echo "               to tests/ (same options as check)"
	echo "  release    : complete release chain: clean, full recompilation"
	echo "               in release mode, full replay of the tests on the"
	echo "               release executable (results under"
	echo "               $(RESULTS_ROOT)/release); only a green replay"
	echo "               unlocks the release"
	echo "  doc        : regenerate the generated docs (fixme index, cmd"
	echo "               line reference, result or performance graphs, ...)"
	echo "  clean      : remove the build and test artifacts, including"
	echo "               the local results directories"
	echo ""
	echo "Local results are git-ignored: diff them against the last"
	echo "committed run of the same profile. Outputs are comparable, run"
	echo "times are not (two machines never agree) — for real measure-"
	echo "ments, same machine and repeated runs, cf. release_procedure.md."
	echo ""
	echo "Refer to AGENTS.md and docs/dev/development_workflow.md for more details."

build:
	echo
	echo === building for validation
	alr --non-interactive build --validation
	echo OK
	echo

build-release:
	echo
	echo === building for release
	alr --non-interactive build --release
	echo OK
	echo

# The tests/ directory owns the scenarios: it receives the executable
# under test and the results directory as parameters. Fill in the replay
# recipe inside tests/Makefile, writing the --junit output under
# $(RESULTS_DIR).
test-run:
	$(MAKE) --directory=tests \
	  EXE_UNDER_TEST="$(EXE_UNDER_TEST)" RESULTS_DIR="$(RESULTS_DIR)" test-run

check: build test-run

# A profile never reuses another profile's results: the release chain
# always starts from a complete clean, rebuilds in release mode, and
# replays the whole suite on the release executable (cf.
# release_procedure.md, step 5).
release:
	$(MAKE) clean
	$(MAKE) build-release
	$(MAKE) RESULTS_DIR=$(RESULTS_ROOT)/release test-run

# Same replay for a single scenario: make <id> delegates to tests/,
# with the same parameters as check.
%:
	$(MAKE) --directory=tests \
	  EXE_UNDER_TEST="$(EXE_UNDER_TEST)" RESULTS_DIR="$(RESULTS_DIR)" $@

# Performance measurements are part of the documentation: when a
# project needs result or performance graphs, they belong in its
# documentation target (make doc), next to the other generated docs,
# not in an ad hoc target of this template (cf. AGENTS.md: measure-
# ments are taken on release executables, same machine, repeated
# runs).

# The generated documentation. The fixme index generation is included,
# identical on Windows and Unix thanks to LC_ALL=C sort (one grep -i
# pass: it catches Fixme:, FIXME: and fixme:, and the sed strips the
# matched prefix). Fill in the rest with the project's generation
# commands (command line reference, results and performance graphs,
# ...) and extend the grep scope with the project's extra source
# directories (e.g. tools/).
doc:
	echo
	echo === regenerating docs/dev/fixme_index.md
	echo '# FIXME index' > docs/dev/fixme_index.md
	echo >> docs/dev/fixme_index.md
	echo 'Generated by `make doc` — do not edit by hand; regenerate instead.' >> docs/dev/fixme_index.md
	echo >> docs/dev/fixme_index.md
	grep -rni 'Fixme:' docs/* src/* \
	  | LC_ALL=C sort \
	  | sed -E "s/:/|/2;s/[Ff][Ii][Xx][Mm][Ee]://;s/(^[^:]*)/[\1](\.\.\/\.\.\/\1)/" \
	  > fixme_entries.tmp
	if [ -s fixme_entries.tmp ]; then \
	  echo 'Location | Text' >> docs/dev/fixme_index.md; \
	  echo '---------|-----' >> docs/dev/fixme_index.md; \
	  cat fixme_entries.tmp >> docs/dev/fixme_index.md; \
	else \
	  echo '(No entries.)' >> docs/dev/fixme_index.md; \
	fi
	rm -f fixme_entries.tmp
	echo OK
	echo

clean:
	echo
	echo === cleaning
	$(MAKE) --directory=tests clean
	alr --non-interactive clean
	rm -rf $(RESULTS_ROOT)/validation $(RESULTS_ROOT)/release
	echo OK
	echo
