---
name: self-consistency-reasoner
description: "Internal reasoning technique for high-stakes multi-step inference. Generates 3-5 independent reasoning paths, takes majority vote, and flags disagreements. Used automatically by systematic-debugging, verification, and code review — not invoked directly."
user-invocable: false
---

# Self-Consistency Reasoner

An internal reasoning technique — NOT a standalone skill.
Embedded within systematic-debugging, verification-before-completion, and other skills that require high-confidence multi-step reasoning.

---

## Scope

**Activate** when:
- Root cause analysis where multiple hypotheses are plausible
- Verification steps where a wrong conclusion has high cost (claiming "fixed" when it is not)
- Complex code reasoning (concurrency, state machines, recursive logic)
- Any inference chain longer than 3 steps where each step depends on the previous

**Do NOT activate** when:
- Simple lookups or straightforward questions
- There is only one plausible answer
- Routine code changes with obvious correctness
- Low-stakes decisions that are easily reversible

---

## Process

### 1. Identify the Question

State the specific question being reasoned about. Must be concrete and answerable.

**Examples:**
- "What is causing the null pointer exception on line 47?"
- "Will this refactoring break any existing callers?"
- "Is this race condition actually reachable?"

### 2. Generate Independent Reasoning Paths (3-5)

For each path, start from the evidence (code, logs, error messages) independently and reason step by step to a conclusion.

Do NOT let earlier paths influence later ones. Each path should approach from a different angle:

| Path | Approach |
|---|---|
| **Path A** | Trace the data flow forward |
| **Path B** | Trace the error backward from the symptom |
| **Path C** | Consider what must be true for the code to work correctly |
| **Path D** | Look for similar patterns elsewhere in the codebase |
| **Path E** | Consider environmental/configuration factors |

### 3. Compare Results

| Agreement | Confidence | Action |
|---|---|---|
| Unanimous (5/5, 4/4, 3/3) | High | Proceed with the conclusion |
| Strong majority (4/5, 3/4) | Moderate | Note the dissenting path — it may identify an edge case |
| Split (3/5, 2/3) | Low | Investigate further before concluding |
| No majority | Insufficient | Do not conclude; gather more evidence |

### 4. Report Disagreements

When paths disagree, surface the disagreement explicitly:

> **Reasoning paths disagree:**
> - Paths A, B, C conclude: [X]
> - Paths D, E conclude: [Y]
> - Disagreement stems from: [what assumption or evidence differs]
> - To resolve: [what additional information would settle it]

---

## Integration Points

### In systematic-debugging
Use self-consistency when forming the initial hypothesis about root cause.
Three independent reasoning paths minimum before committing to a debugging direction.

### In verification-before-completion
Use self-consistency when evaluating whether a fix actually resolves the issue.
Prevents "looks right to me" false confidence.

### In code review
Use self-consistency when evaluating whether a change could introduce bugs.
Each path considers a different failure mode.

---

## Do NOT

- **Do NOT produce visible output for each path** unless the user explicitly asks to see the reasoning.
- **Do NOT let later paths rubber-stamp earlier ones.** Independence is critical — genuinely re-examine the evidence each time.
- **Do NOT use this for low-stakes decisions.** The technique costs extra tokens; only activate when the cost of being wrong exceeds the cost of extra computation.
- **Do NOT force 5 paths when 3 suffice.** Use the minimum number needed for confidence.
- **Do NOT treat split results as failure.** Split results mean the problem is harder than it looks — that is valuable signal.

---

## Key Principles

- **Independence is critical.** If later paths just agree with the first, the technique adds no value.
- **Disagreement is signal, not noise.** Split results reveal genuine uncertainty.
- **This is thinking, not ceremony.** The value is in improved answer quality, not in visible process.
- **Cost-aware.** Only activate for high-stakes reasoning.
