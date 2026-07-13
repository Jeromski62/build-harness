# {{PROJECT_NAME}}: Harness Progress

## Goal

{{GOAL_SENTENCE}}

---

## Progress

| # | Module | Status | Harness Artifact |
|---|---|---|---|
| 1 | Context Engineering | ⬜ open | `harness/AGENTS.md` |
| 2 | Rule Files & Skills | ⬜ open | `harness/skills/` |
| 3 | Tools & MCP | ⬜ open | `harness/tools/` |
| 4 | Multi-Agent Design | ⬜ open | `harness/agents/` |
| 5 | Hooks & Guards | ⬜ open | `harness/hooks/` |
| 6 | Evals | ⬜ open | `harness/evals/` |
| 7 | Observability | ⬜ open | `harness/observability/` |
| 8 | Observability Dashboard (Bonus) | ⬜ open, only after 5+ real session logs | `index.html` |

**Legend:** ⬜ open · 🔄 in progress · ✅ done

---

## How Sessions Work

Every session:
1. Read this file first: where does progress stand?
2. Continue at the first module that isn't ✅ yet
3. Answer the questions from the matching `reference/0X-*.md` file in the `build-harness` skill, in chat
4. Build the harness artifact together
5. Mark the module as ✅

---

## Reference

This project was set up with the `build-harness` skill, a generalized version of a course on building a multi-agent harness. The lessons live in `.claude/skills/build-harness/reference/`.
