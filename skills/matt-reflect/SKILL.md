---
name: matt-reflect
description: "Turns repeated steering, user corrections, and failed quality gates into proposed workflow improvements. Use after a correction, recurring failure, substantial task, or explicit request to improve Matt Mode."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Reflect

Extract durable workflow improvements without turning one unusual task into permanent policy.

## Scope

Use the current workspace conversation and its artifacts. Do not mine unrelated repositories, sessions, or personal data.

When the user identifies a live Hunk review as evidence, follow `/hunk-review` to read user-authored comments from that session. Do not generalize agent-authored comments or silently treat every inline suggestion as a standing preference.

Trigger on:

- Explicit user reflection request.
- User correction or repeated manual steering.
- A review or verification failure that exposed a reusable gap.
- A substantial successful task with a clearly reusable method.

Do not reflect after every trivial task.

## Analyze the Failure or Success

1. Identify the concrete moment, expected behavior, actual behavior, and impact.
2. Decide whether the issue was:
   - A skill failed to trigger or the router chose the wrong playbook.
   - Workflow overhead, subagents, or verification exceeded the task's demonstrated effort or risk.
   - Instructions were absent, ambiguous, contradictory, or too buried.
   - Review failed to catch a quality problem.
   - Verification proved the wrong claim or wrong surface.
   - Repository context was unavailable or not inspected.
   - Architecture, API shape, data structures, or types allowed an avoidable invalid state or operation.
   - Compilation, lint, tests, scripts, runtime checks, or CI should enforce it.
   - A one-off preference that should not persist.
3. Seek contradictory examples before generalizing.

## Durability Filter

Recommend persistence only when the lesson is likely to remain useful for at least six months and is specific enough to change a future decision. Prefer repeated evidence; allow one high-impact failure or one explicitly stated standing preference when the invariant is clear.

For every accepted candidate, evaluate remedies in this order and stop at the earliest level that adequately controls the problem:

1. **Impossible by construction:** improve ownership, architecture, API shape, data structure, state model, or types.
2. **Mechanical enforcement:** compiler, formatter, lint, test, deterministic script, runtime validation, or CI.
3. **Guidance and placement:** amend an existing skill or rule at the narrowest scope that reaches every intended consumer.
4. **Manual review:** add a review check only when earlier levels cannot adequately encode the judgment.

For level 3, consult the shared `skill-placement` skill when available:

- Stable guidance useful in multiple Purelend repositories belongs in the shared plugin at `~/Repos/agents/skills/`. That plugin accepts skills, not shared rules or `.mdc` files.
- Repository architecture, stack, or workflow guidance belongs in that repository.
- Personal, experimental, or temporary guidance belongs in `~/.agents/skills/matt-*`.
- When a team-wide candidate is difficult to upstream, personal guidance may incubate it immediately. Mark the duplication and remove the personal copy if the shared version becomes canonical.
- Prefer amending an existing skill over adding a near-duplicate. Add a new skill only for a distinct reusable workflow.
- Drop guidance that is already enforced, already covered, too vague, contradictory, or one-off.

## Proposal

Return at most a few items:

```text
Observation:
Evidence:
Elimination level:
Placement:
Proposed change:
Why it is durable:
How to pressure-test it:
```

Distinguish `accept`, `reject`, and `backlog` candidates. Prefer editing an existing skill over creating another.

For a shared Purelend promotion candidate, also provide:

```text
Affected repositories or teams:
Why design or CI cannot fully enforce it:
Existing shared skill to amend, or why a new skill is warranted:
Immediate personal incubation, if any:
Evidence required before upstreaming:
```

## Approval and Pressure Test

Do not modify persistent skills, personal preferences, repository rules, tooling, or `~/Repos/agents` during reflection without explicit approval. Approval to update the personal stack does not authorize changing, committing, pushing, or opening a pull request in the shared team plugin.

For an approved skill change:

1. Recreate or describe a compact scenario that previously produced the failure.
2. Record the expected behavior.
3. Make the smallest instruction or trigger change that addresses it.
4. Re-evaluate the scenario and one nearby counterexample.
5. Keep the change only if it fixes the failure without creating broader ceremony or conflict.
