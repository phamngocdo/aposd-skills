# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code **plugin marketplace** (a local plugin bundle) that packages "software design" skills, based on the book *A Philosophy of Software Design* by John Ousterhout. The design principles themselves are captured in Vietnamese in `book-note.md` (change amplification, cognitive load, unknown unknowns, key concepts like deep vs. shallow modules).

The project is currently a **skeleton**: the marketplace structure exists, but `plugins/aposd-review/skills/` is empty — no skill files have been written yet.

## Structure

- `.claude-plugin/marketplace.json` — declares the marketplace (`name: my-plugins`, owner) and lists the plugins it contains, each with a `name`, `source` (relative path), and `description`.
- `plugins/aposd-review/.claude-plugin/plugin.json` — per-plugin manifest (`name: aposd-design-review`, version, author).
- `plugins/aposd-review/skills/` — where skill files (SKILL.md) go. Currently empty.
- `book-note.md` — the source material for the design principles the skills will encode.
- `README.md`, `LICENSE` (MIT).

## How to add a skill

The `skills/` directory uses Claude Code's standard plugin-skill convention. A new skill is a SKILL.md (with frontmatter `name`, `description`, and instructions), typically in its own subdirectory. After adding one, verify the naming in `plugin.json`/`marketplace.json` still matches the plugin's intent.

## Commands

There is no build, test, or lint setup — this is a static plugin bundle. The only "verification" is that the JSON manifests parse:

```bash
python3 -m json.tool .claude-plugin/marketplace.json
python3 -m json.tool plugins/aposd-review/.claude-plugin/plugin.json
```

## Conventions
- Match the Ousterhout framework (deep modules, interfaces, complexity) already established in `book-note.md`.
