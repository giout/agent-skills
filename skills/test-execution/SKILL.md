---
name: test-execution
description: Execute a test plan against a complete isolated application stack, seed isolated test data, and iterate on fixes until the required tests pass. Use when asked to run tests and correct failures on a branch without moving its checkout or disturbing existing services.
---

# Test Execution

## Inputs

- `[TEST_PLAN]`: the test plan to execute, supplied directly, in the conversation, or through a referenced document. Preserve its test IDs and expected results.
- `[BRANCH]`: the branch to test and correct, in its existing worktree or checkout.

Run the planned tests and fix failures within the requested scope. Keep the branch in its existing checkout: do not create another worktree, move the branch, switch branches, reset changes, or delete the checkout. Isolate the runtime and test data rather than relocating the source code. Leave corrections in that checkout for review; commit only when requested.

## Workflow

1. **Locate and inspect.** Read applicable `AGENTS.md` files and required documents. Locate `[BRANCH]` using the current checkout and `git worktree list`, and record its path, HEAD, and existing uncommitted changes. If it is not checked out anywhere accessible, report that blocker without checking it out elsewhere. Resolve `[TEST_PLAN]` and identify required services, prerequisites, data, execution layers, and expected outcomes. Inspect the repository's startup, migration, seed, and test procedures; use CodeGraph first when indexed. Clarify missing expectations that prevent execution rather than inventing them.
2. **Prepare isolation.** Use the project's existing runtime mechanism with a unique run identifier. Keep temporary environment files and overrides outside tracked configuration. Isolate databases, credentials, container/project names, networks, volumes, queues, caches, uploads, browser profiles, and other writable state used by the tests. Direct external integrations to existing test adapters or sandbox endpoints where needed. Do not point migrations, seeds, resets, or test traffic at another checkout's or a shared mutable environment. Verify the effective configuration before starting or populating services.
3. **Allocate ports.** Inspect occupied ports and choose available host ports for every exposed service, including databases and auxiliary services. Apply temporary overrides to bindings and all dependent URLs, origins, callbacks, proxies, and test clients. If a port becomes occupied during startup, choose another and retry the affected task-owned service with updated references. Never stop or reconfigure the process holding the conflicting port. Keep the overrides for this run only.
4. **Start and populate.** Start the complete application stack required by the plan from the branch's existing checkout, using the isolated configuration. Track task-owned process IDs, containers, networks, and volumes for targeted shutdown. Wait for readiness and confirm the running application serves this checkout's current code and uses the isolated dependencies. Apply migrations and populate the isolated database with reproducible data sufficient for the plan, using existing fixtures, seed commands, factories, or application flows. Record how the data was created; do not copy production data or expose credentials in reports.
5. **Execute an iteration.** Run the planned tests against the isolated endpoints. For browser workflows, use the installed `agent-browser` skill with one isolated session for this checkout. Execute checks sequentially: one exact test file per command, Jest with `--runInBand`, and one application or library per typecheck command. Use one-shot commands and respect repository resource limits. Record expected and actual outcomes by test ID; distinguish passed, failed, blocked, and not-run tests. Capture reproduction steps and the logs, screenshots, or other evidence needed for diagnosis before shutdown.
6. **Shut down the isolated stack.** After each iteration, including test errors, close the task-owned browser session and stop all application services started for this run before analyzing results or editing code. Use the recorded ownership and exact runtime configuration; do not use broad process-name kills or stop other stacks. Preserve diagnostic evidence and the isolated data needed to investigate. A startup or seed failure also requires cleanup of partially started resources before diagnosis and retry.
7. **Analyze and correct.** Compare the recorded results with the plan. Finish successfully only when every required test has passed against the final code and no relevant errors remain unresolved; blocked or skipped tests do not count as success. Otherwise trace failures to their cause and make the smallest justified corrections in `[BRANCH]`'s existing checkout, preserving unrelated user changes. Correct test data, runtime configuration, or test defects when evidence identifies those as the cause. Do not weaken expected outcomes or remove tests merely to obtain a pass. Keep fixes tied to the plan's objective and regression scope.
8. **Repeat.** After corrections, restart the isolated stack with the temporary configuration, rebuild affected components as needed, and migrate or reseed isolated data as required. Reset only task-owned state when tests require a clean baseline. Reproduce the previous failures and rerun the required plan against the final code, including previously passing coverage that could regress. Repeat steps 5–8 until success. Investigate resource failures before retrying, and do not repeat an unchanged failing command without new evidence. If progress requires unavailable credentials, infrastructure, an unresolved product decision, or work outside the authorized scope, clean up and report the concrete blocker and remaining tests instead of claiming completion.
9. **Clean up and report.** On success or a blocking exit, remove temporary port/configuration overrides and disposable runtime resources created for this run. Preserve source corrections, existing user state, dependency/build caches, and useful diagnostic evidence. Report any cleanup failure and the exact remaining task-owned resources. Do not leave the isolated stack running unless the user requests it.

## Deliverable

Return a concise report containing:

- The branch, checkout path, tested revision, and any uncommitted corrections included in the tested code.
- Results by test ID, with expected versus actual behavior for failures, reproduction evidence, and any blocked or unexecuted coverage.
- Corrections made, their purpose, and the final verification results.
- The isolated stack and database used, temporary port substitutions, and cleanup status, without secrets.

Limit success claims to the coverage actually executed against the final code. A clean startup or absence of visible errors alone does not establish that the plan passed.
