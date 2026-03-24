# Superpowers (AdityaVG13 Fork)

Agentic skills for Claude Code — forked from [obra/superpowers](https://github.com/obra/superpowers) with additional skills and safety hooks.

## What's Different

This fork adds **8 new skills** and a **safety hook** on top of the 14 original superpowers skills:

**New skills:**
- `deliberation` — multi-perspective decision framework
- `premise-check` — validates whether proposed work should exist
- `context-management` — persistent state across sessions via state.md
- `error-recovery` — project-specific known-issues knowledge base
- `frontend-design` — production-grade UI standards (a11y, responsive, dark mode)
- `token-efficiency` — always-on rules for minimizing token waste
- `self-consistency-reasoner` — multi-path reasoning for high-stakes inference
- `claude-md-creator` — generates minimal CLAUDE.md files from codebase inspection

**New hook:**
- `block-dangerous-commands` — PreToolUse safety hook that blocks destructive shell commands

---

## Skills

| Skill | Description |
|---|---|
| **brainstorming** | Socratic design refinement before writing code |
| **claude-md-creator** | Creates minimal CLAUDE.md/AGENTS.md by inspecting the codebase |
| **context-management** | Persists session state across boundaries via state.md |
| **deliberation** | Multi-perspective analysis for complex architectural decisions |
| **dispatching-parallel-agents** | Concurrent subagent workflows for independent tasks |
| **error-recovery** | Known-issues knowledge base consulted before debugging |
| **executing-plans** | Batch execution of implementation plans with checkpoints |
| **finishing-a-development-branch** | Merge/PR decision workflow after implementation |
| **frontend-design** | Production UI standards: WCAG 2.1 AA, responsive, dark mode |
| **premise-check** | Pre-flight validation that proposed work is worth doing |
| **receiving-code-review** | Technical rigor when responding to review feedback |
| **requesting-code-review** | Pre-review checklist before merging |
| **self-consistency-reasoner** | Multi-path reasoning for high-confidence conclusions |
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
| **critical** | `rm -rf /`, `mkfs`, `dd of=/dev/`, `chmod -R 777 /`, wipe boot |
| **high** (default) | critical + force push main, `git reset --hard`, reading secrets, env dumps, netcat listeners |
| **strict** | high + `sudo`, curl-pipe-shell, `chmod 777`, global npm install |

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

### Verify Installation

Start a new session and ask for something that triggers a skill (e.g., "help me plan this feature" or "let's debug this issue"). The agent should automatically invoke the relevant skill.

---

## Upstream

Forked from [obra/superpowers](https://github.com/obra/superpowers). Original skills, architecture, and plugin system by [Jesse Vincent](https://blog.fsck.com) and [Prime Radiant](https://primeradiant.com).

---

## License

MIT License — see [LICENSE](LICENSE) for details.
