---
name: consistency-and-clarity-review
description: Review code for consistency (names, style, interfaces, patterns, invariants) and obviousness — whether a first-time reader can quickly grasp the code's behavior and meaning. Use when reviewing coding style, formatting, and comprehensibility; triggered by complexity-diagnostic flagging inconsistent style or non-obvious code.
---

# Consistency and Clarity Review

Two lenses: **consistency** (similar things treated similarly, system-wide) and **obviousness** (a first-time reader's first impression is accurate). Both reduce cognitive load and cut special cases.

## When to use
- Reviewing style, formatting, naming conventions, pattern use, or code comprehensibility.
- complexity-diagnostic flagged inconsistent style or non-obvious code.

## Checklist

### Consistency
Consistency is valuable **only when similar things are treated similarly**. Forcing different things into one pattern (or reusing a name for a different concept) makes things worse — note when a "consistency fix" would actually conflate distinct concepts.

Apply at five levels:
1. **Names** — same purpose, same name, system-wide.
2. **Coding style** — indentation, bracket placement, declaration order per the project's style guide.
3. **Interfaces** — one interface, multiple implementations (e.g., driver-like APIs) reduces what developers must learn.
4. **Patterns** — standardized solutions (MVC, and obvious equivalents) are easier to understand than ad-hoc structures.
5. **Invariants** — properties always true (e.g., "every line ends with a newline") remove special cases.

Maintenance:
- Document conventions somewhere accessible (Wiki / CONTRIBUTING / linters).
- Prefer **automatic enforcement** for machine-checkable rules (scripts/linters; e.g., a hook that rejects files containing `\r`).
- **When in Rome**: editing an existing file, follow *that file's* existing conventions.
- **Don't change conventions arbitrarily** just because a "better idea" appears — unless it's significantly better and the cost of updating the codebase is accepted.

### Obviousness
Code is obvious when **another person** can skim it quickly with an accurate first impression. Clarity lives in the reader's mind — if the reviewer says it's not obvious, it isn't.

Techniques:
- Good names + consistency (covered by `naming-review`).
- **Whitespace**: blank lines separating logical blocks; avoid cramming (compare the dense `for (int pass = 1; pass >= 0 && !empty; pass--)` against a readable equivalent). Alignment and spacing make structure scannable.
- **Compensating comments**: when code can't be made obvious, a comment supplies the missing info.

Things that make code *less* obvious — flag these:
- **Dense formatting**: no whitespace, hard-to-scan structures.
- **Indirection that hides control flow**: event handlers invoked via pointers/interfaces — add an interface comment saying when/how the handler is triggered.
- **Generic pair types**: `Pair`/`std::pair` accessed via `getKey()`/`getValue()` → define a small class/struct with meaningful field names (e.g., `totalChars`).
- **Declaring one type, allocating another**: `List` variable holding an `ArrayList` hides performance/threading characteristics.
- **Violating reader expectations**: e.g., a `main` that returns but leaves background threads running (a constructor started them) — must be called out with a comment.

## Output
Per finding: consistency or obviousness issue, why it misleads a reader, and the concrete fix (rename, reformat, restructure, comment).