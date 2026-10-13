---
name: matt-verify
description: "Gathers fresh evidence for changed behavior and diagnoses gaps in verification. Use when asked to verify work or when deciding how to prove a consequential change; ordinary Matt Mode edits still need proportionate checks without loading this skill."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
---

# Matt Verify

Prove the claim being made, not whatever is easiest to test. Gather evidence without repairing failures unless implementation or fixes are authorized.

Read repository scripts, CI, and relevant tests. Choose the smallest set of checks that can distinguish correct behavior from a plausible wrong implementation. Run them against the current state, inspect the results, and rerun affected checks after fixes. No mandatory check ladder or skipped-level receipt.

Static checks, unit tests, builds, and runtime exercise prove different things. Neither green CI nor another agent's report establishes behavior it did not exercise. Compare evidence against the requested outcome, including compatibility and failure paths where relevant.

## Exercise the Consumed Surface

- API: representative requests through routing, auth, and serialization.
- UI: browser interactions and affected states using `/agent-browser`; inspect captured visuals when appearance changes.
- CLI: actual invocation, output, exit status, and side effects.
- Database: local/test migration or query with resulting state read back.
- Generated output: generate and parse or consume it, checking meaningful invariants.
- LLM prompts: a representative eval or model run. Prompt substring assertions do not prove extraction behavior.

For bugs, reproduce before editing when possible and rerun the original case after the fix. A regression check must be capable of failing without the fix. Preserve unrelated work during comparisons.

Prefer existing tooling and disposable probes. Do not retain a one-off validation script merely because it produced evidence. Keep tests that protect consumer behavior, not private structure or incidental wording.

Report exact commands or interactions, observed results, and material limitations concisely. Use a table only when it helps. If the real surface could not run, say what the lower-level checks establish and leave the behavioral claim inconclusive. Never hide failed checks behind a broad success summary.
