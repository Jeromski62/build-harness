# Module 5: Hooks & Guards

## Core Idea

There's now an assembly line with specialized roles, clear handoffs, and a reviewer.

But agents forget sometimes, without meaning to. A step gets skipped. A check gets left out. A follow-up action doesn't get triggered because nobody happened to think of it in that moment.

**Hooks solve that.** They're deterministic code that automatically runs at certain points, regardless of whether a role remembered to do it.

A hook has no intelligence and never forgets. It simply enforces a rule, every time.

## The Analogy: Bouncer vs. Consultant

A role (agent) is like a consultant: it thinks along, gives recommendations, makes decisions.

A hook is like a bouncer: it checks a single rule and either lets you through or not. No discussion, no exceptions.

```
Agent:  "I finished X and I think Y is up to date too."
Hook:   [checks automatically] "Y was not changed. STOP."
```

The hook always wins in that moment, because it has no opinion, only a rule.

## Pre-Hooks vs. Post-Hooks

**Pre-hook:** runs *before* an action happens, preventing something wrong from being executed.
**Post-hook:** runs *after* an action happens, making sure the result is correct and triggering follow-up actions.

```
Pre-hook  →  [role's action]  →  Post-hook
```

## Deriving Hooks From Hard Rules

A hook comes from asking Module 1 (hard rules) and Module 4 (handoffs) one question: **can a role forget this? If yes, hook.**

Six patterns that show up in almost every field. The concrete implementation follows from the intake, not from this list:

**1. Format/rule check (pre)**
Does the output break a hard rule from `AGENTS.md`? (Analogous to: no hardcoded values)

**2. Reference-existence check (pre)**
Does a role reference something that doesn't exist yet? (Analogous to: referenced token doesn't exist)

**3. Breaking-change check (pre)**
Does a role change or delete something others already consume? (Analogous to: public-API change without a migration order)

**4. Auto-sync hook (post)**
Does something else need to be automatically updated after this change? (Analogous to: re-sync the context layer after every change)

**5. Quality-threshold check (post)**
Is there a measurable quality bar that must be checked every single time? (Analogous to: contrast check against WCAG)

**6. Completeness check (post)**
Is the output complete relative to what it's supposed to cover? (Analogous to: missing documentation stories)

Not every field needs all six. Some need only two, plus one that isn't on this list; that's fine.

## How Hooks Work in Claude Code

Claude Code supports hooks via `settings.json`. Hooks are shell commands or scripts that Claude Code runs automatically on certain events.

```json
// .claude/settings.json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          { "type": "command", "command": "node harness/hooks/<check-name>.js" }
        ]
      }
    ]
  }
}
```

So a hook is a script that runs on a specific event. The script itself is plain code, no AI, no agent.

## Hooks vs. Reviewer: the Difference

| | Reviewer (agent) | Hook |
|---|---|---|
| Type | AI agent | Deterministic code |
| Decides on | Quality, context, completeness | A single rule |
| Can forget | Yes (rarely) | No, never |
| Can discuss | Yes | No |
| Costs tokens | Yes | No |
| When | As a step in the pipeline | Automatically on every trigger |

Hooks and reviewer complement each other: the hook catches mechanical mistakes, the reviewer judges quality.

## Questions For You

- What rule must never be broken under any circumstances? (This is almost always the first hook worth building)
- Where has a forgotten step caused the most pain in the past, even without AI?
- Which of the six patterns above fits this field, and which doesn't?

## What Gets Built

- `harness/hooks/hooks.config.json`: which hooks run when
- One script per hook, under `harness/hooks/`, e.g. `harness/hooks/<check-name>.js`
- The wiring into `.claude/settings.json`, so the hooks actually run automatically

## Notes For Running This

- Start with the one hook that covers the most expensive past failure mode, not all six patterns at once.
- A hook that never triggers isn't a problem. A hook that triggers constantly is a signal of a context problem back in Module 1 or 2, not primarily a hook problem.
