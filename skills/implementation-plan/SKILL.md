---
name: implementation-plan
description: Build a repository-grounded implementation plan from supplied requirements, including concrete changes, dependencies, acceptance criteria, and validation. Use when asked to plan a feature or change before implementation.
---

# Implementation Plan

Prepare an implementation plan for these requirements:

> [REQUIREMENTS]

Produce a plan; do not implement it unless the user separately requests implementation.

If the project has an `AGENTS.md`, read it and follow its applicable instructions to the letter throughout planning. Include any nested `AGENTS.md` files governing the affected paths and follow the documents they require. Treat these instructions as mandatory, not advisory, subject to higher-priority instructions. Ensure the proposed implementation and validation conform to them.

## Workflow

1. Extract the intended outcome, actors, workflows, constraints, and observable acceptance criteria. Distinguish explicit requirements from assumptions. Resolve supplied links or documents when available rather than guessing their contents.
2. Read applicable repository instructions and relevant module documentation. Trace the existing workflow, public contracts, data ownership, and established implementations. Use CodeGraph first when the repository is indexed. Identify reusable code and the smallest coherent change that meets the requirements.
3. Identify essential edge cases and unresolved decisions: permissions, invalid or empty inputs, state transitions, concurrency, retries, partial failure, external services, and compatibility with existing data when relevant. Do not turn every possible concern into a mandatory workstream.
4. Resolve questions from the repository and requirements where possible. Ask about decisions that materially change behavior or architecture and cannot be inferred. Continue independent planning while answers are pending; keep dependent work explicitly conditional instead of inventing a decision.
5. Break the work into ordered, reviewable steps. Name the actual modules, paths, symbols, schemas, endpoints, or UI flows affected, what changes in each, and why. Label proposed new paths as new. Keep each step tied to a requirement and identify dependencies between steps.
6. Plan meaningful validation of the acceptance criteria and failure scenarios. Reuse existing test layers and project commands. Include migrations, compatibility, rollout order, recovery, and observability only where the change needs them. Avoid unnecessary abstractions, dependencies, speculative features, and broad cleanup.
7. Check that every requirement maps to implementation work and validation, that the steps fit the existing architecture, and that unresolved decisions remain visible.

## Deliverable

Give a concise, actionable plan containing:

- The outcome, scope, and acceptance criteria.
- Relevant existing behavior and reusable implementations, with file references.
- Ordered implementation steps with affected paths, intended behavior, and dependencies.
- Validation mapped to requirements and important failure cases. Separate proposed checks from checks already executed.
- Material risks, assumptions, and unresolved decisions, including which steps they block.

Scale detail to the change. Do not invent estimates, paths presented as existing, product decisions, or claims of completed validation. Keep source files, Git state, and external services unchanged; write a plan artifact only when requested.
