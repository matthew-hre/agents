# Feature or Refactor

1. Convert the request into observable acceptance criteria and explicit non-goals.
2. Trace the current ownership path, callers, data shapes, and integration boundaries.
3. When introducing or moving a seam, changing a shared contract, or making a choice expensive to reverse, follow `/codebase-design` (deep modules; Design It Twice when the placement is contested). Otherwise record why the established pattern is sufficient.
4. If the work contains multiple coherent units, follow `/matt-pr` now and define the ordered local change stack before editing. Do not use `jj absorb` as the primary decomposition plan.
5. For a refactor, preserve behavior first and prove it before intentional behavior changes.
6. Implement one coherent responsibility at a time using existing repository patterns. Keep its behavior-focused tests in the same unit.
7. Before starting the next unit, inspect and verify the current unit, give its JJ change an honest description, and create the next change. Every proposed PR prefix must remain independently functional, mergeable, deployable, and understandable without any descendant; follow `/matt-pr` to verify each prefix against its exact resulting tree.
8. Inspect the complete diff, not only edited snippets, for accidental scope, quality smells, comments outside the keep-list, and exported symbols that only exist to name a default. For a published package, write the README as a walkthrough for the next caller, not an architecture diagram.
9. Run `/matt-verify`, then apply Matt Mode's proportional review policy.
10. Fix confirmed findings, perform focused re-review when warranted, and reverify affected claims.
11. When the user asks to open or prepare pull requests, follow `/matt-pr` to map the verified local changes to one focused PR or a dependent GitHub stack.
