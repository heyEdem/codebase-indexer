# Phase 2 — Update Mode

Follow these steps after a feature or bugfix. Read templates from `templates/` when updating doc files.

## Step 1: Read Existing Docs

Use **Read** on each file in `docs/`. Understand what is already documented.

## Step 2: Identify What Changed

```bash
git diff HEAD~1 --name-only
```

Note which modules/packages were touched.

## Step 3: Re-Scan Affected Files

Use Glob + Grep on the changed files and their direct neighbors (same package/directory). Do **not** re-scan the whole project.

## Step 4: Update Relevant Doc Files

| Changed area | Update these files |
|---|---|
| New module or package | `architecture.md`, `implementation.md` |
| New class / function / endpoint | `implementation.md` |
| Renamed files or folders | `architecture.md`, `patterns.md` |
| New dependency added | `architecture.md` |
| New naming or code pattern | `patterns.md` |
| Architectural decision | `decisions.md` (see step 5) |

Apply targeted edits — do not rewrite unaffected sections.

## Step 5: Decisions Gate

Ask: **"Did this change involve making or reversing an architectural decision?"**

| Change | Update decisions.md? |
|---|---|
| Added new API endpoint | No |
| Switched REST to GraphQL | **Yes** |
| Fixed a null pointer bug | No |
| Replaced ORM after performance issues | **Yes** — e.g., "chose JOOQ over Hibernate due to N+1 problems" |
| Added a new service module | Only if the structural choice was deliberate |

If yes — read `templates/decisions.md` for the ADR format, then append entry.
If no — skip `decisions.md`.

## Step 6: Append Changelog Entry

Always append a new dated entry to `docs/changelog.md`:

```markdown
## YYYY-MM-DD — [brief description]
- What changed
- Which modules were affected
```
