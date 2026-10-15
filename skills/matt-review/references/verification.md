# Verification Guidance

Prove the claim being made, not whatever is easiest to test. Gather evidence without repairing failures unless implementation or fixes are authorized.

Read repository scripts, CI, and relevant tests. Choose the smallest set of checks that can distinguish correct behavior from a plausible wrong implementation. Run them against the current state, inspect the results, and rerun affected checks after fixes. No mandatory check ladder or skipped-level receipt.

When a `.jj` file or directory exists, use `jj` and run repository pre-commit checks manually; JJ will not invoke Git hooks. Use explicit changed-file/tree inputs for staged-file-dependent checks and inspect resulting edits before reporting readiness.

Static checks, unit tests, builds, and runtime exercise prove different things. Neither green CI nor another agent's report establishes behavior it did not exercise. Compare evidence against the requested outcome, including compatibility and failure paths where relevant.

Track which checks actually ran against which revision. A skipped test, an empty test selection, or an unavailable dependency is not a passing behavioral check. Related-test selection may omit new tests or cross-module effects; inspect its coverage and run the relevant explicit tests or broader suite when needed. Legacy repositories do not get a silent exemption from type or behavior verification.

## Exercise the Consumed Surface

- API: representative requests through routing, auth, and serialization.
- UI: browser interactions and affected states using `/agent-browser`; inspect target-branch and candidate captures for direct or indirect visual changes. PRs show uploaded before/after images or videos in a table.
- CLI: actual invocation, output, exit status, and side effects.
- Database: local/test migration or query with resulting state read back.
- Generated output: generate and parse or consume it, checking meaningful invariants.
- LLM prompts: a representative eval or model run. Prompt substring assertions do not prove extraction behavior.

For benchmarks, run the target-branch baseline and PR candidate with the same workload and environment. Report before/after measurements in a table with units; never use an intermediate candidate as the baseline or invent missing measurements.

For stacked PRs, exercise each proposed PR tip independently and check the deployed consumer combinations that matter. Evidence from the stack tip alone does not prove that a lower PR is safe to merge.

For bugs, reproduce before editing when possible and rerun the original case after the fix. A regression check must be capable of failing without the fix. Preserve unrelated work during comparisons.

Prefer existing tooling and disposable probes. Do not retain a one-off validation script merely because it produced evidence. Keep tests that protect consumer behavior, not private structure or incidental wording.

Report exact commands or interactions, observed results, and material limitations concisely. Use a table only when it helps. If the real surface could not run, say what the lower-level checks establish and leave the behavioral claim inconclusive. Never hide failed checks behind a broad success summary.

Keep raw output for diagnosis when useful. Report verification to the user as needed, but keep routine test receipts and validation sections out of PR bodies. Published comparisons and samples should be concise and sanitized; follow Matt PR for presentation. Do not hide failures or material uncertainty.
