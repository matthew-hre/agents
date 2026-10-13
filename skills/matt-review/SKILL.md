---
name: matt-review
description: "Reviews code for correctness and enforcement of personal, team, and project guidance. Use at the end of a larger Matt Mode implementation session, when asked to review a change or PR, or to check a consequential invariant; not for every small edit."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Review

Review artifacts, not the author's confidence. Stay read-only unless asked to fix findings. Match the requested scope: a casual question about a helper is not a request for a full PR audit.

For the end-of-session review of substantial implementation, use one fresh read-only subagent after initial verification unless the user opts out. Review the complete session diff, not just the last fix. Give the reviewer the intent, exact diff, personal preferences, applicable team guidance, project policy sources, and verification evidence—not the author's defense. Have it read those sources directly rather than trust the implementer's summary of compliance.

For smaller edits and conversational review questions, work directly. Delegate when explicitly requested or when independence helps check a consequential invariant. Be honest about whether review was independent; if delegation is unavailable, self-review and disclose it. The parent owns adjudication, fixes, and reverification.

## Inspect the Change

Establish the review target, intended behavior, applicable team and repository policy, and relevant tests. Read [personal preferences](../matt-mode/references/personal-preferences.md) for names, function signatures, comments, types, and architecture; repository contracts take precedence. Check applicable `AGENTS.md`, Cursor rules, and team skills directly.

Check both intent and quality:

- Does the change deliver the requested behavior without omitted requirements or scope creep? Challenge speculative requirements rather than demanding machinery for every ticket bullet.
- Trace plausible success, boundary, failure, retry, stale-state, and concurrency paths where relevant. Check authorization and trust at the owning boundary, and agreement between types, schemas, persistence, and outputs.
- Can the tests fail for the claimed defect? Do they cover consumer behavior rather than internal calls or incidental formatting?
- Could existing code or a smaller interface replace the new abstraction? Flag weak names, narration comments, duplicated shapes, unjustified casts, impossible-state guards, and unnecessary dependencies.

Verify suspected defects against concrete inputs and execution paths. Keep unverified concerns conditional; do not report pattern-matching suspicions as bugs.

## Report What Matters

Lead with the answer to the user's question. For important findings, give severity, file/symbol, the violated contract, concrete impact, and a correction direction. Separate optional preferences from blockers.

If there are no findings, say what you inspected and disclose material limitations. Do not invent a finding or print a verdict matrix to appear thorough. Give a PR-readiness assessment when requested, not by default.

After fixes, recheck the affected diff and integration points rather than restarting an unfocused whole-repository review.
