---
name: performance-by-design-review
description: Review code from a performance perspective without falling into premature optimization. Use when performance matters (hot paths, latency, throughput) or when reviewing proposed optimizations — measure before optimizing, prefer naturally performant code, and design around the critical path. Triggered by complexity-diagnostic flagging layer-crossing hot paths or unnotivized micro-optimizations.
---

# Performance by Design Review

Performance is a **design** concern, not a line-by-line concern. Blindly optimizing everything slows development and adds complexity; completely ignoring performance invites "death by a thousand cuts" (5–10× slower from accumulated small inefficiencies).

## When to use
- Performance is an explicit requirement (hot path, latency, throughput).
- Someone proposes a micro-optimization — check whether there's justification behind it.
- complexity-diagnostic flagged layer-crossing code or speculative optimizations.

## Checklist

### 1. Prefer naturally performant code
- Choose solutions that are fast *by design* while staying simple: hash table over ordered map when order is unneeded (5–10× on common ops), avoiding layer crossings and special cases.
- **Simple code and deep classes are usually fast** — fewer special cases, fewer layer crossings.
- Optimize structure on top of a basic understanding of expensive ops (network I/O, disk I/O, dynamic allocation, cache misses), not line-by-line micro-tuning.

### 2. Measure before modifying
- Programmer intuition about performance is **unreliable**. Never optimize based solely on assumptions — that's ungrounded complexity.
- Identify the actual bottleneck (profile), establish a **baseline**, change, re-measure.
- If the change isn't a significant improvement, **roll it back**. Don't keep complexity without measured benefit.

### 3. Design around the critical path
- Once the true hot spot is found, redesign that code around the **critical path**: the minimum code that must run in the most common case.
- Method: imagine the cleanest, most concise "ideal" implementation temporarily ignoring the existing class structure and special cases — then build a design that approaches it.
- Move **special cases out of the critical path**: one early `if` that branches to separate handling, keeping the main path uncluttered.
- Watch for **shallow layers** — sequential one-call functions that each add a conditional check. Consolidate the critical path into a single method with one special-case `if` (RAMCloud `Buffer` case: 6 checks → 1 check, ~20% fewer lines, 2× faster).

## Output
Per finding: the performance concern, whether it's backed by measurement or assumption, a design-level fix (structure, not micro-tuning), and — when no measurement exists — a note to profile first.