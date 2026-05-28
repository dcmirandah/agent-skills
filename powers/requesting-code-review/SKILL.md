---
name: requesting-code-review
description: Use when completing tasks, finishing major features, or before merging — to verify the work meets requirements via a fresh-context reviewer.
---

# Requesting Code Review

Dispatch a `powers:code-reviewer` subagent (via `task`) to catch issues before they cascade. The reviewer gets only the context you craft — never your session history. This keeps the reviewer focused on the work product and preserves your own context.

**Core principle:** review early, review often.

## When

**Mandatory:**

- After each task in subagent-driven development.
- After completing a major feature.
- Before merge to `main`.

**Useful:**

- When stuck (fresh perspective).
- Before refactoring (baseline check).
- After fixing a complex bug.

## How

**1. Get git SHAs:**

```bash
BASE_SHA=$(git rev-parse HEAD~1)   # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. Dispatch the reviewer subagent** (via `task`) using the template at `requesting-code-review/code-reviewer.md`. Fill placeholders:

- `{WHAT_WAS_IMPLEMENTED}` — what you just built.
- `{PLAN_OR_REQUIREMENTS}` — what it should do.
- `{BASE_SHA}` / `{HEAD_SHA}` — commits.
- `{DESCRIPTION}` — brief summary.

**3. Act on feedback:**

- Fix Critical issues immediately.
- Fix Important issues before proceeding.
- Note Minor issues for later.
- Push back with reasoning if the reviewer is wrong.

## Example

```text
Just completed Task 2: verification function.

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
HEAD_SHA=$(git rev-parse HEAD)

Dispatch powers:code-reviewer subagent
  WHAT_WAS_IMPLEMENTED: verifyIndex / repairIndex
  PLAN_OR_REQUIREMENTS: Task 2 of docs/powers/plans/deployment-plan.md
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661
  DESCRIPTION: 4 issue types, real tests

Reviewer returns:
  Strengths: clean architecture, real tests
  Important: missing progress indicators
  Minor: magic 100 for reporting interval
  Assessment: ready to proceed

Fix progress indicators → continue.
```

## Workflow integration

- **Subagent-driven development:** review after EACH task.
- **Executing plans:** review after each batch.
- **Ad-hoc:** review before merge / when stuck.

## Red flags

**Never:**

- Skip review because "it's simple".
- Ignore Critical issues.
- Proceed with unfixed Important issues.
- Argue with valid technical feedback.

**If reviewer is wrong:** push back with reasoning, show the working code/tests, request clarification.

Template: `requesting-code-review/code-reviewer.md`.
