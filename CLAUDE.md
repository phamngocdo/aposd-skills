# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code **plugin marketplace** (a local plugin bundle) that packages "software design" skills, based on the book *A Philosophy of Software Design* by John Ousterhout. The design principles themselves are captured in English in `book-note/` (change amplification, cognitive load, unknown unknowns, key concepts like deep vs. shallow modules).

The project packages **9 skills** — one for each major theme of the book. `complexity-diagnostic` is the **gateway skill**: it scans for the three symptoms of complexity and routes to the specialized skills.

## Structure

- `.claude-plugin/plugin.json` — plugin manifest (`name: aposd-skills`, version, author) declaring `"skills": "./skills"`.
- `.claude-plugin/marketplace.json` — marketplace declaration.
- `skills/` — the 9 skills, one subdirectory each: `complexity-diagnostic`, `deep-module-design`, `layer-boundary-review`, `error-handling-simplifier`, `design-it-twice`, `comment-strategy`, `naming-review`, `consistency-and-clarity-review`, `performance-by-design-review`.
- `book-note/` — the source material for the design principles the skills encode (one file per chapter).

## Routing between skills

`complexity-diagnostic` is the entry point for a general review request. Its routing table points to the specialized skill per finding, e.g. many `try/catch` → `error-handling-simplifier`. Suggest the specialized skill step, then run it.

## How to add a skill

A new skill is a SKILL.md (with frontmatter `name`, `description`, and instructions), in its own subdirectory under `skills/`. After adding one, verify the naming in `plugin.json`/`marketplace.json` still matches the plugin's intent.

## Commands

There is no build, test, or lint setup — this is a static plugin bundle. The only "verification" is that the JSON manifests parse:

```bash
python3 -m json.tool .claude-plugin/marketplace.json
python3 -m json.tool .claude-plugin/plugin.json
```

## Conventions
- Match the Ousterhout framework (deep modules, interfaces, complexity) already established in `book-note/`.
- Skill content is written in English; frontmatter `description` should mention both English phrasing and any trigger wording that helps discovery.