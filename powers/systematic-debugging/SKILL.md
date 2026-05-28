---
name: systematic-debugging
description: Use when encountering any bug, test failure, CI/build failure, IaC error, or unexpected behavior — before proposing fixes.
---

# Systematic Debugging

Random fixes waste time and create new bugs. Quick patches mask underlying issues.

**Core principle:** ALWAYS find the root cause before attempting a fix. Symptom fixes are failure.

**Violating the letter of this process is violating the spirit of debugging.**

## The Iron Law

```text
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you cannot propose fixes.

## When to use

For any technical issue:

- Test failures.
- Production bugs.
- Unexpected behavior.
- Performance problems.
- Build failures.
- Unexpected IaC plan/apply diffs or policy failures.
- CI failures (task errors, agent issues, auth failures).
- Markdown rendering / lint failures you can't explain.

**Especially when:**

- Under time pressure (emergencies tempt guessing).
- "Just one quick fix" seems obvious.
- You've already tried multiple fixes.
- A previous fix didn't work.
- You don't fully understand the issue.

**Don't skip when:**

- Issue seems simple (simple bugs have root causes too).
- You're in a hurry (rushing guarantees rework).
- Manager wants it fixed NOW (systematic is faster than thrashing).

## The four phases

You MUST complete each phase before the next.

### Phase 1 — Root cause investigation

1. **Read errors carefully.**
   - Don't skip past warnings. They often contain the answer.
   - Read full stack traces / CI logs / IaC plan output.
   - Note line numbers, file paths, error codes, resource addresses.

2. **Reproduce consistently.**
   - Can you trigger it reliably?
   - Exact steps?
   - Every time, or sometimes?
   - Not reproducible → gather more data, don't guess.

3. **Check recent changes.**
   - `git diff`, recent commits, recent module / provider version bumps, recent pipeline template changes, env var / secret rotations.

4. **Gather evidence in multi-component systems.**

   For systems with multiple layers (CI → build → deploy, API → service → DB, pipeline → runtime → cloud), add diagnostic instrumentation **before** proposing fixes:

   ```text
   For EACH component boundary:
     - Log what data enters
     - Log what data exits
     - Verify env / config propagation
     - Check state at each layer

   Run once → analyze → identify failing component → investigate that component.
   ```

   Example (multi-layer CI → deploy):

   ```bash
   # Layer 1: secret available in CI?
   echo "API_KEY: ${API_KEY:+SET}${API_KEY:-UNSET}"

   # Layer 2: secret in build/runtime env?
   env | grep -E '^API_KEY=' || echo "API_KEY not in environment"

   # Layer 3: app reads it?
   node -e "console.log('key len', (process.env.API_KEY||'').length)"

   # Layer 4: external call succeeds?
   curl -sf -H "Authorization: Bearer $API_KEY" https://api.example/health
   ```

   Reveals which layer fails (CI secret ✓, runtime env ✗, etc.).

5. **Trace data flow.** When the error is deep in a call stack:

   See `root-cause-tracing.md` in this directory.

   Quick version: where does the bad value originate? What called this with the bad value? Trace upward until you find the source. Fix at source, not at symptom.

### Phase 2 — Pattern analysis

1. **Find working examples.** Locate similar working code in the same codebase (or similar working module / pipeline / template).
2. **Compare against references.** Read reference implementations completely — every line. Don't skim.
3. **Identify differences.** List every difference, however small. Don't dismiss "that can't matter."
4. **Understand dependencies.** What other components, providers, settings, env vars, agents, identities does this need?

### Phase 3 — Hypothesis and testing

1. **Form a single hypothesis.** Write it down. *"X is the root cause because Y."*
2. **Test minimally.** The smallest possible change. One variable at a time. Don't fix multiple things at once.
3. **Verify before continuing.** Worked? → Phase 4. Didn't? → form a NEW hypothesis. Don't pile fixes.
4. **When you don't know:** say so. Don't pretend. Ask. Research more.

### Phase 4 — Implementation

1. **Create a failing test case.**
   - Simplest possible reproduction.
   - Automated when possible (unit test, IaC test, CI preview/dry-run, etc.). One-off script if no framework.
   - Required before fixing. Use `powers:test-driven-development`.

2. **Implement a single fix.**
   - Address the root cause.
   - One change at a time.
   - No "while I'm here" improvements. No bundled refactoring.

3. **Verify.**
   - Test passes?
   - No other tests broken?
   - Issue actually resolved?

4. **If the fix doesn't work:**
   - STOP.
   - Count fix attempts.
   - <3 → return to Phase 1 with new information.
   - **≥3 → STOP and question the architecture (step 5).**
   - DON'T attempt fix #4 without architectural discussion.

5. **If 3+ fixes failed: question architecture.**

   Patterns indicating architectural problem:

   - Each fix reveals new shared state / coupling / problem in a different place.
   - Fixes require "massive refactoring" to apply.
   - Each fix creates new symptoms elsewhere.

   STOP and question fundamentals:

   - Is this pattern fundamentally sound?
   - Are we sticking with it through inertia?
   - Should we refactor architecture vs. continue fixing symptoms?

   Discuss with the human partner before more fixes. This is NOT a failed hypothesis — this is a wrong architecture.

## Red flags — STOP and follow process

If you catch yourself thinking:

- "Quick fix for now, investigate later."
- "Just try changing X and see if it works."
- "Add multiple changes, run tests."
- "Skip the test, I'll manually verify."
- "It's probably X, let me fix that."
- "I don't fully understand but this might work."
- "Pattern says X but I'll adapt it differently."
- "Here are the main problems: [lists fixes without investigation]."
- Proposing solutions before tracing data flow.
- **"One more fix attempt" (already tried 2+).**
- **Each fix reveals a new problem in a different place.**

All of these mean: STOP. Return to Phase 1.

If 3+ fixes failed: question the architecture (Phase 4.5).

## Human-partner signals you're doing it wrong

- "Is that not happening?" — you assumed without verifying.
- "Will it show us…?" — you should have added evidence gathering.
- "Stop guessing" — proposing fixes without understanding.
- "Ultrathink this" — question fundamentals, not symptoms.
- "We're stuck?" (frustrated) — your approach isn't working.

When you see these: STOP. Return to Phase 1.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "Issue is simple, no process needed." | Simple issues have root causes too. The process is fast for simple bugs. |
| "Emergency, no time." | Systematic debugging is FASTER than thrashing. |
| "Just try this first, then investigate." | The first fix sets the pattern. Do it right. |
| "Tests after confirming the fix works." | Untested fixes don't stick. Test first proves it. |
| "Multiple fixes at once saves time." | Can't isolate what worked. Causes new bugs. |
| "Reference too long, I'll adapt the pattern." | Partial understanding guarantees bugs. Read it completely. |
| "I see the problem, let me fix it." | Seeing symptoms ≠ understanding root cause. |
| "One more fix attempt" (after 2+ failures). | 3+ failures = architectural problem. Stop fixing. |

## Quick reference

| Phase | Key activities | Success criteria |
|---|---|---|
| 1. Root cause | Read errors, reproduce, check changes, gather evidence | Understand WHAT and WHY |
| 2. Pattern | Find working examples, compare | Identify differences |
| 3. Hypothesis | Form theory, test minimally | Confirmed or new hypothesis |
| 4. Implementation | Create test, fix, verify | Bug resolved, tests pass |

## When the process reveals "no root cause"

If systematic investigation shows the issue is truly environmental, timing-dependent, or external:

1. The process is complete.
2. Document what you investigated.
3. Implement appropriate handling (retry, timeout, error message).
4. Add monitoring / logging for future investigation.

But: 95% of "no root cause" cases are incomplete investigation.

## Supporting techniques (in this directory)

- `root-cause-tracing.md` — trace bugs backward through the call stack.
- `defense-in-depth.md` — add validation at multiple layers AFTER finding the root cause.
- `condition-based-waiting.md` — replace arbitrary timeouts with condition polling.

## Related skills

- `powers:test-driven-development` — for the failing test in Phase 4.
- `powers:verification-before-completion` — before claiming the fix worked.
