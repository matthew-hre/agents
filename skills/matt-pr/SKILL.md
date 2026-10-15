---
name: matt-pr
description: "Prepares focused pull requests and maintains safe review history. Use when asked to restack a commit sequence, prepare, open, update, or maintain a PR, or when implementation needs independently mergeable units. Restacking may produce one commit, several logical commits, or stacked PRs."
compatibility: Cursor and Agent Skills-compatible coding agents with Jujutsu, GitHub CLI, filesystem, and shell access.
---

# Matt PR

Make the change easy to understand, verify, and roll out. Default to one focused PR; a stack is useful only when its parts are independently reviewable and safe to merge.

## Safety and Authority

If a `.jj` file or directory exists, use `jj` for local history and bookmarks for published branches; otherwise use Git. Inspect the working copy, ancestors, actual target branch, and existing PR before acting. Preserve unrelated work; never force a clean tree.

Local checkpoints and history refinement are part of authorized implementation. Before a PR is open, freely rebase, squash, combine, or split your task's local commits into a coherent sequence. Once a PR is open, its existing commits are immutable: append fixes, never squash, rebase, absorb into, or otherwise rewrite them. Multiple review-fix commits are fine. Only an explicit user override changes this rule.

Publishing, updating a remote PR, requesting reviewers, merging, and deploying need authorization for that action. Carry existing authorization forward without asking again, but do not infer it from a request for a local draft or readiness review. Never reply to or resolve human review threads as the user unless explicitly asked.

## Keep the Change Coherent

- "Restack" means reshape the task's commit sequence: reorder or update commits, squash everything into one coherent commit, split logical commits, or arrange them into multiple stacked PRs. It does not necessarily mean multiple PRs or just rebasing. "Restack and open a draft" authorizes choosing a coherent single-PR or stacked-PR arrangement and opening the corresponding draft PRs. Restacking alone does not authorize publication or override immutable open-PR history.
- One PR should have one reviewer narrative. Keep implementation, behavior-focused tests, generated output, and necessary documentation together.
- Search for a smaller complete outcome, not an arbitrary line-count target. A large diff is a reason to inspect boundaries; it is not permission to split a working feature into broken layers.
- Separate behavior-preserving refactoring from intentional behavior changes when each can stand alone. Prefer vertical slices and expand → migrate → contract for broad migrations.
- When multiple independently mergeable units are needed, read [stacking guidance](references/stacking.md) before coding or changing topology. Do not create a stack for routine edits.

## Verify Before Publication

Inspect the actual PR delta against its intended base, including new files—not just the last commit or all ancestors of a stacked branch. Apply `/matt-review` for the relevant scope and use its [verification guidance](../matt-review/references/verification.md). Read current package scripts, CI, and repository guidance instead of assuming commands from another repo.

Run the relevant checks against the exact tree being published and inspect their results. Include required changesets and regenerated contracts. For UI appearance changes, inspect rendered captures; for other behavior, exercise the consumed surface. Check deployment compatibility when contracts or schema change. Reverify affected behavior after fixes; a prior green result does not cover new edits.

JJ does not run Git pre-commit hooks automatically. Inspect the repository's hooks and run their checks manually before treating a change as ready. If a hook depends on Git's staged-file list, run its underlying commands against the actual changed files/tree instead of staging files to satisfy it. Reinspect any generated or formatted changes. If CI checks individual commits, verify those commits too; never rewrite open-PR history to run a fixer.

## Write for the Reviewer

- Don't write essays. Use a concise, intent-focused title and body; use bullet points for the text you do write. Respect mandatory repository requirements without adding template boilerplate.
- Explain the final aggregate change against the target branch, or the final ordered stack when relevant. Do not narrate intermediate commits, earlier implementations, discarded approaches, or how much you shrank the PR. Review-fix commits do not need their own story.
- Prefer useful Mermaid codeblock diagrams, code samples/snippets, and code references over paragraphs. Samples may show internals or caller usage. Choose the forms that explain this change; do not add a diagram merely to fill a section.
- Omit routine test/validation receipts, command lists, "I ran tests," and generic test-plan sections. Still perform verification and report material failures or gaps to the user; concise PR writing is not permission to hide a blocker.
- For truly impressive, difficult, high-risk, or wide-scope work, a technical-blog-style body is appropriate: give the context and story needed to understand it, supported by diagrams, code, and evidence. This is an exception, not the default.

## Show Comparisons

- For visual changes, direct or indirect, show a before/after table with uploaded images or videos. Capture the target-branch baseline and PR candidate in comparable states, with one row per meaningful affected state or viewport. Inspect both; use `/github-image-upload` to attach local artifacts and verify that authorized reviewers can open them.

| State | Before — target branch | After — PR |
|---|---|---|
| <affected state> | <uploaded image/video> | <uploaded image/video> |

- For benchmarks, always show a before/after table: baseline from the target branch, candidate from the PR, same workload and environment. Name units and useful deltas; do not substitute an earlier candidate commit for the baseline or imply an improvement without measurements. If either side cannot run, report the limitation rather than inventing a comparison.

| Metric | Before — target branch | After — PR | Change |
|---|---:|---:|---:|
| <metric and unit> | <measured baseline> | <measured candidate> | <delta> |

- Keep samples and artifacts sanitized. For nonvisual changes, use representative code, usage, responses, or measurements—not decorative screenshots. For dependent PRs, include only the final dependency links, layer scope, and necessary merge/deployment order, including cross-repository order.

## Publish and Maintain

- Before an external action, state the intended destination/base, material unresolved findings, and any specific authorization still needed. No mandatory readiness receipt or empty status matrix.
- When authorized to publish, create a draft unless the user explicitly asks for teammate review now. Use an authored title and `gh pr create --draft --body-file <file>` for a single PR; inspect the resulting title, base, and rendered body.
- After a PR is open, put fixes in new commits on the owning PR layer and leave existing history intact. Update examples, comparisons, and dependent layers when affected, without rewriting open-PR branches.
- Unless the user opts out, subscribe this thread to PR comments/reviews and CI workflow completions for the exact head commit using the platform's event subscriptions. Refresh the CI subscription after an authorized push. Monitoring reports findings; it does not authorize fixes, pushes, replies, or deployment.
- Report actual delivery state: local draft, published draft, ready for review, merged, or deployed. Link the PR and name material verification gaps rather than calling unfinished work complete.

## Optional CodeRabbit Babysitting

Only when explicitly asked to publish and babysit, follow [the bounded babysitter procedure](references/coderabbit-babysitter.md). Keep the PR draft, verify findings rather than trusting bot labels, preserve published history, and never respond to human review threads without permission. Do not trigger bot review or enroll automation merely because a draft exists.

See [provenance](references/provenance.md) when revising this workflow or checking stack-tool assumptions.
