---
name: premise-check
description: "Validates whether proposed work should exist before investing in it. Invoke BEFORE designing, planning, or building anything non-trivial to catch flawed premises, unnecessary complexity, or wrong-problem solutions early."
---

# Premise Check

A pre-flight validation that the work you are about to do is worth doing.
Run this BEFORE brainstorming, designing, or building anything non-trivial.

---

## Scope

Use this skill when:
- Starting any new feature, tool, or system
- A request feels like it might be solving the wrong problem
- You notice yourself building something complex to solve something simple
- Requirements come from assumptions rather than evidence

Do NOT use when:
- The user explicitly says "just do it" or "skip validation"
- The task is a known bug fix with a clear reproduction
- The work is a minor refactor under 20 lines

---

## The Four Questions

Ask each question honestly. If any answer is unsatisfying, pause and discuss with the user before proceeding.

### 1. Does this solve a real problem?

- Can you name a specific person or scenario that has this problem today?
- Is the problem frequent enough to justify a solution?
- Is the pain real, or is this anticipating a problem that may never materialize?

**Red flags:** "We might need this someday," "It would be nice to have," "Other projects do this."

### 2. Is there a simpler alternative?

- Could this be solved with configuration instead of code?
- Could an existing tool, library, or pattern handle this?
- Could a manual process work until the problem is better understood?
- What is the simplest thing that could possibly work?

**Red flags:** Building a framework when a function would do. Adding infrastructure when a script would suffice.

**Example:** User asks for a caching layer. Simpler alternative: add an HTTP `Cache-Control` header. Even simpler: check if the endpoint is actually slow first.

### 3. What assumptions are we making?

List every assumption embedded in the proposed approach:
- About user behavior
- About scale and performance requirements
- About the stability of requirements
- About the technical environment
- About what "done" looks like

For each assumption, rate confidence:
- **Known** — have evidence
- **Believed** — reasonable but unverified
- **Assumed** — no evidence, just convention

### 4. What evidence would change our mind?

Name specific, concrete findings that would make you abandon or fundamentally redesign this approach:
- "If we discover users actually do X instead of Y"
- "If the existing system already handles this case"
- "If the performance requirement is actually 10x lower than assumed"

If you cannot name anything that would change your mind, you are not thinking critically enough.

---

## Output Format

Present findings as:

> **Problem:** [one sentence]
>
> **Verdict:** Proceed / Proceed with caution / Reconsider
>
> **Simpler alternatives considered:** [list]
>
> **Key assumptions (confidence):**
> - [assumption] — [Known/Believed/Assumed]
>
> **Would reconsider if:** [list]

---

## After the Check

- **Proceed:** Move to brainstorming or planning with documented assumptions.
- **Proceed with caution:** Move forward but validate the "Believed" assumptions early.
- **Reconsider:** Discuss alternatives with the user before investing further.

---

## Do NOT

- **Do NOT skip this for "obvious" features.** Obvious features often hide flawed premises.
- **Do NOT let the check take longer than the work.** Five minutes maximum for small tasks.
- **Do NOT present the check as a blocker.** It is a conversation, not a gate.
- **Do NOT conclude "Reconsider" without offering concrete alternatives.** The user needs a path forward.
- **Do NOT re-run premise-check after the user has already approved the premise.** One check per decision.

---

## Key Principles

- **Five minutes of premise-checking saves five hours of wrong-direction work.**
- **"We should build X" is not a premise. "Users need X because Y" is a premise.**
- **Simpler is not lazier.** The best solution is often the least impressive one.
- **Assumptions are not bad — unexamined assumptions are.**
