---
name: matt-verify
description: "Requires fresh evidence for completion claims and exercises the real changed surface. Use before saying code is fixed, working, complete, clean, or ready for review."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Verify

No completion claim without fresh evidence from the current state.

## Verification Gate

For each claim:

1. Name the command, observation, or artifact that would prove it.
2. Run or inspect it freshly after the relevant changes.
3. Read the complete result needed to establish exit status, failures, and scope.
4. Compare the evidence with the exact claim.
5. Mark the claim `proven`, `failed`, or `inconclusive`.

Do not use one evidence type to imply another:

- Formatting does not prove lint or type safety.
- Typechecking does not prove runtime behavior.
- Unit tests do not prove build, integration, or requirement completeness.
- A build does not prove the user-visible feature.
- Green CI does not prove checks cover the changed requirement.
- A subagent report does not prove its artifacts are correct.

## Derive the Ladder

Read repository scripts and CI, then run the relevant subset in increasing breadth:

1. Formatter or changed-file diagnostics.
2. Linter.
3. Typecheck.
4. Focused tests for changed behavior.
5. Broader package or repository tests.
6. Build or generated-artifact validation.
7. Real-surface exercise.

Do not run irrelevant expensive checks for ceremony. Keep skipped levels visible with a reason.

## Real-Surface Proof

Exercise the surface the user or another system consumes whenever practical:

- API: route, middleware, auth, serialization, and representative request/response.
- UI: running browser flow and visible state, not only component tests.
- CLI: actual invocation, output, exit code, and side effects.
- Database: migration or query against the intended local/test database and read back resulting state.
- Generated output: generate, parse or consume, and compare meaningful invariants.

If the correct surface cannot be exercised, report verification as inconclusive rather than substituting a convenient lower-level check.

## Bug Fixes

- Re-run the original reproduction after the fix.
- Preserve evidence that the reproduction was red before the fix when available.
- A regression test should be capable of failing without the fix. Demonstrate this through prior failure or a safe temporary comparison when practical; never destroy unrelated work to manufacture proof.

## Requirements Audit

Check every acceptance criterion and named non-goal against current evidence. Passing tests do not prove an omitted criterion.

## Receipt

Report compactly:

```text
Claim | Evidence | Result
```

Include exact commands, exit status or meaningful counts, the real surface exercised, and anything skipped or inconclusive. Never hide failing checks behind a broad success summary.
