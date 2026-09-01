---
name: matt-review
description: "Reviews a code diff independently for intent, logic, repository fit, names, comments, tests, and unnecessary complexity. Use before completion in Matt Mode or when asked to review a change or pull request."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Review

Review artifacts, not the author's confidence. Be read-only unless the user or Matt Mode explicitly moves accepted findings into a fix stage.

## Preserve Independence

When called from an implementation session, delegate the formal review to a fresh read-only subagent when the environment supports it. Give that reviewer the package below, not the implementation conversation or the author's defense. The parent agent adjudicates findings and owns any fixes.

For high-risk work, use a second independent reviewer focused on the highest-risk invariant. Do not multiply reviewers for routine changes.

If a fresh reviewer is unavailable, run the same procedure as self-review and label the result `not independent`; never imply separation that did not occur.

## Establish Review Package

Pin:

- Review target and merge base or file scope.
- Original request, ticket, spec, and non-goals.
- Applicable `AGENTS.md`, scoped rules, ADRs, and neighboring idioms.
- Diff and changed tests.
- Verification evidence gathered so far.

If operating as an independent reviewer, do not rely on the implementer's prose rationale. Treat it as an unverified claim.

## Review on Two Separate Axes

### Intent and Correctness

- Does every acceptance criterion have an implementation path?
- Is there scope creep or omitted behavior?
- Trace concrete execution paths for success, empty input, boundaries, failures, retries, stale state, idempotency, and concurrency when relevant.
- Are authorization and trust checks performed at the correct boundary?
- Do contracts, runtime schemas, domain types, persistence, and output mappings agree?
- Does the test fail for the behavior it claims to protect, or could it pass while the feature is broken?

Do not report speculative logic bugs without a plausible input and path to the failure.

### Quality and Repository Fit

- **Names:** flag mysterious, generic, misleading, or abstraction-leaking names. Prefer precise domain terms.
- **Function signatures:** flag ambiguous positional arguments: three or more parameters, multiple parameters sharing a type, and positional optional or boolean flags. Prefer a named-argument object unless distinct types and natural ordering make the call self-explanatory.
- **Comments:** flag narration, section labels, stale claims, and comments that compensate for unclear code. Preserve irreducible why-context.
- **Control flow:** flag hidden fallbacks, broad catches, impossible-state guards, repeated conditionals, and branches that obscure the invariant.
- **Types:** flag `any`, unjustified casts, duplicated runtime/static shapes, missing exhaustiveness, and primitive values standing in for meaningful domain states.
- **Structure:** flag pass-through wrappers, speculative abstractions, mixed responsibilities, misplaced logic, shotgun changes, and duplicate sources of truth.
- **Scope:** flag unrelated cleanup and compatibility code with no demonstrated consumer.
- **Tests:** flag white-box assertions, excessive mocking, missing negative cases, and tests that merely repeat implementation.
- **Policy:** verify applicable repository rules directly rather than relying on memory.

## Findings Contract

Every blocking or important finding must include:

- Severity: `blocking`, `important`, or `minor`.
- File and line or exact symbol.
- Violated requirement, invariant, or repository convention.
- Concrete impact or failure path.
- Direction for correction, without requiring a particular patch shape when alternatives exist.

Separate optional improvements from findings. Do not inflate style preference into correctness severity.

For a recurring failure class, recommend remediation in this order: make it impossible through architecture, data structures, API shape, or types; enforce it through compiler, lint, tests, scripts, runtime checks, or CI; encode guidance at the appropriate shared, repository, or personal scope; rely on review only as a backstop. Do not demand new infrastructure for a one-off issue when the cost exceeds the demonstrated risk.

## Verdict

Report the two axes separately:

```text
Intent: pass | changes requested | inconclusive
Quality: pass | changes requested | inconclusive
Confidence: high | medium | low
Ready for teammate PR review: yes | no | inconclusive
```

Then list findings in priority order. If none exist, say what was inspected and name the most important residual risk; do not invent a finding to appear thorough.

After fixes, re-review the affected diff and nearby integration points rather than restarting an unfocused review of the whole repository.
