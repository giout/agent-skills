---
name: pr-comments
description: Retrieve and enumerate pull request comments, including accessible pending-review comments, with their authors. Use when asked to collect feedback for a branch's pull request.
---

# Pull Request Comments

Retrieve comments for the pull request associated with `[BRANCH]`, using the supplied PAT (`[TOKEN]`) and `curl`.

## Workflow

1. Identify the repository and the pull request for the specified branch.
2. Use the hosting provider's API through `curl` to retrieve PR conversation comments, review summaries, and inline review comments. Follow pagination to collect all available results.
3. Retrieve accessible pending reviews and their comments as well. If permissions or API visibility prevent access to pending comments, state that limitation instead of treating them as absent.
4. Present a numbered list containing each comment's author and full content. Include its file and line when applicable, and identify pending-review comments. Avoid duplicating the same comment across API responses.

Keep the PAT out of committed files, reports, and displayed command output. This workflow retrieves feedback; it does not submit pending reviews, reply to comments, or modify the pull request.
