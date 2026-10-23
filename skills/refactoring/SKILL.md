name: refactoring
description: Use when the user wants to refactor, clean up, restructure, or simplify code, or asks to hunt "bad smells", "code smells", or technical debt. Covers smell detection, prioritization, behavior-preserving refactoring moves, and step-by-step verification. Ada-aware: includes Ada-specific smells and uses the project's make targets as the safety net.
---

# Refactoring with bad-smell detection

Refactoring changes the structure of code without changing its
behavior. The non-negotiable rule: **never mix a refactoring with a
behavior change in the same change.** Refactor first in a separate,
verifiable step, or let the refactoring enable the change — never both
at once.

If the user asks only for a review, audit, or bad-smell hunt, stop after
detection and prioritization. Do not modify the code.

## Workflow

1. **Establish the safety net before touching anything.** Run the
   project's build and test targets (`make build`, `make test`,
   `make check`; `make prove` in SPARK projects). If the safety net is
   weak, report the gap before refactoring. Add focused characterization
   tests only when permitted by the user and when the existing behaviour
   can be observed unambiguously. Keep this preparation separate from
   the refactoring itself.
3. **Detect the smells** (catalog below). List what you find with
   locations, do not fix anything yet.
4. **Prioritize.** Fix the smells in or near the code you are about to
   change (campsite rule). Rank by: risk if left in place, benefit to
   the upcoming change, cost to fix. Do not refactor the whole codebase
   for its own sake.
5. **Refactor in small steps.** One move at a time.
   After each move, run the narrowest relevant verification. Run the
   complete project safety net before reporting the refactoring as
   complete. If a step fails to verify, revert that step and
   re-think; do not stack broken steps.
6. **Report.** Smells found, smells fixed, smells left alone on
   purpose, and what verification was run.

## Bad smells catalog

### Inside a subprogram

- **Long or multi-purpose subprogram** — the subprogram combines distinct
  responsibilities, contains independently meaningful stages, or cannot be
  named precisely. Consider extracting cohesive operations. Length alone is
  a signal, not sufficient evidence.
- **Duplicated code** — same logic, two places. Extract a common
  subprogram or a generic; duplicate knowledge is the smell, not the
  text alone.
- **Deep nesting / long conditional chains** — guard clauses (early
  return), extract function, replace nested conditionals with case or
  discrimination on a type.
- **Long parameter list / data clump** — parameters that travel
  together: introduce a record (or private type) holding them.
- **Flag argument** — a Boolean parameter that selects between distinct
  behaviours. Consider splitting the operation into separate subprograms
  or using an enumeration. Ignore booleans that represent genuine domain
  state, user preferences, or simple diagnostic options.
- **Magic numbers** — unexplained literals. Named constants, with the
  bound justified where the project requires it.
- **Comments as deodorant** — a comment explaining messy code. Refactor
  until the comment becomes redundant.
- **Dead code** — unreachable branches, unused declarations, commented
  code. Delete; the version control system remembers.
- **Mutable global state read or written silently** — the caller cannot
  see the side effect. Pass state explicitly; in SPARK projects this
  also poisons proof contexts.

### Between units

- **Divergent change** — one package changes for many different
  reasons. Split it along the reasons.
- **Shotgun surgery** — one change forces edits in many packages.
  Move code and data together into one unit.
- **Feature envy** — a subprogram mostly interested in another type's
  data. Move it there.
- **Switch / if-chain on a type tag** — replace with case on a
  discriminated record, or class-wide dispatch in tagged-type designs.
- **Inappropriate intimacy** — two packages delving into each other's
  private parts. Move functions, or extract the shared part.
- **Primitive obsession** — Integer/String used where a distinct type
  would carry the contract. In Ada, declare a new type with a range or
  a private type with explicit contracts; this is cheap and strong.
- **Speculative generality** — unused generics, parameters, hooks
  "for later". Remove; add them when needed.
- **Middle man / pass-through calls** — a unit that only forwards.
  Collapse the indirection.
- **Kitchen-sink package spec** — one spec declaring everything.
  Split into child packages along responsibilities.
- **Cyclic dependencies between packages** — a layering violation.
  Re-examine which unit owns which responsibility; `gnatmetric` /
  dependency analysis can confirm the cycle before you restructure.

### Ada-specific

- **`use` clauses hiding origins** — prefer `use type` for operators
  only; qualify the rest. Check the project's style rules first.
- **Access types where a plain object or parameter mode would do** —
  heap and aliasing complicate proofs and lifetime analysis. Prefer
  `in`/`in out` parameters and `in` access parameters over
  pointer-returning functions.
- **`when others => null`** — swallowed cases and errors. Handle the
  cases explicitly or assert impossibility with a check.
- **Side-effecting functions** — in Ada, functions should be pure
  (side effects break contracts and SPARK). Make it a procedure.
- **Aliased or global variables used as implicit parameter passing** —
  pass them; the contract becomes visible.
- **Copy-paste with small edits between sibling packages** — extract
  the common part into a parent or generic unit.

## Detection heuristics

- Read the diff or the target unit and ask: can I name this subprogram
  with subject-verb-object? Could I explain it without saying "and"?
- `gnatmetric` for length, complexity, and coupling signals.
- Textual search for markers: `when others`, `pragma Unreferenced`,
  `access`, `use` clauses, repeated literal clusters, duplicated line
  blocks.
- In SPARK projects, proof obligations that require ever-larger
  annotations to discharge are a smell: the code wants restructuring.

## Constraints in Lionel's projects

- Shared rules live in `~/prj/ada-project-template/AGENTS.template.md`:
  no commit without the owner's explicit consent, one refactor as one
  reviewed change, report failures instead of pushing through.
- Documentation is executable in these projects: when a refactoring
  changes user-visible behaviour presentation (help text, output
  formatting), update the Markdown scenarios in the same change — but
  a true refactoring changes no behaviour at all.
