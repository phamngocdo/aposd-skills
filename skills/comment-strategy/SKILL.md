---
name: comment-strategy
description: Review and improve code comments. Use when reviewing documentation quality — comments that repeat code, missing interface/implementation docs, out-of-date comments, or vague descriptions. Also use when writing comments (produce comments first, at the right level). Triggered by complexity-diagnostic flagging missing or code-repeating comments.
---

# Comment Strategy

Comments exist because the programming language **cannot capture everything the developer had in mind**. Their job: reduce cognitive load and unknown unknowns, clarify dependencies, and make abstractions usable without reading the implementation.

## When to use
- Reviewing comment quality (missing, wrong level, repeating code, stale).
- Writing comments for new or modified code.
- Design work where a comment is the "canary in the coal mine" for a bad abstraction (Hard to Describe).

## Checklist for review

### Don't repeat the code
- **Red flag — Comment Repeats Code**: the comment says only what the code already says (e.g., `// Convert PARAMETER to TYPE` next to `downCastParameter(String parameter, String type)`).
- Useful instead: `// Amount of space left on left and right sides of each line of text, in pixels.`

### Right level for the location
- **Lower level (variables, params, returns)** — add precision: units, inclusive/exclusive bounds, meaning of `null`, invariants. Describe what the variable *is* (noun), not how it's used (verb).
  - Vague: `// Current offset in respBuffer`
  - Precise: `// Position in the buffer of the first object not yet returned to the client.`
- **Higher level (inside methods, big blocks)** — explain *intent and why*, not each line: `// Try to insert the current hash key into an existing unsent RPC to the target server.`

### Interface vs implementation
- **Interface comments**: behavior from the *caller's* perspective — parameter constraints, side effects, exceptions, preconditions. This is where the abstraction is defined.
- **Red flag — Implementation Contaminates Interface**: the interface comment describes internal algorithm/details users don't need.

### Place close to the related code
- Comments close to code stay up to date. General rule: *the farther a comment sits from the code it describes, the more abstract/general it must be.*
- For a long multi-stage function: write a short strategic overview at the top, then move the detailed comments to the start of each block.
- Each design decision is documented **once**, at the clearest location; other places get a short pointer (`// See the "Zombies" section in designNotes.`). For existing protocols, link the spec instead of rewriting it.
- Critical details belong **in the code comments, not only in the commit log** — otherwise a future change may silently reintroduce the bug.

### Write comments first
- For new code: write the class-interface comment, then public signatures and comments (bodies empty), then instance-variable comments, then implement.
- Whenever you need a new variable or method, write its comment **before** the code.
- **Red flag — Hard to Describe**: needs a very long comment to explain → the abstraction is missing / the module is shallow. Fix the design, not the comment.

## Output
Per finding: the comment problem (repeats code / wrong level / contamination / stale / misplaced), and the concrete replacement text.