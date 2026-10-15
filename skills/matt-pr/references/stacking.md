# Independently Mergeable PR Stacks

Read when a change needs dependent PRs or when maintaining an existing stack. A stack is not a giant diff divided by file count. Each PR must be understandable, verifiable, and safe to deploy without its descendants.

## Choose Boundaries Before Coding

Identify each unit's observable result, dependencies, owning files, focused proof, and proposed PR boundary. Explain the boundaries briefly; use a table only when it helps.

- Prefer vertical slices. A foundation-only unit is justified when it provides a reusable schema, type, migration, or deliberately dormant capability needed by later work.
- Keep behavior and its tests together. Include generated output and documentation in the unit they enable unless independently useful.
- Database changes must support the currently deployed backend. A new backend must support the deployed frontend. A frontend must remain safe until its required backend is available.
- Use established compatibility or feature-gating patterns when necessary. If a prefix cannot work without speculative scaffolding, combine the dependent units rather than publishing a broken layer.
- For broad migrations, expand → migrate consumers → contract. Destructive steps wait until all consumers have migrated.

## Build and Verify Incrementally

Use `jj` if a `.jj` file or directory exists, otherwise Git. Inspect the current working copy before creating a described change for the first unit; preserve unrelated work. Implement and verify that unit, then create the next described change before starting the next unit. With JJ, run repository pre-commit checks manually and use change IDs for stable local identity. Freely refine your local sequence before PRs are open; never rewrite shared history without authorization.

Before publishing or declaring the stack reviewable:

1. Execute against the exact tree at every proposed PR tip, from bottom to top. Run required build/static checks, focused tests, and the smallest real-surface exercise that proves that layer's result.
2. Exercise relevant coexistence states: migration with old backend, new backend with old frontend, or feature exposure against the available contract. Record evidence per PR tip; stack-tip evidence proves only the tip.
3. If a prefix depends on a descendant, repair the compatibility boundary or combine the PRs. Do not waive failure because a later PR fixes it.
4. Restore the intended working-copy tip and confirm its state after verification.

When a large change already exists, inspect its responsibilities before splitting. Use supported non-interactive JJ operations with explicit selections and messages. Never launch an unattended interactive editor, discard unrelated changes, or substitute destructive Git history commands. Read current command help rather than guessing flags.

## Publish the Topology

- Derive the target branch from repository policy and the actual PR. Do not assume `main`; Purelend feature work may target `preview`.
- Use one bookmark per proposed PR tip. The root targets trunk; each child targets the bookmark directly below it. Review each PR against its own base, not trunk for every layer.
- Assign one topology owner. Only that coordinator moves bookmarks, retargets PRs, or reconciles dependent history.
- Inspect current `gh stack` help before using it. In JJ repositories, use compatible linking of prepared bookmarks rather than Git-oriented commands that manage local history. A link command may push or create/update PRs: ensure those exact effects are authorized first.
- Create every layer as a draft unless the user asks to request review. Describe the final layer's scope, adjacent PR links, and required merge/deployment order using the concise body guidance in Matt PR. Do not narrate how the stack was rearranged or add routine test receipts.
- Across repositories, maintain dependency links and deployment order explicitly; GitHub stack metadata is not a cross-repository rollout plan.

## Maintain Published Layers

Once a PR is open, its existing commits are immutable. Put review fixes on the layer that owns the concern, in new commits. Do not rebase dependent open-PR branches to refresh the stack or absorb into a reviewed ancestor. Integrate required parent updates without rewriting existing PR commits; ask if a repository constraint prevents that. Multiple review-fix commits are acceptable; only an explicit user override permits rewriting open-PR history.

Merge bottom-up when authorized. After a lower layer lands, inspect the remaining PR bases and topology before continuing. Refresh exact-head CI subscriptions and evidence after authorized changes. Never infer lower-layer safety from a green stack tip.
