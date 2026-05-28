---
name: executing-plans
description: Use when you have a written implementation plan to execute in the current session with periodic review checkpoints.
---

# Executing Plans

Load plan, review critically, execute tasks one by one, report when complete.

**Announce:** "I'm using `powers:executing-plans` to implement this plan."

> Prefer `powers:subagent-driven-development` when subagents are available — quality is significantly higher.

## Process

### 1. Load and review

1. Read the plan file.
2. Identify questions or concerns. Raise them with the user before starting.
3. If clear, create todos (one per task) and proceed.

### 2. Execute tasks

For each task:

1. Mark in_progress.
2. Follow steps exactly — plans contain bite-sized steps for a reason.
3. Run the verification commands the plan specifies (e.g., unit tests, lint, build, IaC validate/plan, CI dry-run).
4. Mark completed.

### 3. Finish

After all tasks pass verification:

- Announce: "I'm using `powers:finishing-a-development-branch` to complete this work."
- **REQUIRED:** `powers:finishing-a-development-branch` — verify tests, present options, execute choice.

## Stop when

- A blocker appears (missing dep, failing test, unclear instruction).
- The plan has critical gaps.
- Verification fails repeatedly.
- You don't understand a step.

Ask, don't guess. Don't force through blockers.

## Revisit step 1 when

- The user updates the plan.
- The approach needs rethinking.

## Rules

- Review the plan critically before starting.
- Follow steps exactly. Don't skip verifications.
- Reference skills the plan names.
- Never start implementation on `main`/`master` without explicit consent.

## Integration

- **Source:** `powers:writing-plans` produces the plan you execute here.
- **Required after:** `powers:finishing-a-development-branch`.
