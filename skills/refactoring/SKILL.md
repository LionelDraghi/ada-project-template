name: refactoring
description: Use when the user wants to refactor, clean up, restructure, or simplify code, or asks to hunt "bad smells", "code smells", or technical debt. Covers smell detection, prioritization, behavior-preserving refactoring moves, and step-by-step verification. Ada-aware: includes Ada-specific smells and uses the project's make targets as the safety net.
---

# Refactoring with bad-smell detection

Refactoring changes the structure of code without changing its
behavior. The non-negotiable rule: **never mix a refactoring with a
behavior change in the same change.** Refactor first in a separate,
verifiable step, or let the refactoring enable the change — never both
at once.

Catalog entries are heuristics, not automatic violations. Report a smell only
when there is concrete evidence in the code and explain that evidence. The
suggested refactorings are options, not mandatory transformations. Do not
apply a refactoring mechanically when it would weaken readability, contracts,
proof, performance, ownership, or the public API.

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
- **Deep nesting / long conditional chains** — look for mixed responsibilities
  or obscured control flow. Consider guard clauses, extracted predicates,
  `case` statements, or discriminated types. Do not introduce multiple exits
  when they make contracts or proof paths harder to understand.
- **Long parameter list / data clump** — parameters that travel
  together: introduce a record (or private type) holding them.
- **Flag argument** — a Boolean parameter that selects between distinct
  behaviours. Consider splitting the operation into separate subprograms
  or using an enumeration. Ignore booleans that represent genuine domain
  state, user preferences, or simple diagnostic options.
- **Magic numbers** — a literal whose meaning, unit, origin, or constraint
  is not evident locally. Introduce a named constant or a domain type when
  the name adds information. Do not replace self-evident structural values
  such as zero or one mechanically.
- **Comments as deodorant** — comments that compensate for unclear structure
  or restate what the code does. Refactor the code when practical, but retain
  comments that explain rationale, external constraints, proof strategy, or
  non-obvious invariants.
- **Dead code** — unreachable branches, unused declarations, commented
  code. Delete; the version control system remembers.
- **Hidden mutable global state** — state is read or modified without this
  dependency being visible in the abstraction or contracts. Pass it
  explicitly, encapsulate it behind a package abstraction, or declare precise
  SPARK `Global` dependencies..

### Between units

- **Divergent change** — one package changes for many different
  reasons. Split it along the reasons.
- **Shotgun surgery** — one change forces edits in many packages.
  Move code and data together into one unit.
- **Feature envy** — a subprogram mostly interested in another type's
  data. Move it there.
- **Repeated kind-code dispatch** — several units repeatedly branch on the
  same enumeration or kind field to select type-specific behaviour. Consider
  moving the behaviour to a discriminated abstraction or to primitive
  operations of a tagged type. A local, exhaustive `case` is not a smell by
  itself.
- **Inappropriate intimacy** — two packages delving into each other's
  private parts. Move functions, or extract the shared part.
- **Primitive obsession** — predefined scalar or string types are used where
  a domain concept requires distinct validity rules, units, operations, or
  contracts. Consider a derived scalar type, subtype, bounded abstraction,
  enumeration, or private type.
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

- **Broad `use` clauses obscuring declaration origins** — report only when
  visibility creates ambiguity or makes the code difficult to trace. Follow
  the project's style rules; `use type` and `use all type` may be appropriate
  for operators.
- **Unnecessary access types or aliasing** — use of access values where object
  ownership, optionality, dynamic allocation, or polymorphism is not required.
  Prefer ordinary `in`, `out`, or `in out` parameters when they express the
  intended lifetime and mutability. Do not report access types that model
  required ownership, optional references, dispatch, or data structures.
- **Unexplained `when others => null`** — potentially swallowed cases or
  errors. Handle cases explicitly, document an intentional no-op, or assert
  impossibility when justified. Do not replace an intentional no-op with an
  exception mechanically.
- **Surprising side effects in functions** — a function modifies externally
  visible state or performs actions that callers would not expect from a
  query. Prefer a procedure when mutation is part of the operation's purpose.
  Judge this against the project's Ada/SPARK contracts and restrictions
  rather than assuming every function must be pure.
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
- In SPARK projects, growing proof complexity is a signal to inspect the code's
  structure, contracts, data model, and missing intermediate assertions.
  Do not assume restructuring is always the correct remedy.

## Constraints in Lionel's projects

- Shared rules live in `~/prj/ada-project-template/AGENTS.template.md`:
  no commit without the owner's explicit consent, one refactor as one
  reviewed change, report failures instead of pushing through.
- Documentation is executable in these projects: when a refactoring
  changes user-visible behaviour presentation (help text, output
  formatting), update the Markdown scenarios in the same change — but
  a true refactoring changes no behaviour at all.
