# Feature or Refactor

1. Convert the request into observable acceptance criteria and explicit non-goals.
2. Trace the current ownership path, callers, data shapes, and integration boundaries.
3. When introducing or moving a seam, changing a shared contract, or making a choice expensive to reverse, follow `/codebase-design` (deep modules; Design It Twice when the placement is contested). Otherwise record why the established pattern is sufficient.
4. For a refactor, preserve behavior first and prove it before intentional behavior changes.
5. Implement one coherent responsibility at a time using existing repository patterns.
6. Add behavior-focused tests for meaningful new or changed logic.
7. Inspect the complete diff, not only edited snippets, for accidental scope, quality smells, comments outside the keep-list, and exported symbols that only exist to name a default. For a published package, write the README as a walkthrough for the next caller, not an architecture diagram.
8. Run `/matt-verify`, then `/matt-review`.
9. Fix confirmed findings, perform focused re-review when warranted, and reverify affected claims.
