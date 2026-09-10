---
name: error-handling-simplifier
description: Review error handling for excess complexity. Use when reviewing code heavy on try/catch blocks, exceptions, special return values, or defensive error checks — find places to "define errors out of existence", apply masking/aggregation, and decide which errors should just crash. Also triggered by complexity-diagnostic flagging many try/catch blocks.
---

# Error Handling Simplifier

Goal: **minimize the number of places where exceptions must be handled** — ideally design APIs so there are no exceptions to handle.

## When to use
- Code is littered with try/catch, error return values, or defensive checks.
- Featured in the complexity-diagnostic findings.

## Checklist

### 1. Define errors out of existence
- **Red flag — exception for something a better contract avoids.**
  - *Example:* Windows errors when deleting an open file; Unix marks it for deletion and defers destruction — two error scenarios disappear.
  - *Example:* Java `substring` throws on out-of-range indices; clamp the range instead ("return the characters in the requested range that actually exist") — callers need no validation code.
  - *Example:* special cases in the domain — instead of an "empty/no selection" state with `if` branches, represent "no selection" as an empty selection (start == end); copy/delete then work correctly with no special code.

### 2. Shift responsibility to the caller?
- Throwing "lets the caller decide" often just pushes the burden upward — **callers usually don't know what to do either**. One person (the module author) handling it beats dozens of callers.
- Exceptions should be the jarring interruption, not the default path.

### 3. Mask at the lower layer (when info isn't needed up top)
- **Exception masking**: handle exceptional conditions down low so upper layers never see them (TCP retransmits; NFS retries until the server returns).
- → deeper classes, fewer exposed exceptions.
- **Limit, don't hide blindly**: if upper layers *need* the info to make decisions (e.g., a peer went down), expose it even at the cost of interface complexity. Hiding a real failure and continuing "as if nothing happened" is wrong.

### 4. Aggregate repeated handling
- **Exception aggregation**: multiple exceptions handled by one shared handler instead of repeated per-call handlers.
  - *Example:* instead of wrapping every `getParameter("x")` in its own `NoSuchParameter` handler, let them propagate to one central dispatch/error-handler that produces a single error response.

### 5. Just crash (unrecoverable errors)
- Extremely rare, unrecoverable (OOM, disk hardware failure): report + terminate is often the simplest correct choice.
- Wrap the fallible operation in one place (e.g., a `ckalloc` wrapper around `malloc` that checks and dies) instead of checking every call site.
- **Exception**: distributed/replicated systems often must recover (from replicas) rather than crash.

## Output
Per finding: the current error path, what it costs, and the simpler design (redefine contract, mask, aggregate, or crash).