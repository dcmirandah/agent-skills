---
name: writing-plans
description: Use when you have a spec or requirements and need a step-by-step implementation plan, before touching code.
---

# Writing Plans

Write the plan as if for an engineer who knows zero about this codebase and has questionable taste. Spell out the files to touch per task, exact commands, expected output, code blocks, and tests. Bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume the reader is a skilled developer but knows almost nothing about this repo's tools, frameworks, and conventions or the problem domain, and has weak test design instincts.

**Announce:** "I'm using `powers:writing-plans` to create the implementation plan."

**Context:** run inside a worktree (created by `powers:brainstorming`).

**Save plans to:** `docs/powers/plans/YYYY-MM-DD-<feature-name>.md` (user override wins).

## Scope check

If the spec covers multiple independent subsystems, it should have been split during brainstorming. If it wasn't, suggest one plan per subsystem. Each plan must produce working, testable software on its own.

## File structure

Map files before tasks. This locks in decomposition.

- One clear responsibility per file. Well-defined interfaces between files.
- Smaller, focused files beat large ones — easier to hold in context, more reliable to edit.
- Files that change together live together. Split by responsibility, not by layer.
- In existing codebases, follow established patterns. Don't unilaterally restructure — but if a file you're modifying has grown unwieldy, a split is reasonable.

## Bite-sized task granularity

Each step is one action (2-5 minutes):

- "Write the failing test" — step.
- "Run it to confirm it fails" — step.
- "Write minimal code to pass" — step.
- "Run tests to confirm they pass" — step.
- "Commit" — step.

## Plan header

Every plan starts with:

```markdown
# <Feature Name> Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL — use `powers:subagent-driven-development` (recommended) or `powers:executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** <one sentence>

**Architecture:** <2-3 sentences>

**Tech Stack:** <key technologies / providers / pipeline tasks>

---
```

## Task structure

````markdown
### Task N: <Component>

**Files:**

- Create: `exact/path/to/file.tf`
- Modify: `exact/path/to/existing.yml:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: write the failing test**

```python
def test_specific_behavior():
    assert function(input) == expected
```

- [ ] **Step 2: run test to confirm it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined".

- [ ] **Step 3: write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: run test to confirm it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS.

- [ ] **Step 5: commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

For infrastructure-as-code tasks, use the project's validate/plan (or equivalent) and assert the expected diff. For CI config, use schema validation or a dry-run/preview. For Markdown, use `markdownlint <file>` (or the project's doc linter).

## No placeholders

Every step must contain actual content. The following are plan failures — never write them:

- "TBD", "TODO", "implement later", "fill in details".
- "Add appropriate error handling" / "validation" / "edge cases".
- "Write tests for the above" without the test code.
- "Similar to Task N" — repeat the code (the engineer may read out of order).
- Steps describing what to do without showing how (code blocks required for code steps).
- References to types / functions / resources not defined in any task.

## Rules

- Exact file paths.
- Complete code in every code step.
- Exact commands with expected output.
- DRY, YAGNI, TDD, frequent commits.

## Self-review

After writing the full plan, look at it with fresh eyes against the spec. Run yourself through:

1. **Spec coverage:** every requirement → a task. List gaps.
2. **Placeholder scan:** any "No placeholders" pattern? Fix inline.
3. **Type / name consistency:** signatures and identifiers used in later tasks match earlier tasks. `clearLayers()` in Task 3 vs `clearFullLayers()` in Task 7 is a bug.

Fix issues inline. No re-review loop. If a spec requirement has no task, add the task.

## Execution handoff

After saving, offer the choice:

> Plan complete and saved to `docs/powers/plans/<filename>.md`. Two execution options:
>
> 1. **Subagent-driven (recommended)** — fresh subagent per task with two-stage review.
> 2. **Inline execution** — execute tasks in this session via `powers:executing-plans`.
>
> Which approach?

- Subagent-driven → **REQUIRED:** `powers:subagent-driven-development`.
- Inline → **REQUIRED:** `powers:executing-plans`.
