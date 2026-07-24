# PROJECT ENGINEERING GOVERNANCE

This document defines architectural constraints for all implementation work.
It should be read before making non-trivial changes.

The objective is not stylistic uniformity.
The objective is preserving system coherence as the project evolves.


## 1. Core Rule

Prefer changes that make the system easier to reason about after the change
than before it.

Do not solve a local problem by creating a global architectural problem.


## 2. Preserve Architectural Boundaries

Each subsystem owns a specific responsibility.

Dependencies should follow established architectural direction.

Do not introduce dependencies between unrelated subsystems merely because it
makes the current implementation easier.

Before crossing an existing boundary, determine whether the dependency belongs
there architecturally.


## 3. One Canonical Implementation

Before creating:

- a helper
- a utility
- a data model
- persistent state
- an algorithm
- a configuration value
- a serialization format

search the codebase for an existing implementation.

Extend the canonical implementation rather than creating parallel mechanisms.

Every important concept should have one authoritative representation.


## 4. State Ownership

Every mutable value must have an identifiable owner.

For meaningful state, it should be possible to answer:

- Who owns it?
- Who may change it?
- Who observes it?
- What is its lifetime?
- Is it persisted?
- What is its source of truth?

Do not create mirrored state unless synchronization is intentionally designed.


## 5. Explicit Domain Modeling

Represent meaningful concepts explicitly.

Prefer:

- enums over interacting boolean flags
- domain types over unrelated primitive arguments
- explicit lifecycle states over implicit conventions

Make invalid states difficult to construct.


## 6. Separate Policy From Mechanism

Reusable mechanisms should not contain product-specific decision logic unless
that responsibility explicitly belongs there.

Keep low-level implementation details separate from higher-level behavior and
product policy.


## 7. Abstraction Discipline

Do not abstract based on hypothetical future requirements.

Prefer the simplest concrete implementation that preserves architectural
boundaries.

Consider extracting an abstraction when:

- meaningful duplication exists,
- multiple implementations share a stable concept, or
- a current boundary requires it.

Do not introduce infrastructure merely for theoretical flexibility.


## 8. Local Changes Stay Local

A feature or bug fix should touch the smallest coherent portion of the system.

Do not perform unrelated migrations, architectural redesigns, renaming
campaigns, or cleanup while implementing a scoped task.

If the task reveals an architectural problem, identify it explicitly rather
than silently expanding scope.


## 9. Dependency Discipline

Before adding a dependency between components, ask:

1. Which component owns the concept?
2. Which direction should information flow?
3. Does this dependency expose implementation details?
4. Will this make either component harder to test or replace?

Prefer dependency on narrow interfaces or domain concepts rather than concrete
higher-level components.


## 10. Readability Over Cleverness

Prefer straightforward code whose behavior is visible locally.

Avoid unnecessary:

- indirection
- inheritance
- metaprogramming
- global state
- singleton ownership
- event buses
- generic frameworks
- implicit side effects

Complexity requires justification.


## 11. Refactoring Threshold

Refactor when architectural pressure becomes visible.

Typical signals:

- the same domain behavior is being implemented more than once;
- a component has multiple unrelated responsibilities;
- a new feature requires violating an established boundary;
- ownership of state has become ambiguous;
- changes repeatedly require touching unrelated subsystems.

Do not refactor simply to make code stylistically different.


## 12. Architectural Decisions Are Explicit

Implementation tasks must not silently introduce new architectural patterns.

Examples include:

- global managers
- service locators
- new persistence mechanisms
- cross-system event buses
- new state ownership models
- major dependency inversions
- generalized plugin architectures

When such a decision becomes necessary, surface it explicitly before allowing
it to become precedent.


## 13. Before Writing Code

For non-trivial changes:

1. Locate the current implementation.
2. Identify the owning subsystem.
3. Identify the source of truth.
4. Identify existing abstractions that should be reused.
5. Determine the smallest coherent change.
6. Check whether the proposal violates an architectural boundary.

Only then implement.


## 14. After Writing Code

Verify:

- no duplicate source of truth was introduced;
- no unrelated dependency was added;
- state ownership remains clear;
- existing behavior remains covered;
- obsolete paths introduced by the change have been removed;
- comments explain why, not what;
- documentation is updated when architecture or behavior materially changes.


## Governing Principle

A successful implementation does more than work.

It leaves the architecture at least as understandable as it was before the
change.
