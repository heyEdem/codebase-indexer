# Phase 1 — Initial Indexing

Follow these steps in order. Read templates from `templates/` when generating each doc file.

## Step 1: Detect Project Type

Check for project manifests (use Glob/Read):

| Manifest | Stack |
|----------|-------|
| `package.json` | Node.js / JavaScript / TypeScript |
| `pom.xml` | Maven / Java |
| `build.gradle` / `build.gradle.kts` | Gradle / Java / Kotlin |
| `go.mod` | Go |
| `requirements.txt` / `pyproject.toml` / `setup.py` | Python |
| `Cargo.toml` | Rust |
| `*.csproj` / `*.sln` | .NET / C# |
| `composer.json` | PHP |

Multiple manifests = polyglot/monorepo — note all detected stacks.

## Step 2: Scan the Codebase

Use **Glob** and **Grep** (not Bash find/ls):

| What to find | How |
|---|---|
| Directory structure (3 levels max) | `Glob **/*` then collapse paths |
| Entry points | `Grep` for `main`, `app`, `index`, `server`, `Application` |
| Key abstractions | `Grep` for `class`, `interface`, `export`, `func`, `def` |
| Config files | `Glob` for `*.env*`, `*.config.*`, `application.yml`, `application.properties` |
| External deps | Read manifest + lockfile (`package-lock.json`, `go.sum`, `pom.xml` deps) |
| Routing / API | `Grep` for `@GetMapping`, `router.get`, `app.get`, `path=` |

**Exclude:** `node_modules`, `.git`, `build/`, `dist/`, `target/`, `__pycache__`

Use patterns like `**/*.{ts,js,go,java,py,rs}` to limit scan depth.

## Step 3: Generate Doc Files

Write all five files under `docs/` in the project root:

1. Read `templates/architecture.md` → write `docs/architecture.md`
2. Read `templates/implementation.md` → write `docs/implementation.md`
3. Read `templates/patterns.md` → write `docs/patterns.md`
4. Read `templates/decisions.md` → write `docs/decisions.md`
5. Read `templates/changelog.md` → write `docs/changelog.md`

Do not invent information — if something cannot be determined from the scan, say so explicitly.

## Step 4: Update .gitignore

Read `guides/gitignore-rules.md` and follow it.

## Step 5: Install Auto-Update Rules in CLAUDE.md

This is critical — it makes future doc updates automatic without re-invoking the skill.

1. Read `templates/claude-md-rules.md` to get the rules block.
2. Check if a `CLAUDE.md` exists in the project root.
   - If it exists: read it. If it already contains "Codebase Index" section, skip. Otherwise **append** the rules block to the end.
   - If it does not exist: **create** `CLAUDE.md` with the rules block.
3. Never duplicate — always check before writing.

After this step, Claude will automatically read docs at session start and update them after every feature/bugfix — no manual skill invocation needed.

## Step 6: Report

Tell the user:
- Which project type was detected
- Which files were created
- That CLAUDE.md now has auto-update rules installed
- One sentence summary (e.g., "Spring Boot REST API with 4 service modules and PostgreSQL.")
