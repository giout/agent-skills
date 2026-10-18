## Claude subagent model selection

(Using claude).

When spawning subagents via the Agent tool, always pass the `model` parameter explicitly:

- Default to `sonnet` for all subagent work. Never use `opus` for subagents.
- Use `haiku` only for very easy, mechanical tasks (simple lookups, trivial file reads, straightforward string checks).

This applies to every `Agent` tool call regardless of the `subagent_type`, and overrides the agent definition's default model.

## Running tests

Never run more than one test file at a time. Pass a single spec file path to the test command (for example `pnpm exec nx test <project> --testFile=<path>`); never run a whole project's suite, a directory, or a glob that matches several files.

## Git worktrees

Whenever you are request to implement a feature in a specific branch, the workflow is:

- Create a git worktree from that branch. If the branch is already on another worktree, stop and tell the user. 
- Implement and make all the commits you are planned to do.
- Delete worktree. 