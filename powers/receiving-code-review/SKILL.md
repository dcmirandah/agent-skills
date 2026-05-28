---
name: receiving-code-review
description: Use when receiving code review feedback, before implementing suggestions — especially if any item is unclear or technically questionable. Requires verification, not performative agreement.
---

# Receiving Code Review

Code review is a technical evaluation, not an emotional performance.

**Core principle:** verify before implementing, ask before assuming, technical correctness over social comfort.

## Response pattern

When receiving feedback:

1. **READ** the full feedback without reacting.
2. **UNDERSTAND** — restate each requirement in your own words (or ask).
3. **VERIFY** against the codebase reality.
4. **EVALUATE** — technically sound for *this* codebase?
5. **RESPOND** with a technical acknowledgment or reasoned pushback.
6. **IMPLEMENT** one item at a time, test each.

## Forbidden responses

**Never:**

- "You're absolutely right!" — explicit CLAUDE.md violation.
- "Great point!" / "Excellent feedback!" — performative.
- "Let me implement that now" — before verification.
- Any "Thanks for…" — actions speak.

**Instead:**

- Restate the technical requirement.
- Ask clarifying questions.
- Push back with reasoning when wrong.
- Or just start working — actions over words.

## Unclear feedback

If any item is unclear, **stop**. Don't implement anything yet. Ask for clarification on the unclear items.

Items may be related — partial understanding produces wrong implementations.

```text
Reviewer: "Fix 1-6"
You understand 1, 2, 3, 6. Unclear on 4, 5.

❌ Implement 1, 2, 3, 6 now, ask about 4, 5 later.
✅ "Understand 1, 2, 3, 6. Need clarification on 4 and 5 before implementing."
```

## Source-specific handling

### From your human partner

- Trusted — implement after understanding.
- Still ask if scope is unclear.
- No performative agreement; skip to action or technical acknowledgment.

### From external reviewers (PR reviewers, bots)

Before implementing, check:

1. Technically correct for *this* codebase?
2. Does it break existing functionality?
3. Is there a reason for the current implementation?
4. Does it work across platforms / versions / module variants?
5. Does the reviewer have full context?

If the suggestion is wrong → push back with reasoning. If you can't verify → say so: *"I can't verify this without [X]. Should I investigate / ask / proceed?"* If it conflicts with prior decisions by your human partner → discuss before acting.

**Rule of thumb:** "External feedback — be skeptical, but check carefully."

## YAGNI check for "professional" features

If the reviewer suggests "implement properly":

```text
grep / search the codebase for actual usage.

Unused: "This endpoint isn't called. Remove it (YAGNI)?"
Used: implement properly.
```

> "If we don't need this feature, don't add it."

## Implementation order

For multi-item feedback:

1. Clarify anything unclear FIRST.
2. Implement in this order:
   - Blocking issues (breaks, security).
   - Simple fixes (typos, imports, fmt).
   - Complex fixes (refactor, logic, IaC redesign).
3. Test each fix individually.
4. Verify no regressions.

## When to push back

Push back when:

- The suggestion breaks existing functionality.
- The reviewer lacks full context.
- It violates YAGNI.
- It's technically incorrect for this stack (e.g., framework lifecycle rules, CI task/schema version pins).
- Legacy / compatibility reasons exist.
- It conflicts with architectural decisions.

**How:** technical reasoning, not defensiveness. Ask specific questions. Reference working tests / code. Escalate if architectural.

If you're not comfortable pushing back out loud, signal: *"Strange things are afoot at the Circle K"*.

## Acknowledging correct feedback

```text
✅ "Fixed. <brief description of change>."
✅ "Good catch — <issue>. Fixed in <location>."
✅ Just fix it and let the diff speak.

❌ "You're absolutely right!"
❌ "Great point!"
❌ "Thanks for catching that!"
❌ Any gratitude expression.
```

If you catch yourself about to write "Thanks": delete it. State the fix instead.

## Gracefully correcting a wrong pushback

```text
✅ "You were right — checked X, it does Y. Implementing."
✅ "Verified, you're correct. My initial read was wrong because <reason>. Fixing."

❌ Long apology.
❌ Defending why you pushed back.
❌ Over-explaining.
```

State factually, move on.

## Common mistakes

| Mistake | Fix |
|---|---|
| Performative agreement | Restate requirement or just act. |
| Blind implementation | Verify against codebase first. |
| Batch without testing | One item at a time, test each. |
| Assuming reviewer is right | Check if it breaks things. |
| Avoiding pushback | Technical correctness > comfort. |
| Partial implementation | Clarify all items first. |
| Can't verify, proceed anyway | State the limitation, ask. |

## GitHub thread replies

When replying to inline review comments on GitHub, reply in the comment thread (`gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`), not as a top-level PR comment.

## Bottom line

External feedback = suggestions to evaluate, not orders to follow.

Verify. Question. Then implement. No performative agreement. Technical rigor always.
