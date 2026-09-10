---
name: design-it-twice
description: Facilitate design exploration for a NEW module, API, or major architectural decision. Use when starting a design (new module, new interface/API, decomposition choice) — the first idea is rarely optimal. Sketch 2-3 fundamentally different approaches, list pros/cons of each, then pick or combine. This is a facilitate skill, not a review skill.
---

# Design It Twice

When designing something new, the first idea that comes to mind is rarely the best. Sketch **at least two or three fundamentally different** approaches before committing.

## When to use
- The user asks to *design* (not review) a new module, class API, function signature, or system decomposition.
- Choosing between design alternatives and the user wants a reasoned pick.

## Steps

1. **Frame the design decision.** Name what's being decided: the module's external interface, its internal algorithm, or how a larger module is decomposed. State the constraints and the main use cases the design must serve.

2. **Sketch approach A — the obvious first idea.** Write it down honestly (this is the default everyone reaches for).

3. **Sketch approach B — force a fundamentally different one.** Even if you're convinced there's only one feasible solution, invent a second framing (different abstraction, different split, different data model). Analyzing its weaknesses *is* the value — it deepens understanding of the problem.

4. **Sketch approach C when possible** — another axis of variation (e.g., generality vs. special-purpose, deep single class vs. several layers, push complexity down vs. keep it out).

## Evaluation
For each approach, list **advantages and disadvantages** against criteria like:
- Ease of use for higher-level code (simplest common case)
- Generality / reusability
- Interface simplicity (depth)
- Implementation cost and performance

A table format (criterion × approach) keeps the comparison honest.

## Decision
- Choose the best approach, **or combine the best aspects** of several into a new design.
- If the interfaces of different approaches make the final design ambiguous, say so and pick a resolution.

## When not to use
- Reviewing existing code (use `complexity-diagnostic` + the relevant review skill instead).
- Tiny, obviously-in-place changes.

## Note on cost
A short design-it-twice pass takes little time relative to implementation but pays off many times over. For an API sketch, sanity-check the chosen interface against the common case: is the most frequent usage the simplest usage?