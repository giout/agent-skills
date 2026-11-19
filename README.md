# Agent Skills

Repository of instructions and skills for software development assistants such as Codex and Claude. It centralizes shared rules and reusable procedures for working on projects.

| File or folder | Purpose |
| --- | --- |
| [`RULES.md`](RULES.md) | Defines the shared rules that assistants must read before each task. Includes instructions for running tests, using Git worktrees, and selecting models for Claude subagents; Claude-specific rules apply only to that assistant. |
| [`skills/`](skills/) | Contains reusable procedures for development tasks. Each skill has its own subfolder with a `SKILL.md` file that defines its name, when to use it, and the instructions the assistant must follow. |

## Third-party skills

- [agent-browser](https://github.com/vercel-labs/agent-browser)
- [codegraph](https://github.com/colbymchenry/codegraph)
- [ponytail](https://github.com/DietrichGebert/ponytail)
- [unslop](https://github.com/theclaymethod/unslop)
- [grill-me](https://www-aihero-dev.translate.goog/skills-grill-me?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc&_x_tr_hist=true)