# State and Failure Checklist

Answer these for the change before handoff and before publication. They come from defects reviewers caught in November 2026 that a generic "trace the failure paths" instruction did not prevent. Skip a question only when the change cannot affect it. Answer from the code or by exercising the surface, not from memory of what you intended.

## Writes and Pending Work

- While this write is pending, what can the user still do (select more rows, open another dialog, navigate, retry)? Do the success and failure callbacks act only on what was submitted, or on whatever is current when they run?
- If the request fails after the server changed state, or the third of five sequential writes fails, what does the user see and what is left stale? Invalidate on settle, not only on success, and report which part failed.
- Does every branch that writes the cache guard against an in-flight read overwriting it? When one branch gains a guard, check the others.
- Can a read reused from before the previous write feed the next write?

## Selection, Dialogs, and Derived State

- Does a confirmation act on the set it displayed, or on a live list that can change while it is open? Snapshot what it acts on.
- Can stored open or selected state outlive the thing it refers to (a row that moved tables, a selection that emptied, a breakpoint that changed)? Prefer deriving it.
- Does anything close, navigate, or move focus before the operation it belongs to has resolved?

## Limits and Contracts

- For each request body, read the generated contract's `maxItems`, `max`, and character rules. What happens one past the limit? Client validation matches the service's rules.
- What is the worst-case size, count, and duration (memory held, parallel requests, signed-URL lifetime)?
- If a required field or endpoint is new, does this still work against the currently deployed producer or consumer?

## Each Viewer and Each State

- Walk each permission that gates the surface: forbidden state, hidden controls, and copy that names an action the viewer may not have.
- Empty, loading, error, and long-content states, in each supported locale. Copy states facts (limits, file types) from the constant that owns them, not from a hardcoded string.
- Other viewports and breakpoints, a blocked popup, signed-out, and keyboard-only use. An ARIA role promises the keyboard behavior that goes with it.
- Accented and non-Latin data wherever the change searches, sorts, truncates, or names files. Do not replace a primitive's locale-aware behavior with a hand-rolled one.

## Shared Code and Data Shapes

- When a shared component, hook, or helper changes behavior or defaults, list its callers and check each one, including surfaces this task did not set out to touch.
- Deduplicating before aggregating drops values: check what `uniqBy` discards when two entries share the key and differ elsewhere.
- When a feature is ported from lia-uo or another repository, read the original for behavior that is easy to drop: retries, filename cleaning, limits.

## Tests

- Break the code the test claims to cover and confirm the test fails. A test that asserts before the awaited outcome passes vacuously.
- Bind each input once and assert against the binding. Build fixtures with the repository's factories. Test hook behavior through the hook, and user behavior through user-level events.
- A behavior change, including one made while fixing a review finding, has a test that pins the new behavior.
