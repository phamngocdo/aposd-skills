---
name: naming-review
description: Review identifier names (variables, methods, classes, parameters) for precision, consistency, and the "mental image" a reader forms. Use when reviewing naming quality — vague, generic, over-specific, or inconsistent names. Triggered by complexity-diagnostic flagging vague or inconsistent names, or by a hard-to-pick-name / hard-to-describe red flag.
---

# Naming Review

Names are a form of documentation and abstraction: a good name lets a reader infer what an entity *is* (and what it is not) **even seen in isolation**. Poor names are a major, cumulative source of complexity and bugs.

## When to use
- Reviewing identifiers for clarity, precision, and consistency.
- The complexity-diagnostic scan flagged vague or inconsistent names.

## Checklist

### Create an image
- The name should form a clear mental picture of the entity's nature in the reader's mind.
- Keep names short — around **2–3 words** — focusing on the most important aspects, leaving out secondary details.
- Names are abstractions: too long and you've hidden nothing that matters; too generic and you've hidden *everything*.

### Be precise
- **Red flag — Vague Name**: so imprecise it conveys little useful info.
  - Too generic: `getCount()` on an indexlet manager → `getActiveIndexlets()` or `numIndexlets()`.
  - Ambiguous context: `x`/`y` for a character's file position (mistakable for pixels) → `charIndex`/`lineIndex`.
- **Booleans as predicates**: `blinkStatus` → `cursorVisible` (true = visible, false = hidden).
- Acceptable exceptions: `i`, `j` in short loops spanning a few lines.
- **Red flag — Overly specific**: a name that pins semantics the code doesn't have. `delete(Range selection)` → parameter `range` (it deletes any range, not just the current selection).

### Stay consistent
Same purpose → same name, **everywhere in the system**; never use that name for another purpose; keep the purpose narrow enough that all uses behave the same.
- Need two of a kind in one place → keep the common name, add a distinguishing prefix: `srcFileBlock` / `dstFileBlock`.

### When the name is hard to pick
- **Red flag — Hard to Pick Name**: difficulty naming an entity usually means *the concept is muddled, not the vocabulary*. Go back and question the design (this pairs with `design-it-twice`).
- **Red flag — Hard to Describe** (from comment-strategy): naming and commenting trouble often share one root — the abstraction itself is wrong.

## Output
Per example: current name → suggested name with the one-line rationale (image, precision, or consistency). Flag any names that are hard to pick as a design smell worth revisiting.