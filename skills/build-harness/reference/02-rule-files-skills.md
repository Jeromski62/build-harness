# Module 2: Rule Files & Skills

## Core Idea

There's now an `AGENTS.md`, the house rules for all agents. But the work in this field usually consists of several distinct tasks that require different kinds of thinking.

If a single agent is responsible for everything, this happens:
- It loses focus
- Its instructions become huge and contradictory
- It does mediocre work on everything instead of excellent work on one thing

The solution: **specialization through skill files.**

## What a Skill File Is

A skill file is a markdown file that tells an agent:
- Who it is (role, perspective)
- What its one job is
- How it approaches that job (step by step)
- What it outputs (format, structure)
- What it explicitly does NOT do (this matters just as much)

Think of it as a **precise assignment for a role**, not a manual.

## The Analogy: Roles That Already Exist in the Field

Almost every field already has an informal division of roles, even without AI. That's the best template to draw from:

| Role Pattern | Focus | Doesn't Do |
|---|---|---|
| Creator role(s) | Produces/decides on the core artifact | Doesn't review itself |
| Composer role (if applicable) | Builds something larger out of core artifacts | Doesn't invent new core artifacts |
| Reviewer role | Checks against the rules from `AGENTS.md` | Doesn't build anything itself |

This table becomes concrete from the roles the person named in the intake. Don't invent a role nobody mentioned.

This is exactly how specialized agents work: each one knows its area and respects the boundaries of the others.

## Anatomy of a Skill File

```markdown
# [Role]: Skill

## Identity
Who you are, in one sentence.

## Your One Job
What you do, precise, not vague.

## Input
What you get to work with.

## Process
Step by step, how you proceed.

## Output
What you produce: format, structure, language.

## Boundaries
What you explicitly do NOT do.
```

## Important: Skill Files Don't Reload AGENTS.md

`AGENTS.md` is already loaded by the time a skill file becomes active. The skill file doesn't need to repeat the house rules; it builds on them.

Bad:
```
# [Role]
[A rule that's already in AGENTS.md]  ← noise, duplicated
```

Good:
```
# [Role]
You operate within the rules from AGENTS.md.
Your focus: [specific task]
```

## Questions For You

- What recurring roles/work steps exist in this field? (From the intake; concretize here, usually 2-4 roles plus one reviewer)
- For each role: what's the concrete input it receives? What does it produce as output? Where does its responsibility end?
- Does this actually need a separate reviewer role, or does each role check its own work well enough in this field? (A separate reviewer usually pays off, but not always; ask)

## What Gets Built

One skill file per identified role, under `harness/skills/`, e.g. `harness/skills/<role-name>.md`. Most harnesses end up with at least two creator roles plus a reviewer role, but the exact number follows from the intake, not from this template.

## Notes For Running This

- Overlap check at the end: are there areas where two roles would overlap? Overlap is a signal that the boundaries aren't sharp enough yet. Sharpen them before calling the module done.
- If the person names only one role ("I actually do all of this myself"), it's still worth asking: if you had to delegate this to three different people, how would you split it up? That often surfaces hidden roles.
