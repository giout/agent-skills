---
name: resolved-comments
description: Retrieve pull request feedback with pr-comments and compare the branch with its base to list which requested corrections are implemented, outstanding, or unverifiable. Use when asked to audit whether PR comments were addressed.
---

# Resolved Pull Request Comments

Audit the pull request for `[BRANCH]`. Use `[BASE]` when supplied; otherwise resolve the base from the pull request. This is a read-only assessment of implementation, not an operation to resolve review threads.

## Workflow

1. Locate and read the existing `pr-comments` skill and execute its retrieval workflow. Preserve each comment's identifier, URL, author, full content, file and line, thread context, and accessible review state. Reuse credentials already supplied securely; never include tokens in the report. If that skill is unavailable, report the missing dependency rather than silently substituting a different workflow.
2. Resolve the PR's current head and base commits. Confirm the local refs represent those commits before drawing conclusions; disclose stale or unavailable refs. Compute the branch diff from the merge base of base and head. Inspect committed branch content without incorporating unrelated working-tree changes.
3. Read applicable repository instructions and relevant module documentation. Treat comments as proposed corrections: identify the requested behavior, rationale, acceptance condition, and any later clarification or explicit withdrawal in the thread. Separate multiple independent requests within one comment while preserving their parent identifier.
4. Trace each request through the current branch code, callers, tests, and relevant history. Follow renamed or deleted code to its replacement. Compare with the base to establish what changed, but verify the final behavior even when the request concerns code outside the diff. A different implementation may fulfill the same requirement.
5. Judge whether the requested correction is substantively satisfied. Check the original failure scenario and related paths, rather than relying on matching words, an author's claim, a commit message, or GitHub's resolved/outdated flag. A requested test requires the relevant assertion to exist; test execution is a separate fact.
6. Account for every retrieved comment using the statuses below. If a comment's premise is incorrect, explain the disagreement with code evidence; do not mark an unimplemented request as fulfilled merely because you disagree with it.

## Statuses

- **Fulfilled:** all acceptance conditions are met in the current branch. Note whether the branch implemented the correction or the behavior already existed in the base.
- **Not fulfilled:** the requested behavior remains absent or incorrect. For partial implementation, identify both the completed and missing parts here.
- **Not verifiable:** missing code, permissions, external behavior, ambiguous intent, or other evidence prevents a reliable judgment. Name the missing evidence.
- **No pending correction:** informational comments, questions without a requested change, duplicates, or explicitly withdrawn/superseded requests. Link duplicates and superseded requests to the entry that covers them. A reply claiming completion does not withdraw a request.

## Report

State the PR, branch, base, reviewed commit SHAs, and retrieval or verification limitations. Group entries into **Fulfilled**, **Not fulfilled**, **Not verifiable**, and **No pending correction**, keeping stable comment identifiers and links.

For every entry include the author, a faithful summary of the correction, the verdict and reason, and current `path:line` evidence when applicable. For outstanding requests, name the smallest remaining correction. Distinguish static evidence, tests inspected, and checks actually run. End with totals that account for every comment and identify split requests without double-counting their parent comments.

Do not edit code, reply to comments, submit reviews, or change thread resolution state.
