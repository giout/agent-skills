---
name: test-plan
description: Create a focused test plan from an implementation plan and a branch comparison, covering acceptance criteria and related regressions. Use before implementation or as the planning phase of end-to-end verification.
---

# Test Plan

## Inputs

- `[IMPLEMENTATION_PLAN]`: the plan to implement, supplied directly, in the conversation, or through a referenced document.
- `[BRANCH]`: the target branch. Default to the conversation's checkout branch, or the main checkout's current branch when there is no separate conversation checkout.
- `[BASE]`: the base branch. Default to the repository's configured default branch, resolved from remote HEAD or available repository metadata.

Create a list of tests that verify the intended outcome and detect collateral damage in affected existing behavior. Plan only; do not execute tests, implement changes, switch branches, or modify files or services. Write a plan artifact only when requested.

## Workflow

1. Read applicable `AGENTS.md` files and the documents they require. Resolve the implementation plan and extract its objective, acceptance criteria, affected workflows, and relevant failure cases. Ask focused questions if missing or contradictory information prevents defining expected results; do not invent product decisions.
2. Resolve `[BRANCH]` and `[BASE]` to commit SHAs and compute their merge base. Report the resolved refs. If a default cannot be determined unambiguously, ask for it. Inspect `git diff <merge-base> <branch-sha>` without changing checkouts. A missing implementation diff is valid before implementation: use the plan and current architecture, clearly distinguishing proposed changes from implemented behavior. If changes differ from the plan, make the discrepancy visible and cover the actual changes as well as the stated objective.
3. Trace the affected workflows, shared code, contracts, and relevant existing tests. Use CodeGraph first when the repository is indexed. Identify existing behavior that could regress through the changed components, including other callers or consumers. Keep regression coverage tied to concrete dependencies rather than listing unrelated application features.
4. Select the smallest set of meaningful tests that covers every acceptance criterion and the identified regression risks. Include relevant edge cases and failure paths, such as permissions, invalid inputs, state transitions, retries, or partial failure, only when supported by the plan or affected behavior. Reuse existing tests where they provide the needed coverage. When invoked by another skill, respect its requested test scope; for `e2e`, prioritize essential manual application workflows.
5. Describe each test with a stable ID, the criterion or regression risk it covers, prerequisites and test data, concrete steps or an existing test reference, and observable expected results. Identify the appropriate layer (manual end-to-end, unit, integration, or another existing layer). Name commands and paths only when verified in the repository; file-based commands must target one exact file each, and type checking must target one application or library per command. Record execution blockers without claiming the tests ran.
6. Check that every acceptance criterion has coverage and each regression test has an evidence-backed reason to exist. Make coverage gaps and unresolved expectations explicit; they prevent claiming that the objective or absence of regressions has been verified.

## Deliverable

Return a concise, actionable test list with:

- The objective, scope, and resolved branch/base refs and merge base.
- Tests grouped into objective/acceptance coverage and related regression coverage, with the details from step 5.
- A mapping from acceptance criteria and regression risks to test IDs.
- Prerequisites, blockers, and any coverage limitations or assumptions.

All listed tests are proposed, not executed. When called from `e2e`, hand this list back to that skill for execution and reporting.
