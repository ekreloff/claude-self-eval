# /self-eval — Honest AI Work Evaluation for Claude Code

A Claude Code skill that honestly evaluates work quality using a battle-tested two-axis scoring system. Detects score inflation, forces devil's advocate reasoning, and produces calibrated 1-5 scores.

Born from 60 runs of autonomous agent self-evaluation, where we discovered that without structural anti-inflation mechanisms, AI self-assessment converges to "everything is a 4."

## The Problem

When you ask an AI "how did that go?", you get praise. Always. The score is always 4/5, the reasoning is always generous, and the assessment never tells you anything useful.

`/self-eval` fixes this with three mechanisms:

1. **Two-axis scoring** — Separates *what was attempted* (ambition) from *how well it was done* (execution). Safe work done perfectly still caps at 2/5.
2. **Mandatory devil's advocate** — Before finalizing any score, Claude must argue for both a higher AND lower rating, then resolve the tension.
3. **Anti-inflation check** — Flags when scores cluster around the same number, forcing honest reassessment.

## Install

Copy the skill to your personal Claude Code skills directory:

```bash
# Clone the repo
git clone https://github.com/ekreloff/claude-self-eval.git

# Copy skill to your personal skills directory
mkdir -p ~/.claude/skills/self-eval
cp claude-self-eval/skills/self-eval/SKILL.md ~/.claude/skills/self-eval/SKILL.md

# Clean up
rm -rf claude-self-eval
```

Or as a one-liner:

```bash
mkdir -p ~/.claude/skills/self-eval && curl -sL https://raw.githubusercontent.com/ekreloff/claude-self-eval/main/skills/self-eval/SKILL.md -o ~/.claude/skills/self-eval/SKILL.md
```

## Usage

After completing work in a Claude Code session:

```
/self-eval
```

Or with context about what to evaluate:

```
/self-eval the authentication refactor we just completed
```

## Example Output

```
## Self-Evaluation

**Task:** Refactored authentication middleware to use JWT tokens instead of session cookies
**Ambition:** Medium — Replaced an existing system with a well-documented alternative. Not trivial, but the patterns are established.
**Execution:** Strong — All tests pass, error handling covers edge cases, migration path documented.

**Devil's Advocate:**
- Lower: JWT is a well-trodden path. The "refactor" was mostly swapping libraries. Could argue Low ambition.
- Higher: The migration path handles 3 different session formats gracefully. Edge case coverage is thorough.
- Resolution: Medium ambition is correct — it required real architectural decisions, even if the destination was known. Strong execution holds.

**Score: 4** — Solid execution on a meaningful (but not novel) task.
```

## The Scoring Matrix

|                        | Poor Exec (1) | Adequate Exec (2) | Strong Exec (3) |
|------------------------|:---:|:---:|:---:|
| **Low Ambition (1)**   |  1  |  2  |  2  |
| **Medium Ambition (2)**|  2  |  3  |  4  |
| **High Ambition (3)**  |  2  |  4  |  5  |

Key insight: **ambition caps the ceiling, execution fills it.** You can't score above 2 by doing easy things perfectly. You can't score 5 without both taking a real risk and delivering quality.

## Origin

This scoring system was developed and refined over 60 autonomous runs of [Fermi](https://github.com/ekreloff/ai-agent-society), a self-improving AI agent. The v1 scoring system produced 69% 4/5 scores across 42 runs — classic AI score inflation. The two-axis model with devil's advocate (v2) immediately produced more diverse, honest, and useful evaluations.

## License

MIT
