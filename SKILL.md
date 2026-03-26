---
name: "review-code-design-smells"
description: "Use this skill when reviewing code, pull requests, modules, packages, or service boundaries for maintainability and architecture problems such as duplication, poor abstractions, excessive complexity, tight coupling, low cohesion, feature envy, god objects, leaky layers, cyclic dependencies, architecture drift, or cleanup candidates before new implementation. Do not use it for purely stylistic linting, formatting, or one-off bug fixes unless the user explicitly wants smell analysis."
metadata:
  version: "1.1.0"
---

# Review Code and Design Smells

Review code for smells that materially affect changeability, correctness, or clarity. Focus on evidence-backed findings and practical refactors instead of generic style complaints.

## Default stance

- Optimize for high-leverage findings, not exhaustive nitpicking.
- Treat a smell as real only when you can point to ownership failure, change amplification, brittle sequencing, or unnecessary coupling.
- Default to the smallest refactor that improves ownership or boundary clarity.

## Reviewer gotchas

- Do not call something a design smell from one file alone. Check the surrounding boundary, call sites, and neighbors first.
- Size alone is not enough to label a god object or god module. Flag it only when several unrelated reasons to change are mixed together.
- Duplication is only worth reporting when the duplicated logic must stay behaviorally aligned. Similar shape is not enough.
- Do not recommend introducing an interface, base class, or extra layer unless a real variation point or stable test seam already exists.
- Feature envy is not the same as normal orchestration. An application service may coordinate several collaborators without owning all underlying behavior.
- Prefer a small refactor with a clear ownership win over a broad rewrite or architecture reset.

## Review workflow

### 1. Build context first

- Read the code under review and the nearest call sites, tests, and neighboring modules before naming a smell.
- Identify the unit's intended responsibility, data ownership, and dependency direction.
- Separate immediate bug risk from longer-term maintainability cost.
- Ask which future change is likely to hurt here: adding behavior, changing rules, swapping dependencies, or debugging failures.

### 2. Inspect for code smells

- Start with high-signal local problems: long methods, oversized classes, deeply nested control flow, repeated logic, boolean flag APIs, primitive obsession, data clumps, feature envy, hidden temporal coupling, and inconsistent error handling.
- Treat naming or formatting issues as low value unless they hide behavior or create repeated confusion.
- Prefer concrete evidence such as duplicated branches, repeated parameters, unstable invariants, or one function doing several unrelated jobs.

### 3. Inspect for design smells

- Check boundaries for god modules, leaky abstractions, cyclic dependencies, unstable interfaces, cross-layer knowledge, scattered business rules, and change fan-out across many files.
- Look for concepts that exist in several places without a clear owner.
- Flag abstractions that add indirection without reducing coupling or clarifying invariants.

### 4. Prioritize by impact

- Mark findings `high` when they already cause bugs, block safe changes, or force broad edits across the codebase.
- Mark findings `medium` when they materially slow changes, testing, or reasoning but have a contained blast radius.
- Mark findings `low` when they are local cleanup opportunities with limited impact.
- State uncertainty explicitly when the smell depends on runtime behavior or missing context.
- By default, report at most the five highest-value findings. Suppress low-signal issues unless the user asked for a full inventory.

### 5. Recommend targeted fixes

- Suggest the smallest effective refactor first: extract a concept, move behavior to the data owner, introduce a value object, split a module by responsibility, invert a dependency, or collapse duplicated paths.
- Explain why the refactor improves ownership, coupling, or testability.
- Avoid rewrite recommendations unless the current boundary is already unworkable.

## Confidence rules

- If the smell depends on hidden runtime behavior, mark it as a hypothesis and say what evidence would confirm it.
- If the code is acting as framework glue, a composition root, generated code, or a compatibility adapter, treat apparent complexity with caution.
- When in doubt between "normal orchestration" and "misplaced behavior," prefer the narrower claim and explain the ambiguity.

## Reporting rules

- Present findings first, ordered by severity and confidence.
- For each finding, include the smell type, concrete evidence, why it matters, and the most direct remediation path.
- Distinguish local code smells from broader design smells.
- Use precise file references whenever they are available.
- If no material smell is present, say so directly and mention any residual blind spots such as unreviewed integration paths or missing tests.
- Avoid pattern policing. Flag only issues that make the code harder to change, understand, or trust.

## Output shape

Use this structure unless the user asks for a different format:

```markdown
Findings

1. [Severity] [Smell name]
Evidence: concrete code path, dependency, or repeated change pattern
Why it matters: maintenance, correctness, or testing cost
Suggested fix: smallest refactor with the clearest ownership win

2. [Severity] [Smell name]
Evidence:
Why it matters:
Suggested fix:

Residual risks
```

## References

- Read [references/review-rubric.md](references/review-rubric.md) when you need severity calibration, false-positive checks, or prioritization guidance.
- Read [references/smell-catalog.md](references/smell-catalog.md) when you need classification help or a refactor mapping for a suspected smell.
