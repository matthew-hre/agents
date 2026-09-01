# Review Only

1. Pin the exact review target: merge-base diff, pull request diff, working-tree diff, commit range, or named files.
2. Establish the intended behavior, ticket/spec, non-goals, and applicable repository policy.
3. Follow `/matt-verify` in read-only mode to gather fresh evidence for the changed claims.
4. Follow `/matt-review` without modifying files.
5. Validate suspected logic defects through concrete execution paths and available tests rather than pattern matching alone.
6. Report prioritized findings and an explicit teammate-PR-readiness verdict.
7. Keep suggestions outside the requested scope separate from blocking findings.
