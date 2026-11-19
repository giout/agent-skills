---
name: e2e
description: Plan and run essential manual regression tests for a specified branch against an already running application stack. Use when asked to verify affected application workflows end to end.
---

# End-to-End Verification

Use `[BRANCH]` as the target branch and `[BASE]` as the base branch, with defaults defined by [test-plan](../test-plan/SKILL.md). Use `[IMPLEMENTATION_PLAN]` from the user or conversation when available. The application stack is already running. Analyze the target branch changes against `[BASE]`, prepare a list of essential manual tests, and execute the relevant application workflows to check for regressions.

## Third-party Skills

- agent-browser

## Workflow

1. Use the [test-plan](../test-plan/SKILL.md) skill with `[IMPLEMENTATION_PLAN]`, `[BRANCH]`, and `[BASE]` to prepare essential manual end-to-end tests for the intended outcome and related regressions. If no implementation plan is available, supply a summary of the changed behavior grounded in the branch diff and available requirements, distinguish inferred intent from explicit requirements, and clarify any uncertainty that prevents defining expected outcomes.
2. Confirm that the running stack serves the target branch's code before testing; report a mismatch as a blocker rather than testing another branch or restarting the stack without authorization. Check the generated test plan's prerequisites against the running environment.
3. Run the applicable tests in the web application using the installed `agent-browser` skill. Record actual outcomes and any blockers.
4. Report the tests performed, their results, and any failures with reproduction steps. Distinguish failed tests from tests that could not be executed.

## Environment and Constraints

- Open `[WEB_ADDRESS]` by default.
- Sign in using `[USER]` and password `[PASSWORD]`.
- Create test data manually through the web application. If significant limitations prevent this, insert the required data directly into the database using the supplied `[DATABASE_URL]`.
- Use `[DATABASE_URL]` when database access is necessary. Keep database credentials out of committed files and reports.
- If unit tests are useful, run them one file at a time. Never execute more than one `.spec` file at once, to limit resource usage.
