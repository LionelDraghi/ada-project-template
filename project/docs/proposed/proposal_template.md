# <Change title>

Status: Proposed
Source: <issue, discussion, concern, or request>

## Need

<What problem or opportunity is being addressed?>

## Intended outcome

<What should become true for the user or system?>

## Scope

- ...

## Non-goals

- ...

## Acceptance criteria

- ...

## Specification

<Prose describing the expected behavior.>

## Executable scenarios

<!-- Load the bbt skill before editing this section. Remove the placeholder when no scenario is applicable and explain why below. -->

One scenario per behavior, steps as list items starting with `Given`,
`When`, `Then`; commands and short parameters between backticks.
Group related scenarios under a `Feature` heading, and factor common
preconditions into a `Background`. Validate with `bbt explain` on the
file.

### Scenario: <observable behavior>

- Given ...
- When I run `<command>`
- Then ...

## Verification beyond bbt

- Unit tests: <applicable / not applicable and why>
- SPARK / proof: <applicable / not applicable and why>
- Other: ...

## Design implications

<Link to design decisions; do not duplicate them.>

## Open questions

- ...

## Finalization target

<Existing feature document to update, or proposed durable destination.>
