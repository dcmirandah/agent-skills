---
name: finishing-a-development-branch
description: Use when implementation is complete, all tests pass, and you need to integrate the work — guides verification then presents merge / PR / keep / discard options.
---

# Finishing a Development Branch

Verify tests → present options → execute choice → clean up.

**Announce:** "I'm using `powers:finishing-a-development-branch` to complete this work."

## Process

### 1. Verify tests

Run the project's verification before offering anything. Pick by stack:

```bash
# IaC (if applicable)
<project iac validate command>

# Markdown (if applicable)
<project doc linter>

# Shell (if applicable)
shellcheck $(git ls-files '*.sh')

# Python (if applicable)
pytest

# Default
<project test command>
```

If anything fails:

```text
Tests failing (<N>). Must fix before completing:
[show failures]
Cannot proceed with merge / PR until tests pass.
```

Stop.

### 2. Determine base branch

```bash
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

Or ask: "This branch split from `main` — correct?"

### 3. Present options

Exactly four:

```text
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

No explanation. Keep it tight.

### 4. Execute choice

#### Option 1 — Merge locally

```bash
git checkout <base-branch>
git pull
git merge <feature-branch>
<test command>          # verify on merged result
git branch -d <feature-branch>
```

Then cleanup worktree (Step 5).

#### Option 2 — Push and PR

```bash
git push -u origin <feature-branch>

gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<2-3 bullets of what changed>

## Test Plan
- [ ] <verification steps>
EOF
)"
```

Then cleanup worktree (Step 5).

#### Option 3 — Keep as-is

Report: `Keeping branch <name>. Worktree preserved at <path>.`

Don't clean up the worktree.

#### Option 4 — Discard

Confirm first:

```text
This will permanently delete:
- Branch <name>
- All commits: <commit-list>
- Worktree at <path>

Type 'discard' to confirm.
```

Wait for the literal word `discard`.

If confirmed:

```bash
git checkout <base-branch>
git branch -D <feature-branch>
```

Then cleanup worktree (Step 5).

### 5. Cleanup worktree

Only for Options 1, 2, 4.

```bash
git worktree list | grep $(git branch --show-current)
git worktree remove <worktree-path>
```

Option 3: keep the worktree.

## Quick reference

| Option | Merge | Push | Keep worktree | Cleanup branch |
|---|---|---|---|---|
| 1. Merge locally | ✓ | - | - | ✓ |
| 2. Create PR | - | ✓ | ✓ | - |
| 3. Keep as-is | - | - | ✓ | - |
| 4. Discard | - | - | - | ✓ (force) |

## Common mistakes

| Mistake | Fix |
|---|---|
| Skipping test verification | Always verify before offering options. |
| Open-ended "what's next?" | Present exactly the 4 options. |
| Auto-cleaning worktree | Only Options 1 and 4 clean up. |
| No confirmation for discard | Require typed `discard`. |

## Red flags

**Never:**

- Proceed with failing tests.
- Merge without re-verifying tests on the merged result.
- Delete work without typed confirmation.
- Force-push without explicit request.

**Always:**

- Verify tests first.
- Present exactly four options.
- Cleanup only for Options 1 and 4.

## Integration

**Called by:**

- `powers:subagent-driven-development` (Step 7) — after all tasks complete.
- `powers:executing-plans` (Step 3) — after all batches complete.
