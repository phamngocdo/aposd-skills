---
name: deep-module-design
description: Evaluate whether modules/classes are deep or shallow, check information hiding and leakage, and judge whether interfaces are appropriately general-purpose. Use when reviewing class/module/API design, or when the complexity-diagnostic scan flagged shallow modules, exposed internals, or classitis. Also triggered by "review interface/design này", "module này có tốt không".
---

# Deep Module Design

Assess whether each module is **deep** (simple interface, powerful implementation) or **shallow** (interface nearly as complex as the implementation).

## When to use
- Reviewing a class, module, or API design.
- The complexity-diagnostic scan found shallow modules, exposed internals, or too many small classes.

## Checklist

### 1. Is the module deep or shallow?
- **Deep**: simple interface hides a large, complex implementation → good.
- **Shallow**: the interface is not much simpler than the implementation → red flag.
- **Classitis**: belief that "more classes = better"; excessive decomposition into tiny shallow modules → red flag. A long function with several independent blocks may be better than many tiny functions.

### 2. Information hiding
- Each module should encapsulate knowledge/design decisions (algorithms, data structures, implementation details) **inside** its implementation.
- **Red flag — Information Leakage**: a design decision is reflected in multiple modules (e.g., two classes both know the same file format).
  - Fix: combine the classes, or extract the shared knowledge into a new class with a simple interface.
- **Red flag — Temporal Decomposition**: structure is based on the *order of operations* rather than on information hiding (e.g., a "read raw bytes" class and a separate "parse" class, where the reader must know `Content-Length`).
  - Fix: group code that shares the same information flow into one module.
- **Red flag — Overexposure**: the API forces callers to deal with rarely-used features to use common features.
  - *Example:* `getParams()` returns the whole internal `Map`; callers must search and cast. Fix: `getParameter(name)`, `getIntParameter(name)`.
- Design private methods and minimize variable scope to hide info *within* a class too.
- Only hide information that external users genuinely don't need.

### 3. Interface quality
- **Abstraction** = a simplified view that ignores unimportant details. The more unimportant details hidden, the better.
- Two mistakes: including unimportant details (↑ cognitive load), omitting important ones (leaky/incorrect abstraction → obscurity).
- Design the interface around the **common case**: automatic sensible defaults (e.g., HTTP version taken from the request, timestamp generated automatically), with overrides only for rare customization.
- Interface comments must describe behavior from the caller's perspective — never implementation details.

### 4. General-purpose interface
- Make the *implementation* satisfy current needs, but the *interface* general enough for multiple uses.
- **Red flag — special-purpose API**: methods mirror UI/one-use concepts (e.g., `backspace(cursor)`, `deleteSelection(selection)` instead of `insert(pos, text)`, `delete(start, end)`).
  - Consequences: shallow methods, information leakage, less reuse.
- Ask: Is the API simple enough? How many times can this method be used (if once → too specialized)? Is it still easy to use for current needs (too general → callers write lots of glue code)?
- **False abstraction**: hiding a detail the caller actually needs (e.g., `backspace` hides *which* characters are deleted). When a detail is important, make it explicit.

## Output
Per module: depth verdict (deep/shallow), information-hiding findings, interface issues, and concrete refactoring suggestions with examples.