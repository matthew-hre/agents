# Bug Fix

1. Define the exact observed symptom and user-facing surface.
2. Reproduce it with one deterministic, red-capable command or interaction before naming a cause. If reproduction is impossible, keep the diagnosis conditional.
3. Minimize the reproduction when that helps isolate the fault, but retain the original reproduction for final proof.
4. Trace the failing path and generate falsifiable hypotheses. Change one meaningful variable at a time.
5. Add or improve a regression check at the public behavior boundary when practical.
6. Fix the root cause at its owning layer. Remove code made dead or redundant by that fix without adding unrelated cleanup.
7. Run diff hygiene, `/matt-verify`, and `/matt-review`.
8. Re-run the original, unminimized reproduction after review fixes.

Do not call a plausible explanation the root cause until it predicts and resolves the observed failure.
