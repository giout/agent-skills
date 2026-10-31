---
name: coding-review
description: Review a branch against its base for correctness, code quality, and repository conventions, with evidence-backed findings and scrutinized fixes. Use when asked for a general code review.
---

# Coding Review

Review `[BRANCH]` against `[BASE]` with a senior engineer's product awareness. Complete three independent analytical passes: correctness, quality, and repository standards. Finding counts in one dimension must not limit coverage of another.

This workflow adapts the reference-first, findings-first approach of `senior-review` in xprmedia-dxp. It is self-contained and applies the target repository's own contracts and conventions.

If the project has an `AGENTS.md`, read it and follow its applicable instructions to the letter throughout the review. Include any nested `AGENTS.md` files governing the reviewed paths and follow the documents they require. Treat these instructions as mandatory, not advisory, subject to higher-priority instructions.

## Resolve scope and references

1. Resolve `[BRANCH]` and `[BASE]` to commit SHAs and compute their merge base. Use `git diff <merge-base> <branch-sha>` and its changed-file list. Do not substitute the checked-out branch or include uncommitted changes. Report missing or ambiguous refs; when the diff is empty, state that there is nothing to review.
2. Inspect the exact branch content with read-only Git operations. Read applicable `AGENTS.md` instructions, coding standards, product/domain documentation, selected convention documents, and the nearest module READMEs when they exist. Use CodeGraph first in indexed repositories, and verify that source evidence matches the reviewed commit.
3. Inventory materially changed helpers, components, services, types, schemas, states, error paths, dependencies, tests, and public contracts. For each changed concept, inspect the closest established implementation in its owning module and shared code. Keep a private reference ledger of the concept, reference path, and contract difference. Record “none found” only after looking.
4. Trace callers and consumers across layers. Read unchanged code where needed to establish impact and distinguish introduced or worsened issues from pre-existing behavior.

## Correctness pass

Trace realistic inputs through outputs, persisted state, and external effects. Look for:

- Logic errors, invalid/empty/null handling, async ordering, and incompatible caller or data contracts.
- Authorization and tenant/actor isolation enforced at the owning boundary, including indirect lookup and mutation paths.
- Transactions, races, lost updates, retries, duplicate processing, and unsafe state transitions.
- Integration failure timelines: external success with a lost response, process exit before local completion, retry, reconciliation, and partial completion in a batch. Check idempotency and durable recovery where the workflow requires them.
- Silent failures: broad catches, swallowed errors, misleading success, fallbacks that hide invalid state, and exhausted retries.

Require a concrete trigger and consequence. For isolation findings, identify whose data or operation becomes accessible to whom.

## Quality pass

Review every inventoried concept independently of whether the code currently behaves correctly:

- Tie branches, states, options, wrappers, fallbacks, and compatibility paths to a current requirement or known consumer.
- Compare reuse against actual semantics; identify unnecessary duplication, indirection, unsupported flexibility, and misplaced responsibilities.
- Trace control and data flow, type evidence, valid model states, naming, cohesion, and repeated I/O at realistic data volumes.
- Evaluate whether tests protect behavior and whether comments explain non-obvious reasoning.

A quality finding needs a concrete simpler or safer form and a material reduction in reasoning cost, maintenance burden, or future defect risk. Leave pure taste and mechanically enforced formatting to existing checks.

## Repository standards pass

Build an applicability checklist from the repository's written instructions. Check every touched module, layer, public contract, and changed prose item against relevant rules. Cite the source of each reported convention violation; do not import xprmedia-dxp-specific rules into another repository.

For meaningful behavior changes, identify realistic regressions and inspect whether the appropriate tests protect them. Report coverage gaps only with a concrete unprotected behavior and consequence. Check changed comments, documentation, messages, and labels for accuracy against the resulting behavior.

## Synthesis and fix scrutiny

1. Account for every changed file, ledger entry, and applicable rule before concluding. Sweep related changed paths for repeated instances of each issue found.
2. Remove false positives and merge findings with the same root cause and correction. Keep every distinct substantiated finding across all three dimensions; do not impose an arbitrary count limit.
3. Anchor findings to the smallest useful changed line range and explain impact through actual consumers. An unchanged line may provide evidence, but connect the issue to the branch change. Separate pre-existing concerns from branch findings.
4. Verify uncertain library/runtime claims using the installed version and primary documentation. Label missing evidence instead of reporting speculation as fact.
5. For each survivor, design the smallest concrete correction against the exact code and contracts. Reconstruct its resulting code in the analysis and re-review correctness, quality, and standards, including caller updates and interactions with other proposed fixes. Group inseparable corrections together.
6. Revise a suggestion when it leaves the issue unresolved or creates a regression. Limit design and re-review to three rounds per fix group; if a safe correction remains unclear, preserve the finding and mark the fix **Needs investigation**. Distinguish code-based reasoning from validation that still requires execution; do not describe an unexecuted patch as tested.

## Report

Lead with actionable findings ordered by impact, using stable IDs and these severities:

- **Must-fix:** concrete correctness, security, isolation, integrity, or operational defects that should block shipping.
- **Should-fix:** material lower-impact defects, complexity, or written-rule violations with a concrete correction.
- **Optional:** small, substantiated improvements with a clear net benefit.

For each finding give the dimension (**Correctness**, **Quality**, or **Standards**), `path:line`, the trigger or unnecessary concept, its consequence, evidence, and the scrutinized correction or investigation needed. Severity reflects impact and likelihood, not confidence alone. Identify coupled fixes.

Finish with a brief assessment of correctness and maintainability separately, coverage limitations, and checks actually run or still needed. If no findings survive, say so plainly without guaranteeing the absence of defects.

Keep this review read-only. Do not edit code, create commits, or publish review comments. Apply fixes only when the user requests them; a review request alone does not authorize implementation.
