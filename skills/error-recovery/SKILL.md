---
name: error-recovery
description: "Maintains a project-specific known-issues.md mapping recurring errors to their root causes and fixes. Consulted by systematic-debugging before investigation to avoid rediscovering known problems. Use when you hit a familiar error or fix a non-trivial bug."
---

# Error Recovery

A persistent knowledge base of project-specific errors and their solutions.
Prevents wasting time rediscovering fixes for problems that have already been solved.

---

## Scope

This skill handles three operations:
1. **Lookup** — check known-issues.md before debugging any error
2. **Record** — save a fix after solving a non-trivial error
3. **Prune** — remove entries that are no longer relevant

---

## File Location

- **Per-project:** `docs/known-issues.md` in the project root
- **Cross-project fallback:** `~/.claude/known-issues.md`

---

## Entry Format

Each entry in known-issues.md follows this structure:

```markdown
## [Error signature or short description]
**First seen:** YYYY-MM-DD
**Last seen:** YYYY-MM-DD
**Frequency:** once | occasional | frequent

**Symptoms:**
[What the user sees — exact error message, stack trace pattern, unexpected behavior]

**Root cause:**
[Why it happens — one or two sentences]

**Fix:**
[Exact steps to resolve. Include commands, file paths, code changes.]

**Prevention:**
[How to avoid this in the future, if applicable]

---
```

**Example:**

```markdown
## ECONNREFUSED on localhost:3000 during tests
**First seen:** 2025-11-01
**Last seen:** 2025-12-15
**Frequency:** occasional

**Symptoms:**
`Error: connect ECONNREFUSED 127.0.0.1:3000` when running integration tests.

**Root cause:**
Test setup does not wait for the dev server to be ready. Race condition on CI.

**Fix:**
Add `wait-on http://localhost:3000 --timeout 10000` before test command in `package.json`.

**Prevention:**
Use `start-server-and-test` package for integration tests.

---
```

---

## Operations

### Lookup (before debugging)

1. Read the project's `docs/known-issues.md`
2. Search for matching error signatures (error message substrings, stack trace patterns)
3. If found: apply the documented fix, update "Last seen" date
4. If not found: proceed with normal debugging via systematic-debugging

### Record (after fixing)

1. Determine if this error could recur (environment-specific, dependency-related, subtle logic issue)
2. If yes, append a new entry to known-issues.md
3. Include the **actual error message** or a representative pattern — not a paraphrase
4. The fix section must be copy-pasteable or directly actionable

### Prune (periodic maintenance)

Every ~10 sessions or on user request:
- Remove entries for errors that have been structurally prevented (code refactored, dependency replaced)
- Keep entries for environment-specific issues even if old — they tend to recur

---

## Do NOT

- **Do NOT record typos or simple syntax errors.** Only non-trivial errors that cost real debugging time.
- **Do NOT record one-time mistakes that will not recur.**
- **Do NOT record errors already documented in the project's official docs.**
- **Do NOT paraphrase error messages.** Use the exact text so future searches match.
- **Do NOT write vague fixes.** "Check your configuration" is not a fix. "Set `NODE_ENV=production` in `.env`" is.
- **Do NOT skip the lookup step.** Always check known-issues.md before forming hypotheses.

---

## Integration with systematic-debugging

The systematic-debugging skill should check known-issues.md as its **first step**, before forming hypotheses.
If the error matches a known issue, apply the fix directly.
If the fix does not work, update the known-issues entry with the new information and proceed with normal debugging.

---

## Key Principles

- **Error messages are search keys.** Include the exact text, not a summary.
- **Fixes must be actionable.** Commands, file paths, code changes — not descriptions.
- **Record root causes, not symptoms.** The fix should prevent the error, not hide it.
- **Cross-session value compounds.** Every recorded fix saves future debugging time.
