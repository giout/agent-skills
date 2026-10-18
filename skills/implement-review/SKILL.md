---
name: implement-review
description: Evaluate a review, reassess proposed fixes for simplicity and correctness, implement the agreed findings, and explain disagreements. Use when asked to apply review feedback to a specified branch.
---

# Implement Review Feedback

Use `[BRANCH]` as the target branch and `[REVIEW_FILE]` as the review input.

## Workflow

1. Read the review and inspect the target branch and affected code. Preserve unrelated workspace changes.
2. Execute the `review-thoughts` skill against the review and branch.
3. Reassess each proposed fix. Confirm that it addresses the root cause with the simplest direct solution, avoids unnecessary complexity and fragile changes spread across several places, and accounts for related paths that could generate further review findings.
4. Present the revised review thoughts: which findings are agreed or disagreed with, why, and the intended fixes. Continue with the agreed fixes unless the user has requested a separate approval step.
5. Implement the agreed fixes and improvements. Validate the affected behavior with checks appropriate to the changes, and inspect the final diff for regressions or newly introduced issues.
6. Report the implemented changes and validation results. Provide a list of disagreed findings with concise technical reasons that the user can send to the reviewer. If there are none, state that explicitly.

Base agreement on the code and evidence rather than accepting every finding automatically. Report remaining uncertainty instead of promising that no future review can find another issue.
