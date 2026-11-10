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

## Publication Gate

"Open a PR" is the moment review friction is decided. Before creating a PR or pushing to one, each of these holds for the exact tree being published, or the user has waived it by name in this conversation. This applies when this skill is the first one loaded in the session and when the session wrote no code of its own.

1. **Matt Mode is in force.** If the session has not loaded `/matt-mode`, load it and read its references now.
2. **The base is current.** Fetch, and build the tree on the current target branch before running checks. A branch cut before a new locale, rule, or contract landed passes locally and fails CI.
3. **An independent review covers this tree.** Run `/matt-review` with a fresh reviewer unless one already reviewed these exact revisions. Edits, scope changes, a restack or split, and review fixes made since the last review are not covered by it. For a stack, each layer is reviewed as it would merge alone.
4. **No fix-before-publishing finding is open.** Every finding from that review, and from any earlier review in the session, is fixed, declined with a reason, or accepted by the user as a named known limit. "The reviewer found no blockers" does not satisfy this.
5. **The consumed surface was exercised on this tree,** including the states the [state and failure checklist](../matt-mode/references/state-and-failure-checklist.md) raises, not only the happy path used for screenshots. If the environment blocks it, say what would unblock it and ask whether to wait or publish with the surface marked unverified.
6. **Nothing is waiting on the user.** If the session's handoff listed open decisions, known gaps, or unverified surfaces that the user has not answered, list them in one short message and ask before publishing. A request to open a PR that arrives right after such a handoff has not answered them.

When an item fails and the user is not present, stop with the tree ready and the list of what is open. Do not publish and note the gap afterwards.

## Verify Before Publication

Inspect the actual PR delta against its intended base, including new files—not just the last commit or all ancestors of a stacked branch. Apply `/matt-review` for the relevant scope and use its [verification guidance](../matt-review/references/verification.md). Read current package scripts, CI, and repository guidance instead of assuming commands from another repo.

When splitting into a stack, check each layer's own delta: no export, prop, or parameter whose only caller is in a later layer, and the test for a layer's headline behavior lives in that layer.

Run the relevant checks against the exact tree being published and inspect their results. Include required changesets and regenerated contracts. For UI appearance changes, inspect rendered captures; for other behavior, exercise the consumed surface. Check deployment compatibility when contracts or schema change. Reverify affected behavior after fixes; a prior green result does not cover new edits.

JJ does not run Git pre-commit hooks automatically. Inspect the repository's hooks and run their checks manually before treating a change as ready. If a hook depends on Git's staged-file list, run its underlying commands against the actual changed files/tree instead of staging files to satisfy it. Reinspect any generated or formatted changes. If CI checks individual commits, verify those commits too; never rewrite open-PR history to run a fixer.

## Write for the Reviewer

- Don't write essays. Use a concise, intent-focused title and body; use bullet points for the text you do write. Respect mandatory repository requirements without adding template boilerplate.
- Explain the final aggregate change against the target branch, or the final ordered stack when relevant. Do not narrate intermediate commits, earlier implementations, discarded approaches, or how much you shrank the PR. Review-fix commits do not need their own story.
- Prefer useful Mermaid codeblock diagrams, code samples/snippets, and code references over paragraphs. Samples may show internals or caller usage. Choose the forms that explain this change; do not add a diagram merely to fill a section.
- Omit routine test/validation receipts, command lists, "I ran tests," and generic test-plan sections. Still perform verification and report material failures or gaps to the user; concise PR writing is not permission to hide a blocker.
- When any exist, add a short "Deliberate choices and known limits" list: where the change departs from the ticket, from a repository rule's pattern, or from a sibling's behavior, and why; limits the user accepted; behavior changes a reader would not expect from the title; surfaces not exercised. Reviewers ask about each of these when the body is silent, and each question is a round trip. Omit the section when there is nothing to list.
- Use the repository's title convention; in Purelend repositories that is a lowercase Conventional Commit subject.
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
- Review fixes go through the publication gate like the first push. Run the repository's full check, format, and test commands on the whole tree, not on a filtered file list. A fix that restructures code or changes behavior gets a fresh reviewer and, for UI, a browser pass on the affected states.
- Each push to a PR that is ready for review dismisses approvals and starts another bot round. Collect the open comments from every reviewer, fix them together, and push once. Ask before pushing a partial round unless the user asked for that specific fix now.
- When fixing review comments in a session that did not write the change, first read the PR body, the full diff, and earlier review threads, including findings that were fixed differently or declined, so a fix does not undo a deliberate choice.
- Verify a reviewer's or bot's suggested code before applying it. When a suggestion conflicts with a repository rule or another reviewer, report the conflict to the user rather than alternating between them.
- Unless the user opts out, watch the PR for new comments/reviews and CI workflow completions on the exact head commit. Use the harness's event subscriptions or channels when available; otherwise poll on an interval (for example `/loop` or a background `gh` watcher). Refresh the CI watch after an authorized push. Monitoring reports findings; it does not authorize fixes, pushes, replies, or deployment.
- Report actual delivery state: local draft, published draft, ready for review, merged, or deployed. Link the PR and name material verification gaps rather than calling unfinished work complete.

See [provenance](references/provenance.md) when revising this workflow or checking stack-tool assumptions.
