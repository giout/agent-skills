---
name: edge-cases
description: Identify essential edge cases and unresolved implementation decisions for a proposed feature using the current branch and its base as context. Use before planning a feature implementation.
---

# Feature Edge Cases

Analyze the proposed requirement (`[FEATURE]`) in the context of the current system. Identify fundamental edge cases and critical decisions that must be resolved before implementation planning.

## Workflow

1. Identify the current branch and confirm its base. Use `[BASE_BRANCH]` when supplied; otherwise determine it from repository and pull request information. Ask if it remains ambiguous.
2. Compare the current branch with its base in Git, using their merge base. Inspect affected code and existing behavior relevant to the requirement.
3. Analyze the main workflow and its meaningful boundary and failure conditions. Consider permissions, data consistency, state transitions, concurrency, retries, external services, and existing-data compatibility when relevant to the feature.
4. Separate questions already answered by the code or requirement from decisions that remain unresolved.
5. Report the essential findings, explaining the triggering scenario, its consequence, and the decision or clarification needed. Prioritize issues that block a sound implementation plan.

Ground findings in the requirement and actual system behavior. Avoid speculative cases unrelated to the feature. This workflow produces analysis, not implementation changes.
