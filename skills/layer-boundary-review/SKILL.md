---
name: layer-boundary-review
description: Review layer boundaries and the distribution of responsibility between and within modules. Use when a design/review touches method chains, wrappers ("Decorator" pattern), pass-through methods or variables, module split/merge questions ("together or apart"), or when the complexity-diagnostic scan flagged pass-throughs, decorator stacks, or misplaced layers.
---

# Layer Boundary Review

Checks whether complexity has been pushed to the right layer: down into modules that benefit many users, not up onto callers.

## When to use
- Long call chains, methods that just forward to another method.
- Wrapper/Decorator classes, context/config plumbing.
- Deciding whether two pieces of code belong together or apart.
- complexity-diagnostic flagged pass-through methods, decorator stacks, or mislayered code.

## Checklist

### Pass-through methods
A method that adds no functionality and only calls another method with a similar signature.
- It increases interface complexity without adding functionality, and creates unnecessary dependencies.
- Fix options (in order):
  1. Let callers access the lower-level class directly.
  2. Redistribute functionality between classes to avoid multi-layer forwarding.
  3. Merge the classes.
- **Legitimate duplication**: dispatchers (routing work — e.g., URL dispatcher) and multiple implementations of one interface (e.g., disk drivers) are fine.

### Decorator overuse
Decorators are usually **shallow**: much boilerplate, little new functionality.
- If a Decorator adds only a small feature, integrate it directly into the original class, or into the code of the one specialized use case, or into an existing Decorator (making it deeper). Or make it a fully independent class that doesn't wrap.

### Pass-through variables
A variable threaded through many intermediate methods that don't use it.
- Every intermediate method is forced to know about it; changing one such variable ripples through many signatures.
- Fix options: store it in an object already shared between first and last method; a context object (the author's usual choice); only as a last resort, a global (adds its own problems).

### Pull complexity downward
- If complexity can't be avoided, the module developer should absorb it inside the module — **simple interface > simple implementation**. Users outnumber developers.
- *Wrong:* line-oriented text API that pushes splitting/joining lines onto the UI layer. *Right:* character-oriented `insert/delete(position, text)` that hides line handling inside the class.
- *Wrong:* too many config parameters ("let the user decide"). *Right:* automatic reasonable defaults (measure and self-tune).
- **Don't overdo it**: pull down only when the complexity is closely related to the module's responsibility, significantly simplifies caller code, and simplifies the interface. Pulling in unrelated knowledge causes information leakage.

### Together or apart?
Keep pieces together when they share information, are used together both ways, overlap conceptually, or are hard to understand separately.
Split (into clean abstractions) only when: an independent subtask truly separates (parent and child don't need each other's context), or a function does several unrelated things — but never split just because "it's longer than N lines."
- **Red flag — Repetition**: same code repeated → wrong abstraction.
- **Red flag — Conjoined Methods**: can't understand one without the other → keep together.
- **Red flag — Special-General Mixture**: general-purpose mechanism polluted with one app's special-purpose code → push special code to higher layers.

## Output
Per finding: layer issue, its cost, and a concrete refactor (merge/split/move code, context object, etc.).