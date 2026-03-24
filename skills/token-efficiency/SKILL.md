---
name: token-efficiency
description: "Always-on background rules for minimizing token waste: parallel tool calls, no redundant reads, targeted file access, compressed context, direct answers without preamble. Applied automatically to every session."
user-invocable: false
---

# Token Efficiency

An always-on operational standard applied automatically to every session.
These rules reduce token waste without sacrificing quality.

---

## Scope

These rules apply to **every interaction**. They are background knowledge, not a workflow to invoke.

If a rule here conflicts with a task-specific skill (e.g., systematic-debugging requires reading a file you already read), the task-specific skill takes precedence.

---

## Rules

### 1. Parallel Tool Calls

When multiple tool calls are independent (no data dependency between them), execute them in a single turn.

**Do:**
```
[Read file A] [Read file B] [Read file C]  <- all in one turn
```

**Do NOT:**
```
[Read file A] -> wait -> [Read file B] -> wait -> [Read file C]
```

### 2. No Redundant Reads

If you have already read a file in this session, do not read it again unless:
- It may have changed (you or the user edited it)
- You need a different section of a large file
- Context was compacted and you no longer have the content

Before issuing a Read, ask: "Do I already have this content?"

### 3. Targeted File Access

Read only what you need, not entire files.

**Do:**
- Use `offset` and `limit` to read specific sections of large files
- Use Grep to find the relevant section first, then Read that section
- Read function signatures before reading function bodies

**Do NOT:**
- Read a 2000-line file to find a 5-line function
- Read every file in a directory "just in case"

### 4. Track Exploration

Maintain a mental map of what you have explored. Before searching:
- "Have I already searched for this?"
- "Did a previous search result already answer this?"
- "Can I infer this from files I have already read?"

### 5. Compress Context at Boundaries

At logical boundaries (finishing a subtask, switching focus):
- Summarize findings so far internally
- Release details you no longer need
- Keep only: conclusions, file paths, and next steps

### 6. Direct Answers

- No preamble ("Great question!" / "Let me help you with that" / "I'd be happy to")
- No restating the question back to the user
- No summarizing what you are about to do — just do it
- No narrating tool calls — results speak for themselves
- Conclusions first, supporting details second

### 7. Efficient Search Strategy

1. Start with Glob for file discovery (fast, no content scanning)
2. Use Grep with targeted patterns (specific function names, error messages)
3. Read files only after you know which files matter
4. Search broadly only when you genuinely do not know where to look

### 8. Batch Operations

When making multiple edits to the same file:
- Plan all edits, then execute sequentially
- Do not re-read the file between edits unless the edit changes context you need

---

## Anti-Patterns

| Anti-Pattern | Token Cost | Fix |
|---|---|---|
| Reading a file you just wrote | ~1000 tokens | Trust your own writes |
| Searching for something you already found | ~500 tokens | Track results |
| Full file read for a single function | ~2000 tokens | Grep then targeted read |
| Narrating each step before doing it | ~200 tokens/step | Just execute |
| Re-reading state.md mid-session | ~500 tokens | Read once at start |

---

## Do NOT

- **Do NOT read files speculatively.** Know why you need a file before opening it.
- **Do NOT repeat information the user already stated.** Acknowledge and move forward.
- **Do NOT generate verbose explanations when a short answer suffices.**
- **Do NOT re-read files you wrote in the same session** unless the user edited them.
- **Do NOT issue sequential tool calls when they could run in parallel.**

---

## Measurement

If available, use `rtk gain` to track token savings. Target: 50%+ reduction vs. naive approach.
