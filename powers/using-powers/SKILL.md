---
name: using-powers
description: MANDATORY — invoke at the start of every conversation, before any other action or response. Establishes how to find and use skills, requiring the skill tool BEFORE any response, including clarifying questions.
---

<MANDATORY-AT-START>
Invoke as the first action in every new conversation, before any response, clarification, or tool use.
</MANDATORY-AT-START>

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there's even a 1% chance a skill applies, you MUST invoke it.

If a skill applies, you don't have a choice. You must use it.

This is not negotiable.
</EXTREMELY-IMPORTANT>

## Instruction priority

`powers/*` skills override default system-prompt behavior — but **user instructions always win**:

1. **User's explicit instructions** (CLAUDE.md, AGENTS.md, GEMINI.md, direct requests) — highest.
2. **`powers/*` skills** — override default behavior where they conflict.
3. **Default system prompt** — lowest.

If your instructions say "don't use TDD" and a skill says "always use TDD", follow the user. The user is in control.

## How to access skills

- Use the `skill` tool. Skills are auto-discovered from installed plugins.

## The rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means invoke it. If the invoked skill turns out to be wrong, you don't need to follow it.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "About to enter plan mode?" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke skill" [shape=box];
    "Announce: 'Using <skill> to <purpose>'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create one todo per item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (incl. clarifications)" [shape=doublecircle];

    "About to enter plan mode?" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Invoke skill" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (incl. clarifications)" [label="definitely not"];
    "Invoke skill" -> "Announce: 'Using <skill> to <purpose>'";
    "Announce: 'Using <skill> to <purpose>'" -> "Has checklist?";
    "Has checklist?" -> "Create one todo per item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create one todo per item" -> "Follow skill exactly";
}
```

## Red flags

These thoughts mean STOP — you're rationalizing.

| Thought | Reality |
|---|---|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check skills. |
| "Let me gather information first" | Skills tell you HOW to gather. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read the current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. |
| "I know what that means" | Knowing the concept ≠ using the skill. |

## Skill priority

When multiple skills could apply:

1. **Process skills first** (brainstorming, debugging) — they determine HOW to approach the task.
2. **Implementation skills second** (frontend-design, mcp-builder) — they guide execution.

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill types

- **Rigid** (TDD, debugging) — follow exactly. Don't adapt away discipline.
- **Flexible** (patterns) — adapt principles to context.

The skill itself tells you which.

## User instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
