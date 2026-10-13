---
name: matt-reflect
description: "Proposes improvements to code guidance, skills, tooling, and environments from demonstrated friction. Use when explicitly asked to reflect or improve the workflow; offer concrete remedies during or after costly execution friction without automatically running a retrospective."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Reflect

Learn from friction experienced by both the user and the agent, not every imperfection an agent notices. Adapt to corrections immediately; persistent guidance is a separate decision. Suggest concrete improvements during or after costly execution friction, and run a full reflection when asked.

Use the current conversation and its artifacts, plus other sessions the user asks you to inspect. Use `/hunk-review` for identified live Hunk feedback. Distinguish user-authored preferences from agent suggestions and one-off fixes.

Find the concrete mismatch between expected and actual behavior. Check whether an existing instruction was ignored before proposing another one. Seek contradictory examples; one repeated problem, high-impact failure, or explicitly stated standing preference can justify a durable change.

## Improve the Working Conditions Too

Look beyond code corrections: repeated shell workarounds, brittle scripts, missing dependencies, unreliable startup, unavailable test data, access problems, and awkward browser or service verification can reveal a better way to work.

Distinguish a broken environment from missing knowledge or a missing reusable capability. Prefer fixing the setup or using an existing tool/skill over documenting a workaround. Amend an existing skill when it owns the workflow; propose a new skill when a distinct, reusable procedure is being rediscovered. Retain a script only when deterministic automation has an ongoing use.

At session end, or while a blocker is consuming time, name the observed cost and the smallest improvement that would remove it. For example: “I repeatedly rebuilt test data by hand; a repository verification skill with a fixture command would make this flow repeatable.” Do not offer generic reflection or invent an improvement when the session was smooth.

## Prefer Elimination Over More Instructions

Evaluate remedies in the user's order:

1. Make the issue impossible through ownership, architecture, API shape, data structures, or types.
2. Enforce it mechanically through compiler, lint, tests, scripts, validation, or CI.
3. Amend existing guidance at the right scope.
4. Use manual review as a backstop.

Stop at the earliest adequate remedy. Do not build infrastructure for a one-off issue, or add prose that repeats existing policy.

For guidance placement, consult the shared `skill-placement` skill when available. Purelend-wide guidance belongs in the shared plugin at `~/Repos/agents/skills/`; local contracts belong in the repository; personal or experimental preferences belong in the personal skills. Personal incubation may precede team adoption, but remove duplication when shared guidance becomes canonical.

## Propose Less

Return only the few changes that would have mattered: the observed friction, evidence, proposed remedy and placement, and a scenario that could disprove its usefulness. Say when no persistent change is warranted. No accept/reject/backlog ledger or mandatory proposal template.

Do not edit persistent skills, rules, preferences, or tooling without explicit approval. Personal-stack approval does not authorize changes to the shared team plugin.

For an approved edit, compare the prior failure scenario and a nearby counterexample against the revised instructions. Distinguish an instruction walkthrough from an executed agent trial. Keep the change only if it addresses the failure without introducing new ceremony or weakening needed safeguards.
