---
name: matt-preflight
description: "Establishes effective repository policy, intent, ownership, risk, and verification before coding. Use at the start of Matt Mode implementation, debugging, investigation, or review."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Preflight

Produce a compact execution contract before substantial work. Do the discovery; do not make the user restate facts available in the repository.

## 1. Select Operation Mode

Choose one:

- `plan-only`: investigate and propose; do not edit.
- `execute`: make reversible local changes and verify them.
- `review-only`: inspect and report; do not edit.
- `verify-only`: gather evidence; do not repair failures unless asked.

Infer this from the request. Ask only if the distinction changes the outcome.

## 2. Resolve Effective Policy

Before editing:

1. Inspect repository and working-tree state. Preserve unrelated user or agent changes; do not require a clean tree merely for convenience.
2. Find every applicable `AGENTS.md` from repository root to planned files.
3. Read relevant repository guidance, ADRs, package manifests, CI, and test configuration.
4. For Cursor repositories, apply every `alwaysApply` rule and every scoped rule whose globs match files likely to be touched. Recheck when the file set changes.
5. Identify installed specialist skills whose trigger matches the task. For Purelend work, include applicable team-wide plugin skills; do not duplicate their workflow in Matt Mode.
6. Inspect one or two neighboring implementations and relevant tests. Treat sibling-repository conventions only as hypotheses.
7. Derive commands from the current repository; do not rely on remembered script names.

List only policies that can change this task. Do not dump every instruction into context.

## 3. Frame Intent

State:

- Observable outcome.
- Acceptance criteria.
- Explicit non-goals and scope boundary.
- Product decisions already settled.
- Material uncertainties.

Investigate empirical uncertainty. Ask about genuine product choices that cannot be recovered from the ticket, code, or authoritative docs.

## 4. Map the Change

Identify:

- Owning module and layer.
- Entry points and callers.
- Input, domain, persistence, and output shapes.
- Trust, authorization, process, network, and storage boundaries involved.
- Existing tests and real runtime surface.

For a bug, define a deterministic reproduction before accepting a causal theory.

## 5. Classify Effort and Risk

- Effort `trivial`: wording, typo, formatting, or tightly scoped semantic-preserving rename.
- Effort `low`: localized mechanical code change following an established pattern.
- Effort `normal`: meaningful behavior, multiple coupled files, new logic, or a new local seam.
- Risk `high`: authorization, security, money, migrations, destructive data, concurrency, shared contracts, public APIs, infrastructure, or difficult-to-reverse architecture.

Effort controls workflow overhead. Risk can override effort and controls review depth and verification breadth. File count alone decides neither.

Do not use a subagent for trivial or low-effort work. Escalate only when direct evidence reveals behavior risk, ambiguity, or wider blast radius.

## 6. Define Proof

Map each acceptance claim to evidence. Include the real surface when behavior changes: live API request, browser flow, CLI invocation, migration/query, generated artifact, or equivalent.

## 7. Plan Reviewable Changes

When the work contains more than one coherent unit, follow `/matt-pr` before implementation and include an ordered local change stack. Each unit needs one responsibility, its dependency, an observable result, and focused proof. Do not defer decomposition until the final diff is already large.

## Output

Keep the receipt short:

```text
Mode / route / effort / risk:
Outcome and non-goals:
Effective policy:
Ownership and affected boundaries:
Plan:
Local change stack, when needed:
Proof:
Open product decisions:
```

Begin work immediately after the receipt unless a product decision or external side effect requires approval.
