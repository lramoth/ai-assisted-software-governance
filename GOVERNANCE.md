# PROJECT ENGINEERING GOVERNANCE

The objective is not stylistic uniformity.
It is preserving system coherence as the project evolves.


## 1. Before Writing Code

Run the pre-flight when a change affects ownership, dependency direction,
lifecycle, persistence, concurrency, data shape, or any other architectural
concern.

Common cases, not a complete list: creating a file, adding a dependency,
introducing state that outlives a function call, changing a module's public
surface, adding a serialization or persistence format.

Skip the pre-flight only for behavior-preserving textual changes, such as
correcting prose, comments, or display text. File count, function count, and
diff size do not make an architectural change trivial. The qualifier governs
the examples: if you cannot say why the change preserves behavior, it is not
exempt.

When unsure, run it. It is four lines. A false positive costs nothing; a
false negative is how drift starts.

    Owner:     <subsystem owning this concept>
    Truth:     <where the authoritative value lives>
    Extending: <existing implementation> | none found (searched: <terms>)
    Scope:     <smallest coherent change>

"none found" requires that the search actually ran, across the names the
concept could plausibly carry — not only the name you would give it. An
omitted line is a skipped step, not a trivial one.


## 2. After Writing Code

Verify:

- no duplicate source of truth was introduced;
- no unrelated dependency was added;
- state ownership remains clear;
- tests assert what they asserted before — a suite made green by weakening
  an assertion is a failed change;
- obsolete paths introduced by this change have been removed; pre-existing
  ones are out of scope (§3);
- comments explain why, not what;
- documentation is updated when architecture or behavior materially changes.


---

Rules 3, 4, and 8 run against normal instinct under time pressure — it is
always faster to write a new helper, to add an option for later, and to tidy
the file you are already in. Drift comes from these three more than the other
nine.

Examples throughout illustrate the shape of a rule. They are not templates to
copy.

---


## 3. Local Changes Stay Local

A feature or bug fix touches the smallest coherent portion of the system.

Do not perform unrelated migrations, architectural redesigns, renaming
campaigns, or cleanup while implementing a scoped task.

If the task reveals an architectural problem, identify it explicitly rather
than silently expanding scope.


## 4. One Canonical Implementation

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

Every important concept has one authoritative representation.


## 5. Boundaries and Dependency Direction

Each subsystem owns a specific responsibility. Dependencies follow the
established architectural direction.

Do not introduce a dependency between unrelated subsystems merely because it
makes the current implementation easier.

Before adding a dependency or crossing an existing boundary, ask:

1. Which component owns the concept?
2. Which direction should information flow?
3. Does this dependency expose implementation details?
4. Will this make either component harder to test or replace?

Prefer dependency on narrow interfaces or domain concepts rather than on
concrete higher-level components.


## 6. State Ownership

Every mutable value must have an identifiable owner.

For meaningful state, it should be possible to answer:

- Who owns it?
- Who may change it?
- Who observes it?
- What is its lifetime?
- Is it persisted?
- What is its source of truth?

Do not create mirrored state unless synchronization is intentionally designed.


## 7. Explicit Domain Modeling

Represent meaningful concepts explicitly.

Prefer:

- enums over interacting boolean flags
- domain types over unrelated primitive arguments
- explicit lifecycle states over implicit conventions

Make invalid states difficult to construct.

    Not: isLoading + isError + isDone        →  a RequestState enum
    Not: send(id: string, name: string)      →  send(recipient: Recipient)


## 8. Abstraction Discipline

Do not abstract based on hypothetical future requirements.

Use the simplest concrete implementation that preserves architectural
boundaries.

Consider extracting an abstraction when:

- meaningful duplication exists,
- multiple implementations share a stable concept, or
- a current boundary requires it.

Do not introduce infrastructure merely for theoretical flexibility.

    Not: a Strategy interface with one implementation  →  the concrete function


## 9. Refactoring Threshold

Refactor when architectural pressure becomes visible.

Typical signals:

- the same domain behavior is being implemented more than once;
- a component has multiple unrelated responsibilities;
- a new feature requires violating an established boundary;
- ownership of state has become ambiguous;
- changes repeatedly require touching unrelated subsystems.

Do not refactor simply to make code stylistically different.


## 10. Separate Policy From Mechanism

Reusable mechanisms should not contain product-specific decision logic unless
that responsibility explicitly belongs there.

Keep low-level implementation details separate from higher-level behavior and
product policy.


## 11. Readability Over Cleverness

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

    Not: an event bus introduced for a single consumer  →  a direct call where
    dependency direction allows it, otherwise a narrow interface owned by the
    caller


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

When one becomes necessary: stop and present it before writing code — what
you would decide, the alternative, and why.

After the user decides, record it in the project's existing repository
documentation. If the established mechanism is external — an issue tracker,
wiki, or any system outside the repository — or if no mechanism exists, ask
before writing to or creating it.

Never record a decision that has not been approved.

    Not: a new UserManager singleton  →  pass the dependency in


## Governing Principle

A successful implementation does more than work.

Do not solve a local problem by creating a global architectural problem.

It leaves the architecture at least as understandable as it was before the
change.
