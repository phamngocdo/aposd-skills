---
name: complexity-diagnostic
description: Gateway skill for reviewing code for design complexity. Use when asked to review code in general terms ("review this code", "review this PR", "review this design") — scan for the three symptoms of complexity (change amplification, cognitive load, unknown unknowns), judge whether the code is tactical or strategic, and route to the appropriate specialized review skill.
---

# Complexity Diagnostic

A quick scan that determines *where* complexity lives before any deep review. This is the **entry point** for design review: run it first, then route to a specialized skill based on what you find.

## When to use
- The user asks for a general review ("review this code", "review this diff", "review my design").
- You are starting a design review and don't yet know which aspect to focus on.

## The three symptoms of complexity

Complexity = anything that makes the system **hard to understand and modify**. It shows up as:

1. **Change Amplification** — a small change forces edits in many places.
   - *Example:* changing a background color requires touching multiple pages instead of one variable.
2. **Cognitive Load** — a developer must hold a lot of information in mind to complete a task.
   - *Note:* adding code can *reduce* cognitive load if it makes the system easier to understand.
3. **Unknown Unknowns** — it is unclear which code must change, or what info is needed. **Worst form.**
   - *Example:* a hidden variable `emph` also controls background shadow, unknown to developers changing `bannerBg`.

## Root causes to name
- **Dependencies** — changing one piece forces changes in others.
- **Obscurity** — important information is not expressed or is hard to discover.

## Tactical vs Strategic
- **Tactical**: code written to make the feature work *as fast as possible*, ignoring long-term structure. Look for: quick fixes, special cases, copy-paste, deferred cleanup.
- **Strategic**: code written with an *investment mindset* — clean structure that anticipates future change. Look for: small continuous design improvements, ~10–20% of time invested in design.

## Steps
1. Read the target code / diff / design.
2. For each symptom, note concrete evidence with file:line references.
3. Classify each area: tactical or strategic?
4. Identify the dominant root cause (dependencies vs obscurity).
5. **Route** to a specialized skill (table below) and run it.

## Routing table

| Finding | Specialized skill |
|---|---|
| Shallow modules, exposed internals, classitis | `deep-module-design` |
| Pass-through methods, decorator stacks, misplaced layers | `layer-boundary-review` |
| Heavy try/catch, defensive error handling, special cases | `error-handling-simplifier` |
| New module/API being designed | `design-it-twice` |
| Comments missing or repeating code | `comment-strategy` |
| Vague or inconsistent names | `naming-review` |
| Inconsistent style, non-obvious code | `consistency-and-clarity-review` |
| Performance concerns, hot paths | `performance-by-design-review` |

## Output
A short report: symptoms found (with evidence), tactical/strategic verdict, root causes, and the recommended next skill(s).