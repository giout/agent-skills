---
name: pr-body
description: Fill or update a pull request body from the branch changes while preserving its existing format. Use when asked to write the description of a pull request.
---

# Pull Request Body

Use the supplied PAT (`[TOKEN]`) and `curl` to update the pull request body for `[BRANCH]`.

## Workflow

1. Identify the repository and the pull request for the specified branch. Read its current body and determine its base branch.
2. Compare the branch against its base in Git, using their merge base to isolate the branch changes. Read the relevant changes to understand their purpose and behavior.
3. Fill in the existing PR format without removing, renaming, or reordering titles or subtitles. Leave sections blank when there is nothing supported by the changes to put there.
4. If no format exists, write a general, non-technical description and a technical description, organized as appropriate for the changes.
5. Update the PR body through the hosting provider's API using `curl`, then read it back to verify the result.

Keep claims grounded in the diff and available evidence. Do not claim tests were run without evidence. Keep the PAT out of committed files, reports, and displayed command output. If the repository, branch, or PR cannot be identified unambiguously, request the missing information before updating it.

## Result

Return the pull request link and a brief description of the body update.
