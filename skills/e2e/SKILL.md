---
name: e2e
description: Plan and run essential manual regression tests for the current branch against an already running application stack. Use when asked to verify affected application workflows end to end.
---

# End-to-End Verification

The application stack is already running. Analyze the current branch changes against its base, prepare a list of essential manual tests, and execute the relevant application workflows to check for regressions.

## Third-party Skills

- agent-browser

## Workflow

1. Determine the current branch and its base. Inspect the Git diff from their merge base and the affected application flows.
2. List the essential manual tests for changed behavior and closely related regression paths. Include expected outcomes.
3. Run the applicable tests in the web application using the installed `agent-browser` skill. Record actual outcomes and any blockers.
4. Report the tests performed, their results, and any failures with reproduction steps. Distinguish failed tests from tests that could not be executed.

## Environment and Constraints

- Open `[WEB_ADDRESS]` by default.
- Sign in using `[USER]` and password `[PASSWORD]`.
- Create test data manually through the web application. If significant limitations prevent this, insert the required data directly into the database using the supplied `[DATABASE_URL]`.
- Use `[DATABASE_URL]` when database access is necessary. Keep database credentials out of committed files and reports.
- If unit tests are useful, run them one file at a time. Never execute more than one `.spec` file at once, to limit resource usage.
