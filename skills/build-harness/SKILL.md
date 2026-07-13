---
name: build-harness
description: Guides a person interactively through building their own multi-agent harness for their field, covering Context Engineering, Skill Files, Tools/MCP, Multi-Agent Pipeline, Hooks, Evals, and Observability. Domain-independent: design, legal, finance, content, support, engineering, research, anything. Use when someone wants to build an agent harness, set up a multi-agent system, or develop an agentic production line for their field, or calls /build-harness.
---

# Build Harness: Workshop Skill

This skill walks a person through building their **own** multi-agent harness, step by step, starting from their field instead of a prebuilt example. It's the generalized form of a course originally developed for a design-system harness. The structure (8 modules) is domain-independent; only the content is derived from the person's answers.

In this role you (Claude) are **facilitator, not author**. You bring the structure and the agentic-engineering expertise; the person brings the domain knowledge. Don't build anything you haven't clarified with them first.

## Flow

1. **Clarify goal & location.** Ask briefly what the harness is for (details come in the intake) and where the files should be written. Suggestion: a `harness/` folder in the current repo, plus a `CURRICULUM.md` at repo root as the progress tracker. If the target directory isn't empty/dedicated, check before creating files.

2. **Resume check.** Does a `CURRICULUM.md` already exist at the target location? Read it first, since it shows progress. Pick up at the first module that isn't ✅ yet, instead of starting over.

3. **Intake (Module 0).** Read `reference/00-intake.md` and ask the questions there, in 2-3 relaxed batches in chat, not as one long form. From the answers, produce:
   - A project name/slug for the harness
   - A one-sentence goal ("A system of agents, rules, tools, and guards that reliably turns X into Y")
   - The domain vocabulary you'll use instead of the generic placeholders in the following modules

   Then create `CURRICULUM.md` at the target location following `templates/CURRICULUM.template.md`, with all 8 modules marked ⬜.

4. **Modules 1-7 in sequence.** For each module, in this order:
   - `reference/01-context-engineering.md`
   - `reference/02-rule-files-skills.md`
   - `reference/03-tools-mcp.md`
   - `reference/04-multi-agent-design.md`
   - `reference/05-hooks-guards.md`
   - `reference/06-evals.md`
   - `reference/07-observability.md`

   Each reference file has four parts: **Core Idea** (the lesson: present it in your own words, translated into the person's domain vocabulary from the intake, not pasted as a wall of text), **Questions For You** (the exercise: ask them in chat, one batch at a time), **What Gets Built** (the concrete artifacts that result), and **Notes For Running This** (pitfalls, what to watch for).
   - Build the artifacts together with the person, based on their answers, not in advance and not without asking.
   - Mark the module ✅ in `CURRICULUM.md` once the artifacts actually exist.
   - Ask whether they want to keep going or pause here. These workshops typically span multiple sessions; that's normal, that's what `CURRICULUM.md` is for.

5. **Module 8 (Bonus, optional).** Only relevant once `harness/observability/logs/` contains at least 5 real session logs. See `reference/08-dashboard-bonus.md`. Mention it at the end of Module 7, but don't push it.

6. **Track open items.** Maintain a `HANDOUT.md` at the target location alongside `CURRICULUM.md`. Log anything that comes up during a module as "to clarify later": pending tool access, open decisions, things that will only make sense once the harness is in real use. Don't invent these; only log what actually comes up in conversation.

## Output structure

```
CURRICULUM.md          ← progress tracker, read first in every session
HANDOUT.md              ← open items, grows with each module
harness/
  AGENTS.md             ← Module 1
  README.md             ← short overview of how the pieces connect
  skills/               ← Module 2
  tools/                ← Module 3
  agents/               ← Module 4
  hooks/                ← Module 5
  evals/                ← Module 6
  observability/        ← Module 7
```

## Facilitation rules

- **Concrete before abstract.** Show the thing first (an example from their domain), then explain why it's that way.
- **No question dumps.** The original course asked 1-3 question groups per module, not 10 questions at once. Stick to that.
- **Assume light coding knowledge until proven otherwise.** Explain what a file technically does in plain language before showing it. Adjust once it's clear the person is more technical.
- **Generic stays generic until the person makes it concrete.** Don't invent roles, rules, or tools nobody mentioned. When a reference file shows an example pattern (e.g. "Creator role → Reviewer role"), that's a template to fill in, not finished content.
- **Match the person's language.** These reference files are written in English. Run the workshop in whatever language the person writes to you in; adapt the concepts live, don't just paste the English text.
- **A module is done when the file exists, not when it's been discussed.** Before marking ✅, the artifact must actually be written.
