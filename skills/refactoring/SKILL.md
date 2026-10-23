---
name: refactoring
description: >-
  Use when the user wants to refactor, clean up, restructure, or simplify
  code, or asks to identify bad smells, code smells, or technical debt.
  Covers evidence-based smell detection, prioritization, behaviour-preserving
  refactoring, and step-by-step verification. Ada-aware and SPARK-aware:
  includes language-specific signals and uses the project's build, test,
  analysis, and proof targets as the safety net.
---

# Refactoring with bad-smell detection

Refactoring changes the structure of code without intentionally changing its
observable behaviour.

The non-negotiable rule is:

> Never mix a refactoring with a behaviour change in the same change.

Refactor first in a separate, verifiable step, or let the refactoring enable a
later behaviour change.

## Operating principles

Catalog entries are heuristics, not automatic violations.

Report a smell only when there is concrete evidence in the code, and explain
that evidence. Length, syntax, metrics, and textual patterns are signals, not
proof by themselves.

Suggested refactorings are options, not mandatory transformations. Do not
apply them mechanically when they would weaken readability, contracts, proof,
performance, ownership, compatibility, or the public API.

Prefer the smallest refactoring that addresses the observed problem.

## Workflow

1. **Determine the requested scope.**

   If the user asks only for a review, audit, or bad-smell hunt, stop after
   detection and prioritization. Do not modify the code.

2. **Establish the safety net.**

   Before modifying code, identify and run the relevant project targets, such
   as:

   - `make build`
   - `make test`
   - `make check`
   - `make prove` in SPARK projects

   Also identify narrower unit, test, analysis, or proof commands when the
   project provides them.

   If the safety net is weak, report the gap before refactoring. Add focused
   characterization tests only when permitted by the user and when existing
   behaviour can be observed unambiguously. Keep this preparation separate
   from the refactoring.

3. **Detect the smells.**

   Inspect the requested diff, unit, package, subsystem, or codebase using the
   catalog below. Report each finding with its location and concrete evidence.
   Do not fix anything during this step.

4. **Prioritize.**

   Prefer smells in or near the code being changed: apply the campsite rule,
   not a general cleanup campaign.

   Rank candidates by:

   - risk if left in place;
   - benefit to the intended change;
   - confidence in the diagnosis;
   - cost and risk of the refactoring.

5. **Refactor in small steps.**

   Apply one coherent refactoring move at a time.

   After each move, run the narrowest relevant verification. Run the complete
   project safety net before reporting the refactoring as complete.

   If a step fails verification, revert or correct that step before
   continuing. Do not stack changes on a broken intermediate state.

6. **Report.**

   Report:

   - smells found, with evidence;
   - smells fixed;
   - smells deliberately left unchanged, with reasons;
   - refactoring steps performed;
   - verification commands run and their results;
   - remaining risks or safety-net gaps.

## Bad-smell catalog

### Within a subprogram

- **Long or multi-purpose subprogram** — the subprogram combines distinct
  responsibilities, contains independently meaningful stages, or cannot be
  named precisely. Consider extracting cohesive operations. Length alone is
  not sufficient evidence.

- **Mixed levels of abstraction** — high-level domain operations are mixed
  with low-level representation, parsing, indexing, allocation, or formatting
  details. Extract cohesive lower-level operations so that the main flow reads
  at one level of abstraction.

- **Duplicated knowledge** — the same rule, calculation, invariant, or decision
  is implemented in several places. Extract or centralize the common
  knowledge. Similar text alone is not necessarily duplication.

- **Deep or obscure control flow** — nesting, conditional chains, exits, or
  exception paths make the operation difficult to follow. Consider guard
  clauses, extracted predicates, `case` statements, discriminated types,
  named loops, or explicit `exit Loop_Name when Condition` statements. Do not
  introduce multiple exits when they make contracts or proof paths harder to
  understand.

- **Long parameter list or data clump** — several parameters repeatedly travel
  together or collectively represent one concept. Consider introducing a
  record or private type. Do not hide genuinely independent inputs in an
  artificial parameter object.

- **Flag argument** — a Boolean parameter selects between conceptually
  distinct operations or substantial execution paths. Consider separate,
  intention-revealing subprograms or an enumeration when the alternatives
  remain variants of one operation. Do not report genuine domain state,
  setters, independent preferences, or simple diagnostic options.

- **Boolean blindness** — Boolean arguments or results do not make the meaning
  of `True` and `False` clear at declaration or call sites. Consider named
  predicates, enumerations, or intention-revealing operations. Do not report
  obvious binary properties.

- **Magic value** — a literal has a non-obvious meaning, unit, origin, or
  constraint. Introduce a named constant or domain type when the name adds
  information. Do not replace self-evident structural values such as zero or
  one mechanically.

- **Comment as deodorant** — a comment compensates for unclear structure or
  restates what the code does. Refactor the code when practical, but retain
  comments that explain rationale, external constraints, proof strategy,
  algorithms, units, or non-obvious invariants.

- **Dead code** — declarations, branches, parameters, assignments, or
  commented-out code have no effect and no documented compatibility purpose.
  Remove them after verifying that they are genuinely unused. Version control
  preserves history.

- **Exception-driven ordinary control flow** — exceptions represent expected
  alternatives, routine absence, loop termination, or normal validation
  outcomes. Prefer explicit results, predicates, option-like abstractions, or
  normal control structures. Keep exceptions for conditions that the
  abstraction defines as exceptional.

### Within a type or package abstraction

- **Low cohesion or divergent change** — a type or package has unrelated
  responsibilities and changes for several independent reasons. Split it
  along stable responsibilities, not merely by file size.

- **Kitchen-sink package specification** — one visible specification exposes
  unrelated types, constants, operations, and policies. Consider child
  packages or subordinate abstractions organised by responsibility.

- **Primitive obsession** — predefined scalar or string types represent
  domain concepts with distinct validity rules, units, or operations.
  Consider a derived scalar type, constrained subtype, enumeration, bounded
  abstraction, or private type.

- **Stringly typed data** — strings encode identifiers, modes, commands,
  units, paths, or structured values with their own syntax and validity
  rules. Introduce a domain type or parser at the system boundary.

- **Unit confusion** — values with different physical or logical units share
  the same numeric type, allowing accidental mixing. Introduce distinct types
  or dimension-aware abstractions, with explicit conversions at boundaries.

- **Invalid states representable** — a record or collection of independent
  fields permits combinations with no valid domain meaning. Consider
  discriminated records, constrained subtypes, static or dynamic predicates,
  private types, or constructors that establish the invariant.

- **Scattered invariant enforcement** — many callers check or restore the same
  validity rule instead of the responsible abstraction owning it. Centralize
  enforcement in constructors, primitive operations, predicates, type
  invariants, or contracts.

- **Representation leakage** — clients depend on storage layout, component
  choices, indexing schemes, or other internal representation details.
  Consider a private type exposing domain operations rather than storage
  operations.

- **Parallel collections** — several arrays or containers use the same index
  to hold attributes of one conceptual entity. Consider a collection of
  records, unless a structure-of-arrays design is deliberately justified by
  performance or interfacing constraints.

- **Overexposed mutability** — an API exposes writable components, `in out`
  parameters, or writable access values although clients need only a narrow
  operation. Reduce the mutation surface or provide a domain operation that
  owns the update.

- **Hidden mutable state** — state is read or modified without the dependency
  being visible in the abstraction, call, or contracts. Pass it explicitly,
  encapsulate it behind a package abstraction, or declare precise SPARK
  `Global` dependencies.

- **Speculative generality** — unused generics, parameters, hooks, abstraction
  layers, or extension points exist only for hypothetical future needs.
  Remove them unless they serve a documented requirement or compatibility
  constraint.

- **Middle man or pass-through abstraction** — a package or type mainly
  forwards calls without adding policy, encapsulation, compatibility, or a
  stable boundary. Collapse the indirection when it has no demonstrated role.

- **Contract–implementation mismatch** — the specification exposes broader,
  weaker, or differently shaped semantics than the implementation actually
  supports. Align types, modes, contracts, exceptional behaviour, and
  visibility with the intended abstraction.

### Across units and system boundaries

- **Shotgun surgery** — one conceptual change requires related edits in many
  units. Reconsider where the affected data, policy, and operations are owned.

- **Feature envy** — an operation primarily inspects or manipulates another
  abstraction's data. Consider moving the operation, exposing a suitable
  primitive operation, or revising the ownership boundary.

- **Repeated kind-code dispatch** — several places branch on the same
  enumeration, discriminant, or kind field to select type-specific behaviour.
  Consider centralizing the dispatch, using a discriminated abstraction, or
  defining primitive operations on a tagged type. A local exhaustive `case`
  is not a smell by itself.

- **Inappropriate intimacy** — units depend on each other's internal details
  or repeatedly bypass their public abstractions. Move responsibilities,
  expose appropriate operations, or extract the genuinely shared concept.

- **Cyclic dependency** — units form a dependency cycle that obscures
  layering, ownership, elaboration, or reuse. Confirm the cycle with compiler
  dependency information or suitable analysis before restructuring it.

- **Copy-paste variation between sibling units** — sibling packages reproduce
  the same knowledge with small variations. Consider a parent package,
  generic unit, shared subordinate abstraction, or generated source when the
  commonality is stable.

- **Temporal coupling** — operations must be invoked in an undocumented or
  fragile order for an object or package to remain valid. Encode the lifecycle
  in the abstraction, distinguish states through types, or enforce the order
  with preconditions and postconditions.

- **Accidental nondeterminism** — observable results depend on unstable
  traversal, container iteration, allocation order, concurrency, or
  unspecified ordering although clients or tests require canonical output.
  Define and enforce an ordering at the relevant boundary.

- **Boundary leakage** — parsing, formatting, transport, filesystem, user
  interface, or tool-specific representations spread into the domain model.
  Convert to domain abstractions at the boundary and keep external concerns
  outside the core.

### Ada and SPARK-specific signals

These patterns require Ada- and project-specific judgement. They are not
violations merely because the syntax occurs.

- **Broad `use` clauses obscuring origins** — visibility makes declarations
  ambiguous or difficult to trace. Follow the project's style rules;
  `use type` and `use all type` may appropriately expose operators.

- **Unnecessary access types or aliasing** — access values are used where
  ownership, optionality, dynamic allocation, polymorphism, or linked data
  structures are not required. Prefer ordinary parameter modes when they
  express the intended lifetime and mutation. Do not report access types that
  model a genuine ownership or representation need.

- **Unexplained `when others => null`** — cases or errors may be silently
  swallowed. Handle relevant alternatives explicitly, document an intentional
  no-op, or assert impossibility when justified. Do not replace a legitimate
  no-op mechanically with an exception.

- **Surprising side effects in functions** — a function modifies externally
  visible state or performs actions that callers would not expect from a
  query. Prefer a procedure when mutation is central to the operation's
  purpose. Judge the design against the project's Ada or SPARK restrictions
  and contracts; do not assume that every Ada function must be pure.

- **Implicit parameter passing through aliases or globals** — aliased or
  global objects carry inputs and outputs that are not apparent from the
  operation's interface or contracts. Pass them explicitly, encapsulate the
  state, or declare precise data and dependency contracts.

- **Unjustified warning, analysis, or proof suppression** — pragmas, aspects,
  filters, or configuration switches silence diagnostics without a precise
  local rationale. Remove the cause where practical, or document the reason
  and keep the suppression as narrow as possible.

- **Unconstrained predefined numeric types for domain values** — broad
  predefined types permit values that the domain cannot accept. Consider
  ranges, modular types, decimal or ordinary fixed-point types, predicates,
  dimension-specific types, or private numeric abstractions.

- **Unchecked conversion or representation dependence** — unchecked
  conversions, explicit representation clauses, address manipulation, or
  implementation-defined assumptions escape a clearly isolated boundary.
  Confine them, state the assumptions, and verify size, alignment, endianness,
  validity, and target constraints where relevant.

- **Elaboration-order dependence** — correctness depends on implicit package
  elaboration effects or mutable initialization order. Prefer explicit
  initialization or a design whose dependencies are visible and verifiable.

- **Proof complexity growth** — proofs require increasingly broad invariants,
  assertions, lemmas, or prover-specific guidance. Inspect the code structure,
  data model, contracts, and missing intermediate properties. Growing proof
  effort is a signal, not proof that restructuring is necessarily the right
  remedy.

- **Implementation-driven public contracts** — public preconditions,
  postconditions, or types expose incidental algorithmic limitations rather
  than stable domain semantics. Separate domain guarantees from internal proof
  or implementation details.

## Detection methods

Use several kinds of evidence. Do not diagnose from one metric or textual
pattern alone.

### Structural reading

For each relevant subprogram, type, and package, ask:

- Can its responsibility be stated precisely without saying "and" repeatedly?
- Does each operation work at a coherent level of abstraction?
- Is ownership of state and invariants clear?
- Can invalid states or unit errors be represented?
- Are dependencies, mutation, and exceptional behaviour visible?
- Does the public specification express domain semantics rather than storage
  or algorithmic details?

### Change analysis

Use the diff and recent change context to identify:

- unrelated changes occurring together;
- one concept edited in many units;
- repeated fixes to the same invariant;
- parallel changes in sibling packages;
- public API changes caused by private representation changes.

### Tools and textual signals

When available, use:

- compiler dependency information;
- `gnatmetric` for size and complexity signals;
- project formatting, linting, static-analysis, and review targets;
- GNATprove reports and proof obligations;
- targeted searches for constructs such as:
  - `when others`;
  - `pragma Unreferenced`;
  - warning or proof suppressions;
  - `Unchecked_Conversion`;
  - access declarations;
  - broad `use` clauses;
  - repeated literals;
  - duplicated conditionals;
  - repeated discriminant or kind-code dispatch.

Tool output is evidence to inspect, not an automatic smell report.

### SPARK proof signals

When proof becomes difficult, examine separately:

- missing or weak subprogram contracts;
- missing loop invariants or loop variants;
- excessively broad frame conditions;
- hidden global dependencies;
- data structures that permit invalid states;
- missing intermediate assertions or lemmas;
- limitations of the selected prover;
- genuinely difficult mathematical properties.

Do not restructure correct and coherent code solely to reduce the number of
annotations.

## Constraints in Lionel's projects

- Shared project rules live in
  `~/prj/ada-project-template/AGENTS.template.md`.

- Do not commit without the owner's explicit consent.

- Keep one coherent refactoring as one reviewed change.

- Report verification failures instead of working around or hiding them.

- Preserve public behaviour, output, diagnostics, exit status, file formats,
  and ordering unless a separate behaviour change has been explicitly
  requested.

- Documentation is executable in these projects. If a supposed refactoring
  changes help text, output formatting, canonical ordering, scenarios, or
  externally observable diagnostics, treat it as a behaviour change rather
  than silently updating the documentation.

- Update documentation in the same change only when the documentation
  describes structure, architecture, names, or other non-behavioural facts
  altered by the refactoring.

- Never use formatting churn, unrelated renaming, or broad cleanup to hide the
  semantic scope of a refactoring.
