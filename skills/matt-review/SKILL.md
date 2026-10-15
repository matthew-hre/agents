---
name: matt-review
description: "Reviews changes for correctness and personal, team, and project standards, and verifies claims with fresh evidence when asked. Use for substantial Matt Mode handoffs, change/PR reviews, verification requests, or consequential invariants—not every small edit."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Review

Review artifacts, not the author's confidence. Stay read-only unless asked to fix findings. Match the requested scope: a casual question about a helper is not a request for a full PR audit.

For the end-of-session review of substantial implementation, use one fresh read-only subagent after initial verification unless the user opts out. Review the complete session diff, not just the last fix. Give the reviewer the intent, exact diff, personal preferences, applicable team guidance, project policy sources, and verification evidence—not the author's defense. Have it read those sources directly rather than trust the implementer's summary of compliance.

For smaller edits and conversational review questions, work directly. Delegate when explicitly requested or when independence helps check a consequential invariant. Be honest about whether review was independent; if delegation is unavailable, self-review and disclose it. The parent owns adjudication, fixes, and reverification.

## Inspect the Change

Establish the review target, intended behavior, applicable team and repository policy, and relevant tests. Include new files and the complete task diff; for a stack, distinguish each PR's delta from its ancestors. Read owning files, callers, and tests where needed rather than restricting review to snippets.

Read [personal preferences](../matt-mode/references/personal-preferences.md) for names, function signatures, comments, types, and architecture. In Purelend repositories, also read [review expectations](../matt-mode/references/purelend-review-expectations.md). Repository contracts take precedence. Check applicable `AGENTS.md`, Cursor rules, and team skills directly; legacy style is not permission to ignore current policy.

Check both intent and quality:

- Does the change deliver the requested behavior without omitted requirements or scope creep? Challenge speculative requirements rather than demanding machinery for every ticket bullet.
- Trace plausible success, boundary, failure, retry, stale-state, and concurrency paths where relevant. Check authorization and trust at the owning boundary, and agreement between types, schemas, persistence, and outputs.
- Can the tests fail for the claimed defect? Do they cover consumer behavior rather than internal calls or incidental formatting?
- Could existing code or a smaller interface replace the new abstraction? Flag weak names, narration comments, duplicated shapes, unjustified casts, impossible-state guards, and unnecessary dependencies.

Verify suspected defects against concrete inputs and execution paths. Keep unverified concerns conditional; do not report pattern-matching suspicions as bugs.

## Verify Claims When Needed

When asked to verify behavior, or when a review claim needs execution evidence, follow [verification guidance](references/verification.md). A verification-only request does not require a separate review agent or authorize repairs. Use the current repository's commands and relevant consumer surface, not a frozen per-repo command table. Distinguish actual pass/fail results from skipped checks, empty selections, and blocked environments.

## Report What Matters

Lead with the answer to the user's question. For important findings, give severity, file/symbol, the violated contract, concrete impact, and a correction direction. Separate optional preferences from blockers.

If there are no findings, say what you inspected and disclose material limitations. Do not invent a finding or print a verdict matrix to appear thorough. Give a PR-readiness assessment when requested, not by default.

After fixes, recheck the affected diff and integration points rather than restarting an unfocused whole-repository review.
