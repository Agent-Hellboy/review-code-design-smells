# Review Rubric

## Contents

- Minimum evidence
- Severity rubric
- Common false positives
- Prioritization rules
- Recommendation rules

Use this reference when you are unsure whether a suspected smell is worth reporting or how strongly to state it.

## Minimum evidence

Report a smell only when at least one of these is true:

- A change to one concept requires edits in several places.
- The current boundary hides or splits an important invariant.
- Callers must know internal sequencing, transport, storage, or framework details.
- Ownership is unclear enough that behavior is duplicated or misplaced.
- The implementation shape materially increases testing or debugging cost.

If you only have "this code feels big" or "this abstraction looks odd," do not elevate it beyond a low-confidence observation.

## Severity rubric

### High

Use `high` when the smell already creates production risk, blocks safe changes, or causes wide fan-out:

- hidden temporal coupling with failure risk
- business rules scattered across several layers
- cyclic dependencies preventing isolation
- a god module controlling unrelated capabilities

### Medium

Use `medium` when the smell materially slows development or testing but the blast radius is still contained:

- repeated orchestration or validation that must stay aligned
- unstable abstraction around one real use case
- data clumps or primitive obsession weakening invariants

### Low

Use `low` when the issue is local cleanup with limited downstream impact:

- a slightly oversized helper with one dominant responsibility
- shallow duplication with little change pressure
- naming or structure issues that do not obscure behavior

## Common false positives

Do not over-report these patterns:

- Composition roots and wiring modules: they may look broad because their job is assembly.
- Framework adapters and transport layers: some translation code is expected at boundaries.
- Generated code or schema-heavy types: size alone is not a smell.
- Tests, fixtures, and builders: duplication is often acceptable when it preserves test clarity.
- Compatibility shims and migration paths: temporary indirection may be intentional.
- Application services: orchestration is not automatically feature envy.

## Prioritization rules

- Prefer one `high` finding with a clear fix over five minor smells.
- Merge related findings when they stem from the same ownership problem.
- If several local smells all point to one boundary issue, report the boundary issue first.
- If the fix would be a rewrite, step back and propose an intermediate refactor instead.

## Recommendation rules

- Recommend extraction when a concept or invariant appears in several places.
- Recommend moving behavior when data and decisions live apart.
- Recommend splitting a module when unrelated axes of change are mixed together.
- Recommend a value object when primitives are carrying domain meaning implicitly.
- Recommend dependency inversion only when it removes a real cycle or isolates a stable seam.
