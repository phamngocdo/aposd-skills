# aposd-skills

9 Claude Code skills that apply the design principles from **A Philosophy of Software Design** by John Ousterhout — turning concepts like *deep vs. shallow modules*, *information hiding*, and *defining errors out of existence* into actionable code reviews and design sessions, run directly by Claude.

---

## What it does

Adds **9 skills** to Claude Code. Ask Claude to review a file, a PR, a class, an error-handling path, or a naming choice — and it will review against the Ousterhout framework and route to the right specialized skill.

| Skill | What it does |
|---|---|
| `complexity-diagnostic` | **Gateway skill.** Scans for the three symptoms of complexity (change amplification, cognitive load, unknown unknowns) and routes to the specialized skills below. |
| `deep-module-design` | Checks whether modules/classes are deep or shallow, and whether information is properly hidden. |
| `layer-boundary-review` | Reviews layer boundaries — pass-throughs, wrappers, decorator stacks, "together or apart" decisions. |
| `error-handling-simplifier` | Simplifies excess `try/catch` complexity — where to "define errors out of existence". |
| `design-it-twice` | Design facilitator (not a review): sketches 2–3 fundamentally different approaches for a new module or API before you commit. |
| `comment-strategy` | Reviews comment quality — comments that repeat code, missing interface docs, vague or outdated comments. |
| `naming-review` | Reviews identifiers for precision, consistency, and the "mental image" a reader forms. |
| `consistency-and-clarity-review` | Reviews consistency (names, style, interfaces, invariants) and whether code is obvious to a first-time reader. |
| `performance-by-design-review` | Reviews performance without falling into premature optimization — measure first, design around the critical path. |

---

## Installation
```bash
npx skills add phamngocdo/aposd-skills
```

This installs the marketplace and all 9 skills. Restart Claude Code if needed, then run `/skills` to see them.

---

## Usage

You don't need to memorize the skill names — just ask naturally:

```
Claude, review this code
Claude, review this class design
Claude, is this module too shallow?
Claude, this error handling has too many try/catch — simplify it
Claude, I'm designing a new API. Explore two different designs.
```

`complexity-diagnostic` picks up general review requests and routes to the right specialist. You can also invoke any skill directly with its slash command, e.g. `/deep-module-design`.

---

## How it works

- **Gateway → specialist routing.** `complexity-diagnostic` is the entry point for general review requests. Its routing table maps each finding to a specialized skill (e.g. many `try/catch` blocks → `error-handling-simplifier`) and runs it.
- **Skills mirror the book's chapters.** Each skill encodes one theme from *A Philosophy of Software Design*; the principles themselves are documented in [`book-note/`](book-note/) (one file per chapter) as reference material for the skills.

---

## Project layout

```
skills/                 # the 9 skills, one directory each (SKILL.md)
├── complexity-diagnostic/
├── deep-module-design/
├── layer-boundary-review/
├── error-handling-simplifier/
├── design-it-twice/
├── comment-strategy/
├── naming-review/
├── consistency-and-clarity-review/
└── performance-by-design-review/
.claude-plugin/         # plugin manifest + marketplace declaration
book-note/              # design principles, one file per chapter
```

---

## License

MIT — free to use, fork, and extend. The skills are a software-design aid, not a substitute for the book.

---

*Inspired by John Ousterhout, A Philosophy of Software Design, 2nd edition.*