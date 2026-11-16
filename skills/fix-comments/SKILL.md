---
name: fix-comments
description: Review or correct code comments, docstrings, and documentation using unslop plus rules for documenting current behavior without change history or duplicated constant values. Use when asked to clean up technical prose or as part of planning, implementation, or code review.
---

# Fix Comments

Wrap the installed [unslop](https://github.com/theclaymethod/unslop) skill for technical prose. Read its `SKILL.md` and use its report-only cleanup flow for English text; do not copy its phrase catalog into this skill. If unslop is unavailable, report that limitation and apply the rules below without claiming an unslop check or installing it automatically. For other languages, apply these rules in the original language and report that unslop does not support it.

## Scope and mode

Use the caller's affected files or branch diff. Read the surrounding implementation to establish what the prose should say; avoid unrelated cleanup.

- **Planning:** include the necessary comment and documentation changes in the plan without editing files.
- **Review:** report concrete defects and proposed corrections against the reviewed revision; keep the work read-only.
- **Fix:** when edits are authorized, apply confirmed corrections to comments, docstrings, and documentation only. Do not change executable code or user-facing messages.

If the caller does not authorize edits, use review mode. Preserve directives and functional annotations such as lint suppressions, type-checker pragmas, generated-file markers, and license notices.

## Workflow

1. Read the scoped prose alongside the code and applicable repository instructions. Identify inaccurate, redundant, or unclear explanations.
2. Run unslop in report-only mode on the English prose extracted from comments or documentation, retaining its source locations. Exclude executable code and fenced code examples; a code fence around a whole source file must not hide its comments from review. Run any scanners sequentially, one explicit input file per command, and assess suggestions in context.
3. Apply the additional rules below. These rules explicitly permit removing historical narration and replacing duplicated constant values with symbolic references; preserve the remaining technical meaning rather than treating those removals as accidental fact loss.
4. In fix mode, apply only confirmed edits. Re-read the resulting prose against the implementation and inspect the diff to ensure behavior, directives, code examples, and unrelated text remain unchanged. Leave already useful prose alone.
5. Report corrected or proposed issues with file locations and any validation or dependency limitations. In planning mode, fold these into the plan; in review mode, use the caller's findings format.

## Additional rules

- Describe current behavior, constraints, and non-obvious reasons. Remove narration such as "previously", "now uses", "changed from", or "added in this fix" from ordinary comments and documentation. Put change history in commit messages, PR descriptions, changelogs, or migration/release notes when those artifacts are requested. Preserve migration instructions and compatibility explanations that readers still need to act correctly.
- Do not repeat a constant's hardcoded value in prose. Refer to its symbol or configuration key and explain its purpose; for example, use "Retries up to `MAX_RETRIES`" instead of "Retries up to 3 times". Do not invent a symbol or change the constant. Preserve numeric facts that are independent requirements, protocol constraints, or necessary usage examples rather than copies of an implementation constant.
- Delete comments that merely restate obvious code. Keep explanations of intent, invariants, tradeoffs, and external constraints that the implementation alone cannot convey.
- Prefer direct, concise wording over filler or formulaic phrasing. Preserve technical terms, links, scope, negation, uncertainty, and requirements; do not invent reasons or guarantees.
