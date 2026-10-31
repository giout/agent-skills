---
name: security-review
description: Review a branch against its base for security vulnerabilities and exploitable business-logic flaws, tracing trust boundaries and functional abuse scenarios. Use when asked for a vulnerability or security-focused code review.
---

# Security and Functional Vulnerability Review

Review `[BRANCH]` against `[BASE]` for vulnerabilities in both technical controls and business workflows. Focus on introduced or worsened exposure and substantiate every finding with a reachable scenario.

## Establish scope and threat context

1. Resolve both refs to commit SHAs and compute the merge base. Inspect `git diff <merge-base> <branch-sha>` and its changed-file list. Review committed branch content, excluding unrelated local changes. Report unresolved refs; stop with an explicit result if the diff is empty.
2. Read applicable repository instructions, domain documentation, security conventions, and relevant module READMEs. Use CodeGraph first when indexed, checking source evidence against the reviewed commit. Inspect callers, middleware, shared guards, schemas, persistence, and consumers beyond the diff as needed.
3. Identify the changed entry points, actors, assets, trust boundaries, and sensitive effects. Establish what unauthenticated users, ordinary users, privileged users, tenants, external providers, workers, and webhooks can actually control. Derive intended permissions and workflow invariants from the repository and requirements.

## Technical vulnerability pass

Follow untrusted data and authority from entry point to sensitive operation. Apply the relevant lenses to the actual stack:

- Authentication/session verification, authorization on every affected path, role escalation, ownership checks, and cross-tenant object access. Include list, detail, export, bulk, and indirect relationship paths where affected.
- Injection into database queries, shell commands, templates, HTML, and other interpreters; unsafe deserialization or file handling; path traversal; SSRF and redirects that cross a trust boundary.
- Browser trust boundaries: script execution, credentialed cross-origin actions, CSRF protections where applicable, cookie/session handling, and sensitive client-visible data.
- Secret and personal-data exposure in responses, logs, errors, caches, storage, or changed configuration. Cite locations and redact secret values in the report.
- Webhook signatures, payload binding, event identity, replay handling, and trust in third-party responses.
- Resource exhaustion, unbounded work, expensive queries, upload limits, and attacker-controlled fan-out where reachability and scale establish a material risk.
- Changed dependencies, deployment settings, or cryptographic use when they affect a reachable attack surface. Verify version-specific vulnerability claims with authoritative advisories; a package name alone is not evidence.

Check compensating controls before reporting a missing local guard. A client-side check is not a server-side authorization boundary.

## Functional abuse pass

Trace legitimate operations in unintended sequences and combinations. Check whether an actor can bypass the intended business contract through:

- Skipped or repeated transitions, alternate endpoints, stale state, forged relationships, or mass assignment of server-owned fields.
- Concurrent requests, check-then-act races, duplicate webhook delivery, retries, and absent idempotency around sensitive effects.
- Manipulated prices, quantities, currencies, discounts, quotas, credits, entitlements, approvals, or other domain values. Verify precision and authoritative calculation where relevant.
- Partial success, external success with lost response, process failure before persistence, or recovery paths that duplicate effects, lose assets, or leave protected operations reclaimable by another actor.

State the violated invariant, required actor privileges, triggering sequence, and resulting unauthorized effect or material loss. Include functional integrity failures with significant impact even when no malicious actor is required, labeling them as integrity defects rather than exploits. Keep ordinary style and maintainability advice outside this review.

## Validate and prioritize

For every candidate, trace the complete reachable path, inspect existing protections, compare with the base, and look for counterevidence in callers and tests. Keep a private record of covered entry points and invariants so one severe finding does not end the review early.

Confirm uncertain framework, protocol, and advisory claims against installed versions and primary sources. Use isolated local reasoning, tests, or minimal non-destructive reproductions when useful and permitted; do not probe production or third-party targets. Never present a hypothetical exploit or unexecuted check as reproduced.

Deduplicate by root cause while naming affected paths. Separate existing vulnerabilities from issues introduced or worsened by this branch. Keep unresolved suspicions in a clearly labeled investigation section with the missing evidence, not among confirmed findings.

## Report

State branch/base SHAs and scope, then list findings by impact and likelihood using stable IDs and **Critical**, **High**, **Medium**, or **Low** severity. Confidence is a separate assessment, not a substitute for impact.

For each finding provide:

- The vulnerability or violated functional invariant and the smallest relevant `path:line` reference.
- Actor capabilities, attacker-controlled input, and prerequisites.
- A concrete request sequence or failure scenario, the affected asset or users, and the resulting impact.
- Evidence of the reachable path and why existing controls do not prevent it; distinguish static analysis from executed reproduction.
- The smallest correction at the owning boundary and a regression check that exercises the failure or abuse scenario.

Close with coverage and access limitations, checks actually run, and unresolved investigations. If no substantiated findings remain, say so without claiming the code is secure.

Keep repository and external state unchanged. Do not apply fixes, post comments, expose secret values, or perform live exploitation as part of the review.
