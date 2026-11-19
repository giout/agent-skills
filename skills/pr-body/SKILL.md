---
name: pr-body
description: Fill or update a pull request body from the branch changes while preserving its existing format. Use when asked to write the description of a pull request.
---

# Pull Request Body

Prepare the pull request body for `[BRANCH]` against `[BASE]`. Default to updating the existing PR; when invoked in **compose mode**, return the prepared body without creating or updating a PR. Reuse available authenticated hosting tools; use the supplied PAT (`[TOKEN]`) with `curl` when that is the configured access method.

## Workflow

1. Identify the repository and any existing pull request for the specified branch. Read its current body and determine its base branch. In compose mode, a PR need not exist: use the caller's base and applicable repository PR template and contribution instructions.
2. Compare the branch against its base in Git, using their merge base to isolate the branch changes. Read the relevant changes to understand their purpose and behavior.
3. Fill in the existing PR format without removing, renaming, or reordering titles or subtitles. Leave sections blank when there is nothing supported by the changes to put there.
4. If no format exists, write a general, non-technical description and a technical description, organized as appropriate for the changes.
5. Include diagrams when they clarify a material workflow, architecture, or state transition, using the hosting provider's supported format. Include supplied screenshots with descriptive captions and reviewer-accessible attachment URLs; do not present temporary local paths as published images. Preserve the existing or required template structure, and include only validation supported by evidence.
6. In compose mode, return the body to the caller and stop. Otherwise update the PR body through the authenticated hosting provider tool or API, then read it back to verify the result.

Keep claims grounded in the diff and available evidence. Do not claim tests were run without evidence. Keep the PAT out of committed files, reports, and displayed command output. If the repository, branch, or PR cannot be identified unambiguously, request the missing information before updating it.

## Result

In compose mode, return the prepared body and any unresolved attachment requirements. Otherwise return the pull request link and a brief description of the body update.
