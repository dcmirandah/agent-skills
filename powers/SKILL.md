---
name: powers
description: Router for powers/* workflow skills (brainstorm, plan, execute, debug, verify, review, finish). MANDATORY - invoke powers:using-powers at the start of every conversation.
---

# powers — router - meta skill

Pick the next skill in `powers/*`. This file routes; it does not do work.

<MANDATORY-AT-START>
Begin every new conversation with `powers:using-powers` skill.
</MANDATORY-AT-START>

## Pick by situation

- Unsure / start of conversation → `powers:using-powers`
- Design, spec, or behavior change → `powers:brainstorming`
- Spec ready, need a step-by-step plan → `powers:writing-plans`
- Plan → implement with per-task review → `powers:subagent-driven-development`
- Plan → inline execution with checkpoints → `powers:executing-plans`
- Run independent investigations in parallel → `powers:dispatching-parallel-agents`
- Bug, flaky pipeline, unexpected behavior → `powers:systematic-debugging`
- New feature or bugfix code → `powers:test-driven-development`
- Before claiming done / fixed / passing → `powers:verification-before-completion`
- Major chunk done, want review → `powers:requesting-code-review`
- Got review feedback → `powers:receiving-code-review`
- Merge, PR, branch cleanup → `powers:finishing-a-development-branch`
- Creating or editing a skill → `powers:writing-skills`

## Rules

- Process skills (brainstorm, debug, verify) before implementation skills.
- User instructions (CLAUDE.md, AGENTS.md, direct ask) override skill content.
- Don't restate sub-skill workflows here; load the sub-skill.

## Common pipeline

`using-powers` → `brainstorming` → `writing-plans` → `subagent-driven-development` (or `executing-plans`) → `requesting-code-review` → `verification-before-completion` → `finishing-a-development-branch`

## Cross-harness

Sub-skills use Copilot-native or platform-neutral tool names directly.
