# codebase-indexer

A Claude Code skill that scans a project once and generates a living `/docs/` folder — so Claude reads the index instead of re-scanning the whole codebase every session.

## What it does

**First run:** Scans your project, writes five doc files, and installs auto-update rules in your project's `CLAUDE.md`:

| File | Purpose |
|---|---|
| `docs/architecture.md` | Structure, module map, data flow, external dependencies |
| `docs/implementation.md` | Per-module breakdown — entry points, key classes/functions |
| `docs/patterns.md` | Naming conventions, folder conventions, recurring idioms |
| `docs/decisions.md` | ADRs — *why* things are the way they are |
| `docs/changelog.md` | Dated log of what changed and which modules were affected |

**After the first run, you never invoke the skill again.** The rules in `CLAUDE.md` make Claude automatically read the docs at session start and update them after every feature or bugfix.

`docs/` is added to `.gitignore` automatically — these are session artifacts, not committed documentation.

## Install

```bash
git clone https://github.com/heyEdem/codebase-indexer.git ~/.claude/skills/codebase-indexer
```

Claude Code will discover the skill automatically on next launch.

## Usage

Open any project in Claude Code and say:

- **"index this codebase"** — runs full initial scan

That's it. After the first scan, Claude handles everything automatically via the rules it plants in your project's `CLAUDE.md`.

You can also say **"update docs"** / **"re-index"** if you want to manually trigger an update.

## Supported project types

Detects and handles: Node.js, Java (Maven/Gradle), Go, Python, Rust, .NET, PHP — and polyglot/monorepo setups.

## How it works

```
First run (invoke once)              Every session after (automatic)
───────────────────────              ───────────────────────────────
Scan codebase (Glob + Grep)    →     Claude reads docs/ at session start
Generate 5 doc files           →     No re-scan needed
Install rules in CLAUDE.md     →     Auto-updates docs after changes
Add docs/ to .gitignore        →     Appends changelog entries
```

## Skill structure

```
~/.claude/skills/codebase-indexer/
  SKILL.md                  ← entry point (66 lines)
  guides/
    initial-scan.md         ← Phase 1: full scan steps
    update-mode.md          ← Phase 2: diff-based updates
    gitignore-rules.md      ← .gitignore handling
  templates/
    architecture.md         ← template for each doc file
    implementation.md
    patterns.md
    decisions.md
    changelog.md
    claude-md-rules.md      ← rules planted into CLAUDE.md
```

Uses progressive disclosure — Claude only loads the files it needs for the current phase.
