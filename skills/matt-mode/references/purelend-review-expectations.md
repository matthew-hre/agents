# Purelend Review Expectations

Read alongside current team skills and repository guidance when implementing or reviewing Purelend changes. This is a personal synthesis of the October 2026 codebase/review research, not a replacement for team policy or a frozen description of every repository. If shared guidance owns an expectation, use that source rather than maintaining a competing rule here.

Apply only the sections relevant to the change. Historical review comments identify questions worth checking; they do not establish universal bans or justify unrelated cleanup.

## Ownership and Reuse

- Trace the repository's actual ownership path. In layered backends, controllers parse and map transport inputs/outputs, services own domain workflows and resource authorization, and repositories own persistence. External integrations belong behind the established client or adapter boundary.
- Use the existing composition root and dependency lifetimes. Do not introduce module-level clients or instantiate service dependencies inside controllers merely to bypass wiring. Check wiring when registrations change.
- In frontend code, keep components, hooks, pure helpers, and data access in their established homes. Use the repository's data-access path rather than adding a parallel fetch or route layer.
- Search for existing operations, schemas, factories, and installed library helpers before creating another version. Lodash and date-fns are common choices, not mandatory substitutes for clear language primitives.
- In legacy repositories such as lia-uo and lia-api, nearby code can reveal contracts without establishing the desired style. Follow applicable current policy for new and materially changed code; preserve compatibility and leave unrelated debt alone.

## Types and Tests

- Parse untrusted requests, upstream responses, configuration, and job payloads at the boundary using the repository's validation conventions. Reuse schemas and derive types rather than duplicating them.
- Avoid `any`, unjustified assertions, and non-null assertions. Prefer narrowing, exhaustive maps, and source-owned types. Do not ban legitimate `as const` or add runtime checks to appease already-proven internal types.
- Cover changed consumer behavior and meaningful empty, null, bound, failure, and retry cases. Choose inputs where the likely mistake produces a different result; do not equate one test per syntactic branch with correctness.
- Use the repository's test tools and factories. For hooks, exercise hook behavior; for UI, use user-level interactions rather than DOM-structure assertions. Mock external boundaries, not the unit's decisions.
- For LLM extraction changes, use representative evals or model-facing runs. Prompt substring assertions do not establish extraction quality. Migration checks should prove compatibility and resulting data/schema behavior, not migration-file wording.

## Trust Boundaries and Domain Behavior

- Check server-side tenant/resource authorization and the relevant HTTP status semantics. Do not expose raw upstream bodies, SQL, paths, or exception messages to users. Use the repository's shared error classes and vetted product copy.
- Check timeouts, retry classification, and idempotency when the change touches remote work. Validation failures should not become retry storms. Keep secrets and sensitive data out of logs, URLs, and client cache keys.
- Derive money, rate, income, tax, and date behavior from the ticket or authoritative domain contract. Check rounding, period boundaries, and tie-breaking; use worked examples as tests where available. Do not invent a missing business decision.
- For migrations and live product configuration, inspect the actual rollout/versioning policy. Preserve deployed consumers, version immutable live configs, and separate migration publication when required. Do not split by technical layer if that leaves an intermediate state broken.

## UI, Copy, and Publication

- Use the repository's design system, tokens, and shared primitives. Fix genuinely shared behavior at its owner rather than copying components into a feature.
- In localized applications, use the established translation API, named formats, and ICU messages. Cover the supported locales, avoid sentence fragments assembled in code, and check language quality rather than just key existence.
- Exercise affected UI states and inspect captured visuals for direct or indirect appearance changes. PRs show uploaded before/after images or videos in a table; a capture alone does not prove the interaction works.
- Keep PRs coherent and easy to review. Size is a warning, not a hard line-count gate. Include required changesets/generated contracts and relevant boundary/rollout rationale. Perform verification without adding routine test receipts to the PR body.
- Triage bot findings against current code and intent. Neither a bot severity label nor a completed bot review establishes correctness.

## What Review Caught in November 2026

Across 35 lender-portal PRs, nearly every substantive finding came from two review bots, and most fell into a few groups. Check for these before a reviewer does; the [state and failure checklist](state-and-failure-checklist.md) turns them into questions.

- **Test hygiene, the most frequent finding:** an expected value that repeats an input literal, hand-built fixtures where a factory exists, `fireEvent` where the rules ask for `userEvent.setup()`, hook behavior proved through a component, and a test that passes before the outcome it asserts.
- **State around writes:** selection or dialog state acted on after it changed, callbacks that touch more than what was submitted, invalidation on success only, and unguarded cache writes.
- **Partial failure and limits:** a loop of writes that fails midway without saying which, request bodies past the contract's limit, and unbounded memory or parallelism.
- **Viewer and state coverage:** a missing forbidden state, copy naming an action the viewer lacks, other breakpoints, and accented data.
- **Sibling parity:** a new page, route, error, or switch that omits what the repository's skill or the neighboring file does.
- **Drift:** a rule file, ticket link, or copy string that no longer matches the code, and a refactor commit that changes behavior.

Reviewers on the team catch little that these checks do not, with one exception: behavior carried over from lia-uo. When porting a feature, read the original.

Personal naming, signature, and comment rules live in [personal preferences](personal-preferences.md). Verification procedure lives in [verification guidance](../../matt-review/references/verification.md).
