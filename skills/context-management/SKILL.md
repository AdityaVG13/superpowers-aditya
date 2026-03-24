---
name: context-management
description: "Persists durable state across session boundaries via state.md. Use to save progress, restore context, hand off between sessions, or prepare for /compact."
disable-model-invocation: true
---

# Context Management

Maintains persistent state across session boundaries using `~/.claude/state.md`.
Use this to save progress, hand off between sessions, and avoid losing context.

**This skill is user-invoked only.** State saves should be explicit, not automatic.

---

## Scope

Use this skill when:
- The user asks to save progress or session state
- At the end of a long session before signing off
- After completing a major milestone
- Before a context window compaction (`/compact`)
- When switching between projects or tasks
- At session start to restore previous context

Do NOT use when:
- The user has not asked to save state
- The session is short and self-contained
- The information is already captured in CLAUDE.md or MEMORY.md

---

## State File Location

Always write to and read from: `~/.claude/state.md`

---

## State File Format

```markdown
# Session State

## Last Updated
YYYY-MM-DD HH:MM

## Current Project
[project name and path]

## Completed
- [what was finished, with file paths]
- [decisions made and why]

## In Progress
- [what is partially done]
- [current status and next immediate step]

## Pending
- [what has not been started yet]
- [ordered by priority]

## Key Decisions
- [decision]: [rationale] — [date]

## Blockers
- [anything preventing progress]

## Environment Notes
- [relevant setup details, ports, configs]
```

---

## Operations

### Save State

1. Read the current `state.md` if it exists
2. Merge new information — do not overwrite still-relevant context
3. Move completed items from "In Progress" to "Completed"
4. Update the timestamp
5. Write the file

Keep entries concise. File paths are more useful than descriptions of code.

### Restore State

1. Read `~/.claude/state.md`
2. Summarize current status to the user
3. Confirm the "In Progress" and "Pending" items are still accurate
4. Ask what to work on next

### Compact Handoff

Before a `/compact` event:
1. Save current state immediately
2. Include any information that would be lost during compaction
3. After compaction, re-read `state.md` as first action

---

## Do NOT

- **Do NOT auto-save state without the user asking.** State saves are user-controlled.
- **Do NOT overwrite state.md entirely.** Always merge with existing content.
- **Do NOT store code snippets in state.md.** Store file paths instead.
- **Do NOT keep completed items forever.** Remove items older than 2 sessions unless they contain important decisions.
- **Do NOT duplicate information already in CLAUDE.md or MEMORY.md.** State.md is for transient session state, not permanent project knowledge.

---

## Key Principles

- **State.md is ground truth.** If memory and state.md conflict, trust state.md.
- **File paths over descriptions.** `/src/auth/login.ts` is better than "the login component."
- **Decisions need rationale.** "Chose SQLite because embedded, no server needed" is useful. "Chose SQLite" is not.
- **Always read state.md at session start.** This is the single most important habit for continuity.
