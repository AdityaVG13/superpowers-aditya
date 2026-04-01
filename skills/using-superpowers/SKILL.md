---
name: using-superpowers
description: Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## Instruction Priority

Superpowers skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's explicit instructions** (CLAUDE.md, GEMINI.md, AGENTS.md, direct requests) — highest priority
2. **Superpowers skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

If CLAUDE.md, GEMINI.md, or AGENTS.md says "don't use TDD" and a skill says "always use TDD," follow the user's instructions. The user is in control.

## How to Access Skills

Use the `Skill` tool to invoke skills. Content is loaded and presented -- follow it directly.

# Using Skills

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means invoke it to check.

Flow: User message → Check for matching skills → Invoke Skill tool → Follow skill → Respond.
Process skills (brainstorming, debugging) first, then implementation skills.

## Core Skills (Tier 1 -- always loaded)

| Skill | When to Use |
|---|---|
| **brainstorming** | Before creative work, features, design |
| **crew** | 3+ files, architectural decisions, multi-phase work |
| **code-review** | Before PR, after completing a task |
| **systematic-debugging** | Any bug, test failure, unexpected behavior |
| **verification-before-completion** | Before claiming work is done |
| **commit** | Creating git commits |
| **create-pr** | Creating pull requests |
| **session-capture** | End of session, "wrap up", "save state" |
| **deep-research** | Multi-source research with citations |
| **security-review** | Auth, user input, secrets, API endpoints |

All other skills (writing-plans, TDD, frontend-design, executing-plans, deliberation, etc.) are available via the Skill tool. Use `/skill-name` or invoke by name.
