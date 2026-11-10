---
name: matt-review
description: "Reviews changes for correctness and personal, team, and project standards, and verifies claims with fresh evidence when asked. Use for substantial Matt Mode handoffs, change/PR reviews, verification requests, or consequential invariants—not every small edit."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Review

Review artifacts, not the author's confidence. Stay read-only unless asked to fix findings. Match the requested scope: a casual question about a helper is not a request for a full PR audit.

For the end-of-session review of substantial implementation, use one fresh read-only subagent after initial verification unless the user opts out. Review the complete session diff, not just the last fix. Give the reviewer the intent, exact diff, personal preferences, applicable team guidance, project policy sources, and verification evidence—not the author's defense. Have it read those sources directly rather than trust the implementer's summary of compliance.

## Brief the Reviewer Without Steering It

The reviewer finds what the author did not think of, so the brief must not narrow it to what the author did think of.

- Give: the intent in the user's words, the exact revisions or diff range of the tree to be published, the paths of the policy sources (personal preferences, review expectations, the [state and failure checklist](../matt-mode/references/state-and-failure-checklist.md), the target repository's `AGENTS.md`, rule files, and skills), and how to run the checks.
- Do not give: a "look for" list of the author's own concerns, a list of things not to relitigate, the author's verification results as settled fact, or a word or finding cap. Name a user's hand-made changes as theirs, without asking the reviewer to skip them. If you have a specific worry, ask it as a separate question after the open review.
- For a stack, name each layer's range and have each reviewed as it would merge on its own.
- Use this skill, not an improvised agent prompt. A harness's built-in code review that runs in the author's context, or that reads only `CLAUDE.md`, does not replace it in a repository whose policy lives in rule files.

For smaller edits and conversational review questions, work directly. Delegate when explicitly requested or when independence helps check a consequential invariant. Be honest about whether review was independent; if delegation is unavailable, self-review and disclose it. The parent owns adjudication, fixes, and reverification.

## Inspect the Change

Establish the review target, intended behavior, applicable team and repository policy, and relevant tests. Include new files and the complete task diff; for a stack, distinguish each PR's delta from its ancestors. Read owning files, callers, and tests where needed rather than restricting review to snippets.

Read [personal preferences](../matt-mode/references/personal-preferences.md) for names, function signatures, comments, types, and architecture. In Purelend repositories, also read [review expectations](../matt-mode/references/purelend-review-expectations.md). Repository contracts take precedence. Check applicable `AGENTS.md`, Cursor rules, and team skills directly; legacy style is not permission to ignore current policy.

Check both intent and quality:

- Does the change deliver the requested behavior without omitted requirements or scope creep? Challenge speculative requirements rather than demanding machinery for every ticket bullet.
- Trace plausible success, boundary, failure, retry, stale-state, and concurrency paths where relevant. Check authorization and trust at the owning boundary, and agreement between types, schemas, persistence, and outputs.
- Can the tests fail for the claimed defect? Do they cover consumer behavior rather than internal calls or incidental formatting?
- Could existing code or a smaller interface replace the new abstraction? Flag weak names, narration comments, duplicated shapes, unjustified casts, impossible-state guards, and unnecessary dependencies.

Then make four passes that a read of the diff alone misses:

- **Rules, file by file.** For each changed file, list the rule files whose scope matches it, in the repository that owns the file, and check the file against each. Include test files against the testing rules. "Placement and conventions look fine" is not a finding; name the rules checked.
- **Siblings.** For each new route, page, component, hook, mutation, or test, open the nearest existing one of the same kind and note what it does that the new one does not: permission gate, metadata, typed error, cache handling, factory, a repository skill's checklist.
- **Callers.** For each shared component, hook, helper, or constant whose behavior or default changed, list the callers and check the ones the task did not mean to touch.
- **States and failures.** Answer the [state and failure checklist](../matt-mode/references/state-and-failure-checklist.md) for the change.

Also check that each claim in the handoff or PR body is true of the diff and covered by a test, that each stack layer has no export, prop, or parameter without a caller in that layer, and that rule files still describe the code.

Verify suspected defects against concrete inputs and execution paths. Keep unverified concerns conditional; do not report pattern-matching suspicions as bugs. A concern you could verify by reading one more file or running one test is verified, not left conditional.

## Verify Claims When Needed

When asked to verify behavior, or when a review claim needs execution evidence, follow [verification guidance](references/verification.md). A verification-only request does not require a separate review agent or authorize repairs. Use the current repository's commands and relevant consumer surface, not a frozen per-repo command table. Distinguish actual pass/fail results from skipped checks, empty selections, and blocked environments.

## Report What Matters

Lead with the answer to the user's question. For important findings, give file/symbol, the violated contract, concrete impact, and a correction direction.

Sort findings by what happens if they ship, not by how often it happens:

- **Fix before publishing:** wrong result or lost work on an interaction a user can perform, a failure the user is not told about, stale or racy state, a contract or resource limit exceeded, a missing permission check, a test that cannot fail, and a violation of a written repository rule. Rarity does not move a finding out of this group.
- **Decide:** a product or design choice the author made without asking, and a deviation from the ticket or from a rule's pattern. State the question for the user.
- **Optional:** taste, naming alternatives, and structure that works as written.

Do not open with "no blockers" or "nothing major" when the first group is non-empty. A reviewer on the team will raise those items, so they are the report's headline.

If there are no findings, say what you inspected and disclose material limitations. Do not invent a finding or print a verdict matrix to appear thorough. Give a PR-readiness assessment when requested, not by default.

After fixes, recheck the affected diff and integration points rather than restarting an unfocused whole-repository review. When a fix restructured code, added state, or touched shared code, the recheck is a fresh reviewer on the fix's diff: fixes to review findings have introduced the next round of findings more than once.
