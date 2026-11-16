## Claude subagent model selection

(Using claude).

When spawning subagents via the Agent tool, always pass the `model` parameter explicitly:

- Default to `sonnet` for all subagent work. Never use `opus` for subagents.
- Use `haiku` only for very easy, mechanical tasks (simple lookups, trivial file reads, straightforward string checks).

This applies to every `Agent` tool call regardless of the `subagent_type`, and overrides the agent definition's default model.

## Automated checks

These rules apply to every AI agent working in this environment.

- Within each Git worktree, run automated checks sequentially. Wait for each command to finish before starting the next. Never overlap checks within that worktree through parallel tool calls, background processes, task runners, or delegated agents. Checks in two separate worktrees may run concurrently under the local resource rules below.
- Run file-based checks, including unit, integration, and end-to-end tests, one file per command. Pass the exact file path. Never combine multiple files or run an entire directory, suite, or glob that selects multiple files.
- Run Jest with `--runInBand`, including when invoked through a project task runner. Do not combine it with `--maxWorkers`.
- Run type checking one application at a time, or one library at a time when the library has its own typecheck target. Never combine multiple projects in one invocation.
- Run other project-wide checks that cannot target a single file one project at a time.
- Select only the checks relevant to the change. If a command cannot respect these limits, explain the limitation before running it and ask the user how to proceed.

## Local resource usage

These rules apply to Codex and Claude, including their delegated agents, when working on this computer.

- Support two concurrent Git worktree workflows. Within each worktree, run only one resource-heavy operation at a time across the agent's work and delegated work. Tests, type checking, builds, indexing, and browser automation must not overlap within that worktree. Do not start a third resource-heavy workflow unless the user explicitly requests it. Among services started by the agent, keep only those needed for the current operation running.
- Reuse existing development servers and containers only after checking that they serve the intended worktree and configuration. Start only missing dependencies. Do not stop or restart services started by the user or another session without authorization.
- Assign distinct application ports, browser session names and profiles, and writable runtime state to each active worktree. Isolate databases, queues, uploads, and generated outputs wherever concurrent runs could interfere. Shared infrastructure is acceptable only when its data and consumers are isolated; do not reuse another worktree's mutable state by default.
- Use one browser automation session per worktree at a time and reuse it across steps. Close tabs when no longer needed, and close agent-created browser sessions when the task finishes or fails. Preserve user-owned browser sessions and sessions belonging to other worktrees.
- Track processes, browser sessions, and containers started for the task. Clean up those no longer needed, including after failures, unless the user requested that they remain running. Target the specific process or session; do not use broad process-name kills.
- Use checks that exit after one run. Do not start watch modes, background checks, or additional development servers unless the task requires them.
- Preserve build and dependency caches for reuse. Do not clear caches, reinstall dependencies, or rebuild the whole workspace as a routine troubleshooting step; require evidence that the targeted cache or installation is involved.
- After an out-of-memory error, exit code 137, browser crash, or resource-related timeout, inspect available memory, swap, and relevant processes before retrying. Distinguish a Node heap limit from system memory exhaustion. Reduce unnecessary resource usage before increasing limits, and do not repeat the same failing command without a reason to expect a different result.
- Apply any necessary Node heap override to the specific command. Do not set a global `NODE_OPTIONS` memory increase or change WSL, swap, or system resource limits without user authorization. Leave memory available for Windows, the editor, and required services.

## Git worktrees

Whenever you are request to implement a feature in a specific branch, the workflow is:

- Create a git worktree from that branch. If the branch is already on another worktree, stop and tell the user. 
- Implement and make all the commits you are planned to do.
- Delete worktree.
