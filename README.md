# build-harness

An interactive domain-agnostic skill that walks you through designing your own multi-agent harness (context, roles, tools, pipeline, guards, evals, and observability), tailored to your own field through guided questions instead of a fixed template.

Claude asks you about your domain, then co-builds the actual harness artifacts with you, one module at a time, the way you'd run a real workshop rather than hand someone a reference doc to read on their own.

<p align="center">
  <img src="assets/wheel.svg" alt="You at the center, Domain at the top, and clockwise around the wheel: Context, Roles, Tools, Multi-Agent, Hooks, Evals, Observability" width="420">
</p>

## Why

Most "how to build an agent system" guides teach you the concepts in the abstract, or show you one hardcoded example (usually code generation) that doesn't map cleanly onto your own field. This skill flips that: it asks about *your* domain first, including what you produce, what your source of truth is, what roles already exist in your workflow, and what must never go wrong. Only then does it build the harness, using your own vocabulary throughout.

The module structure works for any field: design systems, legal review, financial reporting, content ops, customer support, research pipelines, engineering, anywhere a repeatable, multi-step piece of work currently depends on a person doing it carefully every time.

**Any domain, one runtime.** The field is open, but the result is built for Claude Code. Roles become subagents, the pipeline becomes a skill you start with a slash command, and guards become hooks. Every file lands where Claude Code loads it, so the harness runs as soon as it's built.

## Install

As a Claude Code plugin (recommended):

```
/plugin marketplace add Jeromski62/build-harness
/plugin install build-harness@build-harness
```

Or copy the skill into your project's skills folder directly:

```
your-project/.claude/skills/build-harness/   ← copy contents of skills/build-harness/ here
```

Either way, no dependencies, no build step. It's plain markdown that Claude Code reads directly.

## Use

Start Claude Code in the project the harness is for, then either:
- Say what you want in plain language, e.g. *"I want to build an agent harness for reviewing customer support tickets,"* and Claude will recognize the skill and start the workshop, or
- Invoke it explicitly if your setup supports slash commands: `/build-harness`

The workshop runs across as many sessions as it takes. Progress is tracked in a `CURRICULUM.md` file that Claude reads first each time, so you can pick up exactly where you left off.

## What it produces

```
AGENTS.md               ← Module 1: persistent context & hard rules
CLAUDE.md               ← Module 1: one line, @AGENTS.md
CURRICULUM.md           ← progress tracker, read first in every session
HANDOUT.md              ← open items, grows with each module
.mcp.json               ← Module 3: MCP servers (only if needed)
.claude/
  agents/<role>.md      ← Module 2: one subagent per role, with its own tool list
  skills/<pipeline>/
    SKILL.md            ← Module 4: the pipeline, started with /<pipeline> <task>
  settings.json         ← Module 5 + 7: hook wiring
harness/
  README.md             ← how the pieces connect
  handoffs/             ← Module 4: what each role handed to the next, per task
  hooks/                ← Module 5: deterministic guard scripts
  evals/                ← Module 6: test set, runner script, results
  observability/        ← Module 7: session logs & metrics
```

If your project already has a `CLAUDE.md`, `AGENTS.md`, `.mcp.json`, or `.claude/settings.json`, the workshop adds to it and shows you the result first. It doesn't overwrite.

## What's manual, what's automatic

Once the harness is built, it runs on its own: you start the pipeline with one command, the roles hand off to each other, hooks block rule violations, and every session writes its own log with real token counts.

**Evals** run from one command too. Module 6 gives you a set of 10-15 test cases and a runner script:

```
node harness/evals/run-evals.js
```

It starts the pipeline once per test case, checks everything a script can check (did the reviewer pass it, does the output exist, did a guard step in), and writes a report per case plus a results table with tokens and cost. No copy-pasting inputs into a chat.

What stays with you is judgment. Criteria like "the tone fits" are marked `[manual]` in the eval set, and the report lists them as a tick list next to the pipeline's output. The more criteria you phrase as checks, the shorter that list gets.

Both scripts the harness uses (the eval runner and the session logger) need Node.

## The modules

An intake, seven core modules, and an optional bonus:

| # | Module | Question it answers |
|---|---|---|
| 0 | Intake | What field is this for, and what does the harness need to know? |
| 1 | Context Engineering | What must the agent know on day one? |
| 2 | Roles & Subagents | Who does what, and where does each role's job end? |
| 3 | Tools & MCP | What does each role need to be able to touch? |
| 4 | Multi-Agent Design | What order do things happen in, and how do roles hand off to each other? |
| 5 | Hooks & Guards | What must never happen, and how do you enforce that deterministically? |
| 6 | Evals | How do you know the harness is actually working? |
| 7 | Observability | What's happening right now, and what does it cost? |
| 8 | Dashboard (bonus) | Optional, unlocked after 5+ real logged sessions |

## Structure

```
.claude-plugin/
  plugin.json                     ← plugin manifest
  marketplace.json                ← lets /plugin marketplace add find this repo
skills/build-harness/
  SKILL.md                        ← facilitation logic Claude follows
  reference/
    00-intake.md                  ← domain intake questions
    01-context-engineering.md     ← Modules 1-7: lesson + exercise + notes
    02-roles-subagents.md
    03-tools-mcp.md
    04-multi-agent-design.md
    05-hooks-guards.md
    06-evals.md
    07-observability.md
    08-dashboard-bonus.md
  templates/
    CURRICULUM.template.md        ← progress-tracker skeleton
    run-evals.js                  ← eval runner, copied into your harness in Module 6
    log-event.js                  ← session logger, copied into your harness in Module 7
```

Each `reference/` file has four parts: **Core Idea** (the lesson, in plain language), **Questions For You** (what gets asked in chat), **What Gets Built** (the resulting artifact), and **Notes For Running This** (facilitation guidance for Claude, not shown to the user).

## Upgrading from 1.x

Version numbers here mean the plugin, not the harness you build with it.

Plugin 2.0 changes where the output goes. Plugin 1.x wrote everything into `harness/`, including role files (`harness/skills/`) and the pipeline (`harness/agents/`), where Claude Code didn't load them. If you have a harness built with 1.x, run the workshop again in that project: it recognizes the old `CURRICULUM.md` and offers to redo Modules 1-4 with the new locations. Your hooks, evals, and intake answers carry over.

## Origin

Generalized from a hands-on course originally built to teach one specific team how to construct a design-system harness. The seven core modules (context, roles, tools, multi-agent, hooks, evals, observability) turned out to be domain-independent; only the content needed to change per field. This repo is that structure, parameterized on whoever runs it.

## License

MIT. See [LICENSE](LICENSE).
