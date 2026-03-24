---
name: claude-md-creator
description: "Creates minimal, high-signal CLAUDE.md and AGENTS.md context files for a project. Reads the codebase to discover build commands, project structure, conventions, and CI requirements. Use when setting up a new project, onboarding a codebase, or when an existing CLAUDE.md is outdated or bloated."
---

# CLAUDE.md Creator

Generates minimal, accurate CLAUDE.md and AGENTS.md files by inspecting the actual codebase.
No aspirational rules — only empirically verifiable facts.

---

## Scope

Use this skill when:
- Setting up a new project for Claude Code
- A project's CLAUDE.md is missing, outdated, or bloated (100+ lines)
- After major project restructuring
- When onboarding a new codebase

Do NOT use when:
- The project already has a well-maintained CLAUDE.md under 50 lines
- The user just wants to add a single entry to an existing CLAUDE.md (edit directly instead)

---

## Process

### 1. Discover Build/Test Commands

Check these sources in order:

| Source | What to Extract |
|---|---|
| `package.json` (scripts section) | npm/pnpm/yarn commands |
| `Makefile` / `Justfile` | build and test targets |
| `pyproject.toml` / `setup.py` | Python build config |
| `Cargo.toml` | Rust build config |
| `go.mod` | Go module info |
| CI config (`.github/workflows/`, `.gitlab-ci.yml`) | Required checks |
| `Dockerfile` / `docker-compose.yml` | Container setup |

**Extract the actual commands. Do not guess.**

### 2. Discover Project Structure

- List top-level directories and their purpose
- Identify the source root, test root, and output directories
- Note monorepo structure if applicable

### 3. Discover Conventions

Look for evidence of:
- Linting config (`.eslintrc`, `ruff.toml`, `.golangci.yml`)
- Formatting config (`.prettierrc`, `rustfmt.toml`, `black` in pyproject.toml)
- Type checking (`tsconfig.json`, `mypy.ini`, `pyright`)
- Import ordering rules
- Naming conventions (check existing code, not aspirational docs)

### 4. Discover CI Requirements

From CI config, extract:
- What checks must pass before merge
- Required test coverage thresholds
- Branch protection rules

---

## Output: CLAUDE.md

Target: **under 50 lines.** Every line must earn its place.

```markdown
# CLAUDE.md

## Build & Test
[actual commands to build, test, lint, format]

## Project Structure
[3-5 line overview of key directories]

## Conventions
[only conventions with tooling enforcement — if there is no linter rule, it is aspirational]

## CI Requirements
[what must pass before merge]
```

**Example:**

```markdown
# CLAUDE.md

## Build & Test
- `npm run build` — TypeScript compilation
- `npm test` — Jest test suite
- `npm run lint` — ESLint + Prettier check
- `npm run typecheck` — tsc --noEmit

## Project Structure
- `src/` — application source (TypeScript)
- `tests/` — Jest test files, mirrors src/ structure
- `dist/` — build output (gitignored)

## Conventions
- ESLint enforces camelCase for variables, PascalCase for components
- Prettier formats on save (see .prettierrc)
- Strict TypeScript (noImplicitAny, strictNullChecks)

## CI Requirements
- All tests pass, lint clean, typecheck clean before merge
- Coverage threshold: 80% lines
```

---

## Output: AGENTS.md (Optional)

Only create if the project uses subagents or multi-agent patterns.
Same rules: empirical, minimal, under 50 lines.

---

## Do NOT

- **Do NOT create bloated CLAUDE.md files (100+ lines).** Nobody reads them, and they waste context window.
- **Do NOT include aspirational rules.** "We follow clean architecture" means nothing without tooling enforcement.
- **Do NOT duplicate the README.** CLAUDE.md is for the AI, not for humans.
- **Do NOT list every file in the project.** Only mention files the AI needs to know about and would not find on its own.
- **Do NOT include style guides without enforcement.** "Use camelCase" means nothing without a linter rule. Instead: "ESLint enforces camelCase (see .eslintrc)."
- **Do NOT guess build commands.** Run them to verify they work.

---

## Verification

After generating:
1. Run every build/test command listed to confirm they work
2. Verify file paths mentioned actually exist
3. Confirm CI requirements match the actual CI config
4. Check total line count — if over 50, cut more

---

## Key Principles

- **Empirical over aspirational.** Only document what the codebase actually does.
- **Commands over descriptions.** `npm test` is better than "run the test suite."
- **50-line budget forces prioritization.** If you cannot fit it in 50 lines, you are including things the AI does not need.
- **Update, do not accumulate.** When the project changes, rewrite CLAUDE.md from scratch rather than appending.
