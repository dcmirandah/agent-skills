---
name: prompt-engineer
description: Rough intent -> copy-paste prompt for another AI. On prompt engineer,turn into prompt,make a prompt for,prompt for AI. Deliver prompt not task unless asked.
---

**Deliver:** one fenced prompt block, copy-ready. Optional 1-line assumptions only if they change behavior. No run downstream task unless asked.

## Flow

1. Intent — what downstream AI does (analyze, write, refactor, compare, …).
2. Context — chat, files, repo; **never invent** user facts.
3. Fill gaps — role, scope, inputs, constraints, output, done-when; tag guesses: `[Assumption: …]`.
4. Draft — self-contained; no fluff ("take your time") unless reasoning/safety needs it.
5. Check — executable without chat? One fork → branch in prompt ("If X… If Y…") or **one** short question.

## Infer (defaults)

| Signal | Default |
|--------|---------|
| No audience | expert peer |
| No format | structure fit to task |
| No done-when | correctness + edge cases |
| fix/review | preserve vs change + verify |
| code/repo | paths/stack **only if known** |
| short + "this" | user attaches artifact; say how to use it |

## Template (omit empty; collapse if simple)

```markdown
## Role
## Context
## Objective
## Inputs
## Requirements
## Output
## Quality bar
```

Simple task → tight bullets or one paragraph; no forced headings.

## Output rules

- Prompt inside **one** code fence; no meta inside fence.
- Max 1 variant only if user benefits; default single prompt.
- Polish don't bloat if input already good.
- Match user domain; specific > verbose.

## Avoid

Invented deadlines/names/policies. Prompt buried in prose. 500-word mega-prompt for 5-line jobs. Many questions when safe defaults work.

More pairs: [examples.md](examples.md).
