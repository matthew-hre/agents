# Matthew's Engineering Preferences

These are personal defaults. Repository policy and deliberate local architecture take precedence.

## Eliminating Recurring Issues

When a defect, correction, or review concern could recur, prefer remedies in this order:

1. **Impossible by construction:** change ownership, architecture, API shape, data structure, state model, or types so the invalid operation or state cannot be represented.
2. **Mechanical enforcement:** make compilation, lint, tests, deterministic scripts, runtime validation, or CI reject the issue.
3. **Guidance at the right scope:** amend an existing shared team skill, repository skill/rule, or personal skill. Prefer shared Purelend guidance for stable behavior useful across repositories, repository guidance for local architecture, and personal guidance for personal or experimental preferences.
4. **Manual review:** use agent, Hunk, and teammate review only for judgment that cannot yet be encoded earlier.

Do not jump directly to prose guidance when design or automation can eliminate the failure class. Manual review is a backstop, not the primary control.

## Clarity and Naming

- Use precise domain names that reveal what a value represents and why an operation exists.
- Avoid vague names such as `data`, `result`, `item`, `value`, `handler`, `manager`, or `utils` when a more honest domain name exists.
- Name booleans as propositions and collections as plural domain nouns.
- Keep names at the abstraction level of their owner. Difficulty naming a unit is evidence that its responsibility may be mixed.
- Do not create trivial aliases or intermediate variables that merely repeat an expression without adding meaning.

## Visual Chunking

Blank lines are a readability tool for human scanning. Prefer visual separation so related declarations and logic chunks are obvious at a glance.

- Prefer a blank line between sibling overloads and between dense top-level type or interface aliases that are not a tight related pair.
- Prefer a blank line between logical phases inside a function (setup, transform, side effect, return) when the body is more than a few lines.
- Do not fight the formatter. Keep blank lines the formatter preserves; do not invent spacing rules the formatter will collapse.
- Do not insert blank lines inside a short interface or type body, or between every single-line statement in a tiny function.

## Function Signatures

- Use one named-argument object when a function takes three or more arguments, two or more arguments share a type, or an argument is optional, boolean, or rarely supplied.
- Name the argument type after the operation when a named type improves reuse or readability, for example `CreateReportArgs` for `createReport`.
- Keep positional arguments when their types are unambiguously distinct and their order reads naturally, such as `(request, response)` or `(error, value)`.
- Apply this to new and materially changed APIs. Do not churn unrelated stable call sites solely to satisfy the preference.
- Consider stronger domain types in addition to named arguments when values with the same runtime representation remain easy to confuse.

## Logic and Types

- Make state and invariants explicit. Prefer discriminated unions, validated boundary schemas, exhaustive handling, and domain-owned types over scattered booleans, casts, and duplicated shapes.
- Validate untrusted input at boundaries; do not add defensive checks for states already made impossible by trusted internal contracts.
- Trace empty, boundary, failure, retry, idempotency, stale-state, and concurrency behavior when relevant.
- Prefer direct control flow over hidden fallbacks, catch-all branches, and abstraction layers that only forward calls.
- Fix ownership and root causes rather than adding one-off wrappers or patches around the wrong layer.

## Comments

Default: no comments. When unsure, delete. Do not shorten a restating comment to keep it.

Keep only:

- Legal or license headers.
- A constraint forced by an external dependency, platform, vendor, or protocol that this code cannot reshape. Link the issue, RFC, or docs when practical.
- Formatter escapes such as `prettier-ignore`. Lint or TypeScript suppressions only when the rule is wrong, pedantic, or style-only. If the rule catches a real bug, remove the suppression and fix the code or the type.
- Public API TSDoc only when the signature cannot express the contract consumers must rely on: conditionally required arguments, thrown errors, side effects, or trust boundaries.
- A link to an issue or RFC that explains a constraint the code cannot express.

Delete narration, restated names, section banners, commented-out code, TODO/FIXME notes, workaround sermons, and `IMPORTANT` / `do not remove` justifications. A public JSDoc whose first sentence paraphrases the symbol name is narration.

If a comment exists to explain surprising behavior in code we own, do not keep the comment. Rename, extract, type, or restructure until the behavior is obvious.

## Scope and Architecture

- Put behavior in the layer that owns it. Keep transport mapping, domain workflow, persistence, and presentation concerns distinct where the repository follows those boundaries.
- Reuse established abstractions. Add a new abstraction only when it removes real complexity, has genuine reuse, or establishes a meaningful boundary.
- Do not create shared packages or generic helpers speculatively.
- Keep changes scoped to the requested outcome, while following necessary cleanup through callers when the change makes code dead or invalid.
- Write a published-package README as what the next caller does. Do not introduce a name unless the caller must type it.
- If a default already applies, keep the helper private unless a second caller must name it. Do not export constants that exist only to document the default.

## Contracts and Isolation Tests

- A forbid-import or module-graph test records current isolation. It is not the source ADR. When it conflicts with the ADR, project guidelines, or a required shared error type, re-read the isolation sentence. Do not invent a parallel type to keep the test green.

## User-Visible Errors

- Prefer stable, product-owned toast and alert copy over appending raw `error.message` (or similar) from API, Zod, or thrown errors.
- We often do not know what those messages look like in every failure path, and they can expose cryptic text or implementation details to end users.
- Put diagnostic detail in logs or developer-facing surfaces. Surface a specific message in the UI only when that feature already uses a vetted, user-safe string for that path.

## Documentation Register

- When editing an existing README or operator doc section, match that section's density and term precision.
- Do not rewrite neighboring prose into marketing-simple language or into a jargon pile.
- A greenfield doc may set its own voice; do not force an existing section to change register for polish alone.

## History After a Pull Request Exists

- Before a pull request is public: absorb and squash locally so the stack is one concern per commit.
- After the pull request is public: put review fixes and follow-up work in new commits on top. Reviewers read those commits. Do not rewrite pushed commits with absorb, squash, or edit unless the user explicitly asks.

## Tests and Evidence

- Test public behavior rather than private implementation details.
- Mock external boundaries, not the unit's own decisions.
- A test should be capable of failing for the defect or behavior it claims to cover.
- Static checks, unit tests, builds, and runtime exercise prove different claims. Report them separately.
