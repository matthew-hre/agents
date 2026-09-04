---
name: matt-pr
description: "Structures substantial work into atomic local changes and focused single or stacked GitHub pull requests. Use during Matt Mode planning when work has multiple coherent units, or when asked to prepare, open, review, update, restack, or maintain a pull request."
compatibility: Cursor and Agent Skills-compatible coding agents with Jujutsu, GitHub CLI, filesystem, and shell access.
---

# Matt PR

Design for the reviewer before the diff becomes large. A stack is a sequence of coherent, green changes—not a giant change split by file count after implementation.

## Safety and Authority

1. If `.jj/` exists, follow `/jujutsu` before any VCS command. Use JJ for local history; do not substitute Git staging, checkout, amend, rebase, reset, or stash commands.
2. Inspect current state before planning: working-copy change, ancestors since trunk, descriptions, bookmarks, and existing pull requests.
3. Use JJ change IDs for stable local identity and bookmarks for GitHub branch identity.
4. Local `jj describe` and `jj new` checkpoints are part of an authorized Matt Mode implementation. Creating or moving bookmarks, pushing, opening/updating PRs, and rewriting public changes require explicit user approval.
5. Preserve unrelated work. Never force a clean tree or fold another person's changes into the stack.

## Design the Change Stack Before Coding

For work with more than one coherent unit, produce:

```text
Unit | Depends on | Observable result | Responsibility/files | Focused proof | PR boundary?
```

Choose boundaries by reviewer independence:

- One unit has one responsibility and can be explained in one sentence.
- A reviewer can approve or reject it without mentally reconstructing later units.
- Keep implementation and its behavior-focused tests together. Every proposed PR prefix must be safe to merge and deploy while all descendant PRs are absent: the application remains functional, existing user paths do not break, and the layer delivers a coherent result or a deliberately dormant capability.
- Prefer vertical tracer bullets. Use foundation-only units when later work genuinely depends on a reusable schema, type, migration, or compatibility seam.
- For broad migrations, prefer expand → migrate consumers in coherent batches → contract. Keep every ancestor green.
- Fold setup, generated output, and documentation into the unit they enable unless independently useful.
- Size is a warning, not the definition of atomicity. When a unit gains a second concern, stop and create the next unit before editing it.

A pull request may contain multiple atomic commits when they form one small reviewer narrative. Use a stacked PR boundary when a dependency is independently reviewable and mergeable, when ownership/reviewers differ, or when the cumulative diff would be daunting.

For changes spanning database, backend, and frontend, prove the actual rollout states rather than splitting by file or technical layer:

- A database layer must remain compatible with the currently deployed backend. Destructive contract steps wait until all consumers have migrated.
- A backend layer must work with the currently deployed frontend and must not require an unmerged descendant to preserve existing behavior.
- A frontend layer must degrade safely until its required backend is deployed; use an existing feature-gating or compatibility pattern when exposure must wait.
- If an intermediate state cannot be made functional without speculative scaffolding, combine those changes into one PR boundary instead of publishing a broken stack layer.

## Build the Local JJ Stack Incrementally

1. Start the first unit in an empty working-copy change. Describe that change before editing; if the current change is not empty, inspect it and do not pile new work onto unrelated or already-complete work.
2. Implement only the current unit.
3. Run its focused proof and inspect its complete diff.
4. Confirm the description is honest, the change contains no unrelated concern, and the resulting tree is buildable.
5. Create the next described JJ change before starting the next unit.
6. Repeat. Run broader checks at each proposed PR tip and the full relevant suite at the stack tip.

Use `jj absorb` only to move a later local correction into the mutable ancestor that owns those lines. It is cleanup, not the primary decomposition strategy. Inspect the resulting operation and reverify affected changes.

## Verify Every PR Prefix Independently

Do not infer that lower layers work because the stack tip is green. Before publication or declaring the stack reviewable:

1. Materialize or otherwise execute against the exact resulting tree at each proposed PR tip, from root to top.
2. At each tip, run the repository's required build/type/static checks, that layer's focused tests, and the smallest real-surface exercise that proves the observable result.
3. Test relevant upgrade and coexistence states explicitly—for example, migration plus old backend, new backend plus old frontend, and new frontend against the available backend contract.
4. Record evidence per PR tip. A check run only at the final stack tip is evidence only for that tip.
5. If a prefix fails or depends on a descendant, repair the compatibility boundary, move the dependent work into the owning layer, or combine the PRs. Do not waive the failure as something a later PR fixes.

Verification may reuse deterministic commands, but each command must run against each prefix it claims to prove. Restore the working copy to the intended stack tip after prefix verification and confirm its state before continuing.

If work has already accumulated in one large change:

- Prefer non-interactive `jj split` with explicit filesets and `-m` only when responsibility boundaries align cleanly with files.
- Do not launch an interactive diff editor from an unattended agent.
- When concerns are intertwined within files, first map the intended units and use safe JJ-native history editing with explicit user oversight rather than guessing through destructive Git commands.

## Publish a Draft Pull Request

Create every pull request as a draft unless the user explicitly asks to request teammate review now. For a single PR, use `gh pr create --draft` with an agent-authored title and `--body-file`; do not rely on generated commit summaries. For a stack, preserve the draft default for every layer.

Write for a reviewer deciding whether the change is necessary and correct, not for the implementer remembering which files changed:

- Use a concise, intent-focused title that follows repository conventions.
- Read and link the source ticket. State the user or business problem it establishes.
- Explain the observable outcome and the important design choice. Omit routine implementation narration and file inventories.
- Name material non-goals, compatibility constraints, rollout concerns, or follow-up work only when they affect review.
- Never claim evidence that was not gathered, and never publish secrets, credentials, customer data, or sensitive terminal output.

Use this body shape, omitting sections that truly do not apply:

```markdown
## Why

<Ticket link and the problem or requirement it establishes.>

## What

<Observable outcome and intentionally included scope.>

## Approach

<Important design decisions and why this approach was chosen.>

## Stack

<This PR's position, dependencies, merge/deploy order, and adjacent PR links.>

## Verification evidence

<Screenshots, recordings, representative sanitized requests/responses, schema output, or other direct evidence.>

## Test plan

- [x] <A user-visible or system behavior that was actually exercised.>
- [ ] <A relevant scenario not exercised — include the concrete blocker or required environment.>

## Automated checks

- `<command>` — passed

## Generated summary

@coderabbitai summary
```

The test plan is a behavioral checklist, not a command transcript. Write concrete stories, interactions, failure cases, compatibility states, and rollout states that establish the feature works. Check an item only after performing that exact scenario. Put lint, type-check, build, and automated test commands under **Automated checks** with their observed result; never publish unchecked placeholders such as `- [ ] lint passes`.

Choose evidence that exposes the changed surface:

- UI: exercise representative flows and states in a browser; attach inspected screenshots or a short recording.
- API or CLI: include a sanitized representative invocation and the response or output that proves the claim.
- Database or infrastructure: include relevant migration/schema/plan output or a compact table/data-flow diagram, plus compatibility evidence for intermediate rollout states.
- Nonvisual internal logic: cite the focused behavioral test or runtime exercise; do not manufacture a screenshot merely to decorate the PR.

Use `/github-image-upload` when local screenshots, recordings, logs, or other artifacts must be embedded in the description. Verify every uploaded artifact renders for an authorized reviewer, every link resolves, and the final Markdown body is readable before reporting the PR as prepared.

Treat **Stack** as a dependency map, not only a GitHub feature. Within one repository, link each stacked PR to its parent and child. Across microservices or repositories, list every related PR in required merge and deployment order, identify which PR is currently being read, and state the compatibility contract between steps. A cross-repository chain cannot be represented by `gh stack`; maintain its links and order explicitly in every PR description.

## Map Local Changes to Pull Requests

Choose deliberately:

- **One PR:** one focused reviewer narrative, even if it contains several atomic commits.
- **Stacked PRs:** two or more dependent, independently reviewable and mergeable layers.

For a stack:

1. Derive the actual trunk/base from repository policy; do not assume `main`. Purelend feature work may target `preview`.
2. Put foundational changes lower and dependents higher.
3. Use one bookmark per PR tip. The root PR targets trunk; each child targets the bookmark immediately below it.
4. Assign one topology owner. Only that coordinator moves bookmarks, rebases descendants, retargets PRs, or updates the remote stack.
5. In JJ-managed repositories, prefer GitHub's `gh stack link --base <trunk> <bottom> ... <top>` integration after the bookmarks are ready. Do not use Git-oriented `gh stack init`, `modify`, `rebase`, or `sync` to manage local history alongside JJ.
6. `gh stack` is a public-preview interface. Read current command help before acting. Linking may push branches and create or update PRs, so require approval immediately before running it.
7. Create every layer as a draft. Use `--open` only when the user asks to request review and every layer has its required evidence.

Each PR description should state:

- Its position and dependency in the stack.
- What this layer changes, excluding lower layers.
- Why the boundary exists.
- How this layer was verified.
- What reviewers should focus on.
- Links to adjacent PRs when GitHub does not render the stack clearly, including cross-repository dependencies and required merge/deployment order.

## Maintain Public Pull Requests

Treat local and public history differently:

- **Before publication:** freely refine mutable local changes with JJ when it improves atomicity; reverify affected units.
- **After publication:** never rewrite reviewed changes silently. Default to a clearly described fix commit for each coherent feedback batch so reviewers can inspect the delta.
- Put a fix on the PR layer that owns the concern. Updating a lower layer may require rebasing descendants and refreshing their evidence.
- After feedback is resolved, ask whether to keep fix commits for auditability or fold them into their owning changes before landing. Folding or rebasing invalidates old commit IDs and potentially prior CI/review evidence; rerun affected checks and report new bookmark tips.
- Merge stacks bottom-up unless GitHub's stack merge operation is explicitly chosen. After a lower PR lands, verify the remaining topology and bases before continuing.

Do not use vague commits such as `address review comments` when a precise description such as `Handle expired sessions during report export` is available.

## Babysit CodeRabbit on a Draft

When the user explicitly asks to publish and babysit a PR, follow [`references/coderabbit-babysitter.md`](references/coderabbit-babysitter.md). Enrollment is opt-in; do not trigger CodeRabbit merely because a draft exists. CodeRabbit review commands must be posted through a shell GitHub credential verified as the user's identity; Cursor's bot-authored PR-comment tool is not sufficient. Keep the PR draft throughout the loop, triage findings rather than trusting them, push only verified fixes within the approved scope, and notify the user instead of marking the PR ready.

The babysitter must operate on the exact current HEAD, avoid duplicate or full-review requests, stop after its bounded retry budget, and treat rate limits, ambiguous findings, failed checks, concurrent human pushes, and unavailable environments as reasons to stop and notify—not reasons to guess or force progress. It must never respond to human review comments. On every wake, absence of the enrollment label is an immediate permanent stop; terminal flows cancel subscriptions before removing that label.

## Readiness Gates

Before proposing any push or PR action, report:

```text
Local change stack:
Per-PR-prefix verification and standalone rollout evidence:
Proposed PR topology and bases:
Public-history rewrites, if any:
Unresolved review findings:
External actions awaiting approval:
```

Do not call a stack reviewable merely because it has several commits. Inspect each commit range and each proposed PR diff.

See `references/provenance.md` when revising this workflow.
