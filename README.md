# Superpowers (AdityaVG13 Fork)

Agentic skills for Claude Code — forked from [obra/superpowers](https://github.com/obra/superpowers) with additional skills and a safety hook.

## What's Different

This fork adds **3 new skills** and a **safety hook** on top of the original superpowers skills.

**Added skills:**
- `frontend-design` — production-grade UI standards (a11y, responsive, dark mode)
- `token-efficiency` — always-on rules for minimizing token waste
- `claude-md-creator` — generates minimal CLAUDE.md files from codebase inspection

**Safety hook:**
- `block-dangerous-commands` — PreToolUse hook that blocks destructive shell commands

> **Note:** 5 skills previously in this fork (`deliberation`, `premise-check`, `self-consistency-reasoner`, `error-recovery`, `context-management`) have been migrated to a private plugin with enhanced, autoresearch-optimized versions. The upstream versions of these skills remain available through the original [obra/superpowers](https://github.com/obra/superpowers).

---

## Skills

All skills from the upstream [superpowers](https://github.com/obra/superpowers) framework, plus:

| Skill | Description |
|---|---|
| **brainstorming** | Socratic design refinement before writing code |
| **claude-md-creator** | Creates minimal CLAUDE.md/AGENTS.md by inspecting the codebase |
| **dispatching-parallel-agents** | Concurrent subagent workflows for independent tasks |
| **executing-plans** | Batch execution of implementation plans with checkpoints |
| **finishing-a-development-branch** | Merge/PR decision workflow after implementation |
| **frontend-design** | Production UI standards: WCAG 2.1 AA, responsive, dark mode |
| **receiving-code-review** | Technical rigor when responding to review feedback |
| **requesting-code-review** | Pre-review checklist before merging |
| **subagent-driven-development** | Fast iteration with two-stage review (spec + quality) |
| **systematic-debugging** | 4-phase root cause debugging process |
| **test-driven-development** | RED-GREEN-REFACTOR cycle enforcement |
| **token-efficiency** | Always-on rules for minimizing token waste |
| **using-git-worktrees** | Isolated git worktrees for parallel development |
| **using-superpowers** | Introduction to the skills system and routing |
| **verification-before-completion** | Evidence-based verification before claiming done |
| **writing-plans** | Detailed implementation plans from specs |
| **writing-skills** | Guide for creating new skills |

---

## Safety Hook

The `block-dangerous-commands` hook runs as a PreToolUse hook on every Bash tool call. It blocks destructive commands at three configurable levels:

| Level | What It Blocks |
|---|---|
| **critical** | Disk wipes, partition formatting, boot sector destruction |
| **high** (default) | critical + force push main, hard resets, reading secrets, env dumps |
| **strict** | high + sudo, curl-pipe-shell, chmod 777, global npm install |

Set the level via the `SAFETY_LEVEL` environment variable. Blocked commands are logged to `~/.claude/hooks-logs/`.

---

## Installation

### Claude Code (Plugin Marketplace)

```bash
/plugin marketplace add AdityaVG13/superpowers
/plugin install superpowers@superpowers
```

### Manual (Clone)

```bash
git clone https://github.com/AdityaVG13/superpowers.git ~/.claude/plugins/superpowers
```
