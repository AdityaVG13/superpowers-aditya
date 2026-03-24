---
name: deliberation
description: "Use BEFORE brainstorming when facing complex decisions, architectural trade-offs, technology selection, or design dilemmas where options aren't well-defined. Assembles multiple stakeholder perspectives to surface tensions without forcing premature choices."
---

# Multi-Perspective Deliberation

A structured decision framework for complex choices where the right path is not obvious.
Invoke this BEFORE brainstorming when the problem space itself is unclear.

---

## Scope

This skill applies to decisions where:
- Multiple valid options exist with competing constraints
- Stakeholders have legitimately different priorities
- Jumping straight to brainstorming would silently encode unexamined assumptions

This skill does NOT apply to:
- Clear requirements with obvious implementation paths — go straight to brainstorming
- Pure implementation questions — use systematic-debugging or writing-plans
- Decisions already made — do not re-litigate

---

## Process

### 1. Frame the Decision

State the decision clearly in one sentence. Identify:
- What we are deciding (not how)
- Why the decision matters
- What constraints are non-negotiable vs. flexible

**Example:**
> "Should we use a relational database or document store for the user activity log, given we need both fast writes and complex queries?"

### 2. Assemble Perspectives (3-5)

Select stakeholder perspectives relevant to this decision. Each perspective gets a name and role.

| Perspective | Speaks For |
|---|---|
| **The User** | End-user experience, simplicity, speed |
| **The Maintainer** | Future developers, readability, debuggability |
| **The Operator** | Deployment, monitoring, failure modes |
| **The Architect** | System boundaries, coupling, extensibility |
| **The Pragmatist** | Shipping timeline, team skills, existing code |

Choose perspectives that create genuine tension for THIS decision. If all perspectives agree, you picked the wrong perspectives or the decision is not actually hard.

### 3. Each Perspective Speaks Once

Each named perspective states:
- What they value most in this decision
- What risks they see
- What they would choose and why

Rules:
- **No debate.** Each perspective speaks independently.
- **No strawmen.** Each perspective makes its strongest honest case.
- **Keep it concrete.** Reference the actual system, not abstract principles.

### 4. Surface Convergence and Tensions

After all perspectives have spoken:
- **Convergence:** Where do 3+ perspectives agree? These are likely safe choices.
- **Tensions:** Where do perspectives genuinely conflict? Name the trade-off explicitly.
- **Hidden assumptions:** What did multiple perspectives take for granted that might be wrong?

### 5. Present to User

Deliver the analysis WITHOUT a recommendation. Format:

> **Decision:** [one sentence]
>
> **Perspectives heard:** [names]
>
> **They agree on:** [bullet points]
>
> **They disagree on:** [bullet points with the actual trade-off named]
>
> **Assumptions worth questioning:** [bullet points]

Then ask the user which tensions they want to resolve and how. Their answers become constraints for the brainstorming phase.

---

## Do NOT

- **Do NOT recommend a solution.** The goal is clarity about trade-offs, not consensus.
- **Do NOT roleplay as stakeholders.** Perspectives are analytical lenses, not characters.
- **Do NOT include perspectives that all agree.** Unanimous perspectives add no signal.
- **Do NOT use this for simple decisions.** If the answer is obvious, skip deliberation.
- **Do NOT run this after brainstorming has started.** It feeds INTO brainstorming, not the reverse.

---

## Key Principles

- **No premature convergence.** Tensions are features, not bugs.
- **This feeds into brainstorming.** The resolved tensions become design constraints.
- **3-5 perspectives maximum.** More than 5 creates noise without additional signal.
