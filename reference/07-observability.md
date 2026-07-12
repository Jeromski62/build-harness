# Module 7: Observability

## Core Idea

There's now a complete harness: roles, skills, tools, hooks, evals, pipeline.

But how do you tell what's actually happening? How much does a session cost? Why did a role make this decision? When did the harness start getting worse?

That's observability: the ability to look inside the running harness.

Without observability, you're working blind. Outputs are visible, but not why they turned out that way. Problems only surface once the damage is already done.

## The Three Layers

**1. Logs: what happened?**
Every action, every decision, every hook call. The complete history of a session.

**2. Traces: why did it happen?**
A role's reasoning path. Which tools did it call, in what order, why?

**3. Metrics: how well is it running?**
Token costs, latency, reviewer PASS/FAIL rate, hook trigger frequency.

## What to Actually Watch

### Token cost per session
Every interaction costs tokens. A session with many roles and several review rounds costs more than one with few. Knowing what a typical session costs makes it easy to spot when something's gone off the rails, e.g. because a role is stuck in a loop.

### Reviewer PASS/FAIL rate
Frequent FAILs have two possible causes: the roles are making more mistakes (adjust skill files) or the reviewer is too strict (clarify rules in `AGENTS.md`). Without this number, it's unclear which of the two problems is actually happening.

### Hook trigger frequency
If a hook fires every day, there's a systemic problem that needs fixing in the skill file or `AGENTS.md`, not primarily a hook problem, a context problem.

### Drift
Agents can behave differently over time: after model updates, after changes to skill files, after long pauses. Drift is hard to spot without a comparison point. The eval set from Module 6 is that comparison point; observability shows when it's worth running.

## What Observability Looks Like in Practice

No complex monitoring system needed. To start, this is enough:

### Session log
A short log file after every session:

```json
// harness/observability/logs/2026-07-12.json
{
  "date": "2026-07-12",
  "pipeline": "<pipeline-name>",
  "task": "<short description of the task>",
  "agents": [
    { "agent": "<role>", "result": "pass", "tokens_used": 1240 },
    { "agent": "reviewer", "result": "pass", "tokens_used": 380 }
  ],
  "hooks": [
    { "hook": "<hook-name>", "triggered": 1, "blocked": 0 }
  ],
  "total_tokens": 1620,
  "duration_minutes": 8
}
```

### Metrics table
A simple markdown file, updated after every session:

```markdown
## Harness Metrics

| Date | Pipeline | Task | Tokens | Reviewer FAILs | Duration |
|---|---|---|---|---|---|
```

### Drift check
Regularly (e.g. weekly): run 3-4 evals from the eval set, compare with the latest results. More FAILs than before means it's time to adjust the harness.

## What Observability Gives You Over Time

After enough sessions, patterns become visible:
- What a typical session costs
- Which pipeline is the most expensive
- Where the reviewer gives FAIL most often
- Which hooks trigger most often

These numbers are signals for where the harness still isn't configured well enough.

## Observability vs. Hooks vs. Evals

| | Hooks | Evals | Observability |
|---|---|---|---|
| When | Real-time, during a session | Manual, after changes | After every session |
| What | A single rule | Overall behavior | Trends over time |
| Reacts to | Mistakes in the moment | Regressions | Drift and cost |
| Output | PASS/FAIL | PASS/FAIL list | Metrics and logs |

All three together give a complete picture.

## Questions For You

- Realistically, how often will sessions happen: daily, weekly?
- Who looks at these metrics later: just the person themselves, or a team?
- What's a realistic rhythm for a drift check (weekly, monthly)?

## What Gets Built

- `harness/observability/logs/.gitkeep`: folder for session logs
- `harness/observability/session-log-template.json`: template for session logs
- `harness/observability/metrics.md`: metrics table (empty, ready to fill in)
- `harness/observability/drift-check.md`: instructions for the regular drift check

## Notes For Running This

- This structure only becomes useful once real sessions have run. The goal here is to put it in place, not to fill it with data immediately.
- Once 5+ real session logs exist, Module 8 (Observability Dashboard, bonus) becomes relevant. See `08-dashboard-bonus.md`.
