# Module 4: Multi-Agent Design

## Core Idea

There are now several specialized roles. Each knows what it does and what it doesn't do.

But who starts? Who hands what off to whom? What happens when a role is done: does the next one start automatically, or does someone have to kick it off?

That's Multi-Agent Design: **the order, the handoffs, and the rules that determine who's up next.**

## The Factory Analogy

Picture an assembly line:

```
Raw material → Station 1 → Station 2 → Quality control → Finished product
```

Each station has one job. No station starts before the previous one is done. Quality control usually happens at the end of a station, not somewhere in the middle where it can get forgotten.

The harness works the same way. The question is: what does this assembly line look like for this field?

## The Two Basic Patterns

**1. Sequence: one after another**
```
Role A → Role B → Role C
```
Each role waits for the output of the previous one. Simple, predictable, easy to debug. Downside: slow if tasks are actually independent of each other.

**2. Parallel: several at once**
```
Role A ──┐
         ├── Reviewer
Role B ──┘
```
Several roles work at the same time, one role collects and checks. Faster, but more complex; handoffs need to be precisely defined.

**For a start:** sequence is almost always the right entry point. Parallel comes once the rhythm is known.

## Deriving Your Own Assembly Line

From the roles in Module 2, a pattern like this usually emerges:

```
1. Creator role 1
      ↓
2. Reviewer          ← first quality check
      ↓ (only if PASS)
3. Creator role 2 (if applicable)
      ↓
4. Reviewer          ← second quality check
      ↓ (only if PASS)
5. Documentation/output role (if applicable)
      ↓
6. Context/memory role   ← usually last, keeps the persistent context up to date
```

The reviewer often shows up more than once, once after each creator role. That's not a mistake, it's intentional: catching errors early is cheaper than catching them late.

This is a pattern, not a fixed rule. The real order emerges from the conversation with the person, not from this template.

## Handoffs: the Handover Between Roles

A handoff is the moment a role is done and the next one starts.

A good handoff defines:
- **What gets handed over**: which files/data, what format
- **What the next role does first**, so it doesn't have to guess
- **What happens if something's missing**: the role asks, it doesn't guess

Bad:
```
Role A: "Here's the result."
Role B: [guesses what's meant]
```

Good:
```
Role A output:
- Changed/new artifacts: [concrete list]
- Reviewer gave PASS on: [date]
→ Role B can start
```

## What If a Role Gets FAIL?

```
Reviewer: FAIL
  ❌ [concrete reason]

The creator role gets the feedback:
  → Corrects
  → Sends back to reviewer

Reviewer: PASS
  → Next role can start
```

That's a **feedback loop**: the harness corrects itself without constant intervention. That's the core of agentic engineering.

## The Orchestrator Prompt

For all of this to work, someone needs to start the line. That's the person themselves, in the form of an **orchestrator prompt**.

The orchestrator prompt is simply the instruction that kicks off an entire assembly line, not a separate agent:

```
"New task: [concrete description].
Start the assembly line: [Role A] → Reviewer → [Role B] → Reviewer → [Role C]."
```

The harness already knows what each step means, via the skill files and `AGENTS.md`. The person just gives the starting signal.

## Questions For You

- Does the pattern above hold for this field, or does the order look different?
- Are there special cases? (Just one partial step without the others, exception workflows …)
- What happens on FAIL: how many correction attempts before a human steps in?
- What exactly gets passed on at each handoff?

## What Gets Built

- `harness/agents/pipeline.md`: the full assembly line with handoffs
- `harness/agents/orchestrator-prompt.md`: the starting-signal prompt for future use

## Notes For Running This

- Start with sequence, even if parallel feels tempting. Debuggability matters more than speed at the start.
- If the reviewer only shows up once, right at the end, ask whether an earlier check wouldn't pay off. It usually does.
