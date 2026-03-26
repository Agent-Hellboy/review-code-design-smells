# Smell Catalog

## Contents

- Code smells
- Design smells
- Triage questions
- Refactor mapping

Use this catalog to sharpen classification and remediation. Do not treat it as a checklist to apply mechanically.

## Code smells

### Large unit

- Symptoms: long functions, classes with several unrelated responsibilities, wide constructor parameter lists.
- Risk: hides invariants, increases change cost, and makes tests broad and fragile.
- Typical fixes: split by responsibility, extract cohesive helpers, narrow interfaces.

### Deep branching

- Symptoms: nested conditionals, branching on several booleans, long `switch` or `if` ladders.
- Risk: increases path count and makes behavior hard to reason about.
- Typical fixes: extract policies, replace mode flags with separate behaviors, model state explicitly.

### Duplication

- Symptoms: repeated branches, mirrored validation, copy-pasted orchestration, similar data mapping in several places.
- Risk: creates inconsistent fixes and silent behavior drift.
- Typical fixes: extract shared behavior only after confirming the duplication is semantically the same.

### Feature envy or misplaced behavior

- Symptoms: a function pulls data from another object and makes all decisions itself.
- Risk: breaks ownership and spreads business rules away from the data they govern.
- Typical fixes: move behavior to the data owner or introduce a dedicated domain object.

### Primitive obsession and data clumps

- Symptoms: repeated bundles of strings, ids, numbers, or flags passed together.
- Risk: weakens invariants and makes invalid states easy to represent.
- Typical fixes: introduce value objects, typed parameters, or small records with explicit meaning.

### Temporal coupling

- Symptoms: callers must invoke methods in a hidden order, or state becomes valid only after several steps.
- Risk: creates fragile call sites and bug-prone lifecycle assumptions.
- Typical fixes: enforce construction-time invariants, reduce mutable stages, or provide a higher-level API.

### Divergent change or shotgun surgery

- Symptoms: one concept changes in many files, or one file changes for unrelated reasons.
- Risk: signals poor responsibility boundaries and expensive maintenance.
- Typical fixes: regroup behavior around the axis of change and reduce fan-out.

## Design smells

### God module

- Symptoms: one service, component, or package coordinates many concerns and knows too much about several subsystems.
- Risk: turns into a bottleneck for changes, reviews, and tests.
- Typical fixes: split by capability or business boundary, then narrow public APIs.

### Cyclic dependencies

- Symptoms: packages depend on each other directly or through a short loop.
- Risk: prevents clean layering and makes reuse and testing harder.
- Typical fixes: invert dependencies, extract interfaces, or move shared concepts to a lower layer.

### Leaky abstraction

- Symptoms: callers must understand storage, transport, framework, or protocol details that should stay internal.
- Risk: raises coupling and spreads low-level knowledge everywhere.
- Typical fixes: raise the abstraction level of the API and isolate translation logic.

### Hub dependency

- Symptoms: one module becomes the common dependency of unrelated areas.
- Risk: small changes create wide regression risk and incidental coupling.
- Typical fixes: split read models from write logic, separate utilities from domain behavior, or reduce surface area.

### Scattered concern

- Symptoms: one business rule is reimplemented across handlers, views, jobs, or services.
- Risk: makes behavior inconsistent and hard to audit.
- Typical fixes: move the rule to a single owner and call it from the edges.

### Unstable abstraction

- Symptoms: generic interfaces with one real implementation, or layers added before the variation exists.
- Risk: adds indirection without reducing coupling.
- Typical fixes: inline the abstraction until a real variation appears or tighten the interface around the real use case.

### Cross-layer knowledge

- Symptoms: UI, transport, or persistence code contains domain decisions that belong elsewhere.
- Risk: changes leak through layers and tests become integration-heavy.
- Typical fixes: move domain rules inward and keep edge layers thin.

## Triage questions

- Which change currently fans out across the most files?
- Which invariant has no obvious owner?
- Which dependency direction makes testing or reuse harder than it should be?
- Which abstraction forces callers to know internal details?
- Which area mixes orchestration, policy, and data access in one place?

## Refactor mapping

- Duplication: extract shared behavior or shared data representation.
- Feature envy: move behavior to the data owner.
- Primitive obsession: introduce a value object or typed API.
- God module: split by responsibility and narrow the interface.
- Cycles: invert dependencies or extract a lower-level shared concept.
- Leaky abstraction: isolate translation details behind a clearer boundary.
