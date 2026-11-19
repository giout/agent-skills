---
name: implementation-plan
description: Build a repository-grounded implementation plan from supplied requirements, including concrete changes, dependencies, acceptance criteria, and validation. Use when asked to plan a feature or change before implementation.
---

# Implementation Plan

Prepare an implementation plan for these requirements:

> [REQUIREMENTS]

Produce a plan; do not implement it unless the user separately requests implementation.

Requires the installed third-party skills `grill-me` and `grilling` from [mattpocock/skills](https://www.aihero.dev/skills-grill-me). Install both with `npx skills add mattpocock/skills --skill grill-me --skill grilling`. If either is unavailable, stop and report the missing dependency before planning.

If the project has an `AGENTS.md`, read it and follow its applicable instructions to the letter throughout planning. Include any nested `AGENTS.md` files governing the affected paths and follow the documents they require. Treat these instructions as mandatory, not advisory, subject to higher-priority instructions. Ensure the proposed implementation and validation conform to them.

## Workflow

1. Extract the intended outcome, actors, workflows, constraints, and observable acceptance criteria. Distinguish explicit requirements from assumptions. Resolve supplied links or documents when available rather than guessing their contents.
2. Read applicable repository instructions and relevant module documentation. Trace the existing workflow, public contracts, data ownership, and established implementations. Use CodeGraph first when the repository is indexed. Identify reusable code and the smallest coherent change that meets the requirements.
3. Examine each requirement for essential edge cases and unresolved decisions: permissions, invalid or empty inputs, state transitions, concurrency, retries, partial failure, external services, and compatibility with existing data when relevant. Check whether the requirements and existing behavior define the expected outcome for each relevant case. Do not turn every possible concern into a mandatory workstream.
4. Read the installed `grill-me` skill and follow its `grilling` dependency to resolve the questions identified above before finalizing the plan. If the agent has no Skill tool, read and follow the installed `grilling/SKILL.md` directly. Ground the interview in the supplied requirements, repository findings, and relevant edge cases. Resolve factual questions through repository exploration; ask the user for decisions, not facts you can look up. Ask rounds of questions whose prerequisites are settled, wait for answers, then reassess the requirements and edge cases to find the next round. Keep the interview within the requested scope. Continue independent research while answers are pending, but do not infer answers from silence or produce the final plan while relevant doubts remain. When no questions remain, summarize the agreed decisions and obtain the user's confirmation of shared understanding before proceeding. If a decision needs evidence the conversation cannot provide, identify what is needed and keep the plan pending until it is resolved or the user explicitly removes that part from scope.
5. Break the work into ordered, reviewable steps. Name the actual modules, paths, symbols, schemas, endpoints, or UI flows affected, what changes in each, and why. Label proposed new paths as new. Keep each step tied to a requirement and identify dependencies between steps.
6. Use the [fix-comments](../fix-comments/SKILL.md) skill in planning mode to identify necessary comment and documentation work in the affected scope. Include its corrections in the implementation steps without editing files.
7. Plan meaningful validation of the acceptance criteria and failure scenarios. Reuse existing test layers and project commands. Include migrations, compatibility, rollout order, recovery, and observability only where the change needs them. Avoid unnecessary abstractions, dependencies, speculative features, and broad cleanup.
8. Check that every requirement maps to implementation work and validation, that the steps fit the existing architecture, and that no unresolved decisions remain within the agreed scope. If planning reveals a new relevant doubt, return to step 4 before delivering the final plan.

## Deliverable

Give a concise, actionable plan containing:

- The outcome, scope, and acceptance criteria.
- Relevant existing behavior and reusable implementations, with file references.
- Ordered implementation steps with affected paths, intended behavior, and dependencies.
- Relevant edge cases, their agreed or repository-supported expected behavior, and validation mapped to requirements and important failure cases. Separate proposed checks from checks already executed.
- Material risks, confirmed assumptions, and any exclusions explicitly agreed with the user.

Scale detail to the change. Do not invent estimates, paths presented as existing, product decisions, or claims of completed validation. Keep source files, Git state, and external services unchanged; write a plan artifact only when requested.
