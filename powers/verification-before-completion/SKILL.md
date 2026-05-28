---
name: verification-before-completion
description: Use before claiming work is complete, fixed, or passing — and before committing or opening a PR. Requires running fresh verification commands and reading the output before any success claim.
---

# Verification Before Completion

Claiming work is complete without verification is dishonesty, not efficiency.

**Core principle:** evidence before claims, always.

**Violating the letter of this rule is violating the spirit of it.**

## The Iron Law

```text
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

If you haven't run the verification command in this message, you cannot claim it passes.

## The Gate

Before claiming any status or expressing satisfaction:

1. **IDENTIFY** what command proves the claim.
2. **RUN** the full command, fresh.
3. **READ** the full output, exit code, failure count.
4. **VERIFY** the output confirms the claim.
   - No → state actual status with evidence.
   - Yes → state claim with evidence.
5. Only then make the claim.

Skipping any step = lying.

## Verification commands by stack

| Claim | Command (typical) | Pass criterion |
|---|---|---|
| IaC validates | project validate command (e.g. `terraform validate`, `pulumi preview`) | exit 0 |
| IaC format/lint ok | project format check (e.g. `terraform fmt -check`, `tofu fmt -check`) | exit 0 |
| Markdown lint clean | `markdownlint <files>` | 0 errors |
| Bash script ok | `shellcheck <file>` | 0 errors |
| Python tests pass | `pytest` | 0 failures |
| Build succeeds | project build command | exit 0 |
| Bug fixed | test reproducing the original symptom | passes |
| Regression test works | red → revert fix → red → restore → green | full red-green cycle observed |
| Subagent task done | `git diff` + spot-check | changes match the report |
| Requirements met | re-read spec → checklist each item | all items observed |

## Red flags — STOP

- "should", "probably", "seems to".
- "Great!", "Perfect!", "Done!" before evidence.
- About to commit / push / open a PR without running checks.
- Trusting a subagent's success report without diff inspection.
- Relying on partial verification.
- "Just this once" / "I'm tired" / "I'm confident".
- Any wording implying success without having run the command.

## Rationalization table

| Excuse | Reality |
|---|---|
| "Should work now" | Run the verification. |
| "I'm confident" | Confidence ≠ evidence. |
| "Just this once" | No exceptions. |
| "Linter passed" | Linter ≠ compiler ≠ tests. |
| "Subagent said success" | Verify via diff and re-run. |
| "Partial check is enough" | Partial proves nothing. |
| "Different words so the rule doesn't apply" | Spirit over letter. |

## Patterns

**Tests:**

```text
✅ run → see 34/34 pass → "all tests pass"
❌ "should pass now"
```

**Regression test (TDD red-green):**

```text
✅ write → pass → revert fix → MUST FAIL → restore → pass
❌ "I added a regression test" (no red-green cycle)
```

**Build:**

```text
✅ run build → exit 0 → "build passes"
❌ "linter passed" (linter does not compile)
```

**Requirements:**

```text
✅ re-read plan → checklist each item → report gaps or completion
❌ "tests pass, phase complete"
```

**Subagent delegation:**

```text
✅ subagent reports success → check git diff → verify changes → report actual state
❌ trust the subagent
```

## When to apply

Always before:

- Any success / completion claim.
- Any expression of satisfaction.
- Committing, pushing, opening a PR, marking a task done.
- Moving to the next task.
- Delegating to subagents.

The rule applies to exact phrases, paraphrases, synonyms, and any implication of success.

## Bottom line

Run the command. Read the output. Then claim the result. Non-negotiable.
