## Claude subagent model selection

(Using claude).

When spawning subagents via the Agent tool, always pass the `model` parameter explicitly:

- Default to `sonnet` for all subagent work. Never use `opus` for subagents.
- Use `haiku` only for very easy, mechanical tasks (simple lookups, trivial file reads, straightforward string checks).

This applies to every `Agent` tool call regardless of the `subagent_type`, and overrides the agent definition's default model.

## Automated checks

These rules apply to every AI agent working in this environment.

- Run automated checks sequentially. Wait for each command to finish before starting the next. Never overlap checks through parallel tool calls, background processes, task runners, or delegated agents.
- Run file-based checks, including unit, integration, and end-to-end tests, one file per command. Pass the exact file path. Never combine multiple files or run an entire directory, suite, or glob that selects multiple files.
- Run type checking one application at a time, or one library at a time when the library has its own typecheck target. Never combine multiple projects in one invocation.
- Run other project-wide checks that cannot target a single file one project at a time.
- Select only the checks relevant to the change. If a command cannot respect these limits, explain the limitation before running it and ask the user how to proceed.

## Git worktrees

Whenever you are request to implement a feature in a specific branch, the workflow is:

- Create a git worktree from that branch. If the branch is already on another worktree, stop and tell the user. 
- Implement and make all the commits you are planned to do.
- Delete worktree. 