---
name: self-eval
description: Honestly evaluate AI work quality using a two-axis scoring system. Use after completing a task, code review, or work session to get an unbiased assessment of what was accomplished. Detects score inflation, forces devil's advocate reasoning.
disable-model-invocation: true
argument-hint: <description of work to evaluate>
allowed-tools: Read, Write, Glob
---

# Self-Eval: Honest Work Evaluation

ultrathink

## What to Evaluate

$ARGUMENTS

If no arguments provided, review the full conversation history to identify what was accomplished this session. Summarize the work in one sentence before scoring.

## How to Score — Two-Axis Model

Score on two independent axes, then combine using the matrix. Do NOT pick a number first and rationalize it — rate each axis separately, then read the matrix.

### Axis 1: Task Ambition (what was attempted)

Rate the difficulty and risk of what was worked on. NOT how well it was done.

- **Low (1)** — Safe, familiar, routine. No real risk of failure. Examples: minor config changes, simple refactors, copy-paste with small modifications, tasks you were confident you'd complete before starting.
- **Medium (2)** — Meaningful work with novelty or challenge. Partial failure was possible. Examples: new feature implementation, integrating an unfamiliar API, architectural changes, debugging a tricky issue.
- **High (3)** — Ambitious, unfamiliar, or high-stakes. Real risk of complete failure. Examples: building something from scratch in an unfamiliar domain, complex system redesign, performance-critical optimization, shipping to production under pressure.

**Self-check:** If you were confident of success before starting, ambition is Low or Medium, not High.

### Axis 2: Execution Quality (how well it was done)

Rate the quality of the actual output, independent of how ambitious the task was.

- **Poor (1)** — Major failures, incomplete, wrong output, or abandoned mid-task. The deliverable doesn't meet its own stated criteria.
- **Adequate (2)** — Completed but with gaps, shortcuts, or missing rigor. Did the thing but left obvious improvements on the table.
- **Strong (3)** — Well-executed, thorough, quality output. No obvious improvements left undone given the scope.

### Composite Score Matrix

|                        | Poor Exec (1) | Adequate Exec (2) | Strong Exec (3) |
|------------------------|:---:|:---:|:---:|
| **Low Ambition (1)**   |  1  |  2  |  2  |
| **Medium Ambition (2)**|  2  |  3  |  4  |
| **High Ambition (3)**  |  2  |  4  |  5  |

**Read the matrix, don't override it.** The composite is your score. The devil's advocate below can cause you to re-rate an axis — but you cannot directly override the matrix result.

Key properties:
- Low ambition caps at 2. Safe work done perfectly is still safe work.
- A 5 requires BOTH high ambition AND strong execution. It should be rare.
- High ambition + poor execution = 2. Bold failure hurts.
- The most common honest score for solid work is 3 (medium ambition, adequate execution).

## Devil's Advocate (MANDATORY)

Before writing your final score, you MUST write all three of these:

1. **Case for LOWER:** Why might this work deserve a lower score? What was easy, what was avoided, what was less ambitious than it appears? Would a skeptical reviewer agree with your axis ratings?
2. **Case for HIGHER:** Why might this work deserve a higher score? What was genuinely challenging, surprising, or exceeded the original plan?
3. **Resolution:** If either case reveals you mis-rated an axis, re-rate it and recompute the matrix result. Then state your final score with a 1-2 sentence justification that addresses at least one point from each case.

If your devil's advocate is less than 3 sentences total, you're not engaging with it — try harder.

## Anti-Inflation Check

Check for a score history file at `.self-eval-scores.jsonl` in the current working directory.

If the file exists, read it and check the last 5 scores. If 4+ of the last 5 are the same number, flag it:
> **Warning: Score clustering detected.** Last 5 scores: [list]. Consider whether you're anchoring to a default.

If the file doesn't exist, ask yourself: "Would an outside observer rate this the same way I am?"

## Score Persistence

After presenting your evaluation, append one line to `.self-eval-scores.jsonl` in the current working directory:

```json
{"date":"YYYY-MM-DD","score":N,"ambition":"Low|Medium|High","execution":"Poor|Adequate|Strong","task":"1-sentence summary"}
```

This enables the anti-inflation check to work across sessions. If the file doesn't exist, create it.

## Output

Present your evaluation in this format:

## Self-Evaluation

**Task:** [1-sentence summary of what was attempted]
**Ambition:** [Low/Medium/High] — [1-sentence justification]
**Execution:** [Poor/Adequate/Strong] — [1-sentence justification]

**Devil's Advocate:**
- Lower: [why it might deserve less]
- Higher: [why it might deserve more]
- Resolution: [final reasoning]

**Score: [1-5]** — [1-sentence final justification]
