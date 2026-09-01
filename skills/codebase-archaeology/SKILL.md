---
name: codebase-archaeology
description: >-
  Investigate why code exists before changing it. Trace call sites, history,
  and intent; map keep/move/replace/delete options with risks; recommend one;
  stop without editing unless explicitly asked. Use when the user asks why
  something exists, what it does, whether it can be removed/replaced/cleaned
  up, if something is low-hanging fruit, or where a type/helper/path is used —
  not when implementing a known ticket, verifying review findings, or running a
  forward-looking feature spike.
---

# Codebase Archaeology

Curiosity and cleanup prompts are **investigation requests**, not refactor permission. Dig, label, report — do not rearrange the site until the curator says so.

Default stance: the weird code is guilty of looking weird, not of being wrong.

## Workflow

### 1. Identify the artifact

Pin down exactly what is under study: file, symbol, type, module, pattern, or folder slice. If the user pointed at a dumping ground (`lib/`, a grab-bag utils file), ask which corner to dig first — or pick the smallest concrete suspects and say so.

### 2. Trace current use

Evidence over vibe:

1. **Call sites / importers** — who references it, from where, how often.
2. **Registration & indirection** — DI containers, routers, barrels, stringly names, dynamic import, config maps, reflection-ish lookup. "No references" is not "dead" until these are checked.
3. **Tests & fixtures** — production-only vs test-only life.
4. **Neighbors** — the idiomatic replacement that already exists (or doesn't).

Prefer searching by symbol and path over trusting file names or comments.

### 3. Recover intent

Why it was shaped this way:

- Recent blame / history for the artifact (jj or git — use jj when `.jj/` exists).
- Nearby ADR, ticket, PR description, or comment that explains a non-obvious constraint.
- Boundaries it enforces (auth, tenancy, sync vs async, client/server).

If intent stays opaque, say **unknown** — do not invent a story.

### 4. Map options (do not implement)

Present concrete options. Typical set:

| Option | Meaning |
|---|---|
| **Keep** | Still earns its place; leave it. |
| **Move** | Right idea, wrong home (e.g. contract vs `lib/`, system vs request repo). |
| **Replace** | Superseded by an existing abstraction; migrate callers. |
| **Delete** | Provably unused or pure dead weight after the checks above. |
| **Extract** | Mixed responsibilities; split before any removal. |

For each option you seriously consider: **cost**, **risk**, **blast radius** (callers, PRs, behavior). Drop options that are fantasy refactors.

### 5. Recommend one — then stop

- Pick a single recommendation with a one-line why.
- List **open questions** only if they block the recommendation.
- **Do not edit** unless the user explicitly asks to apply (e.g. "do it", "apply the replace", "delete it").
- "Low-hanging fruit" means *candidates to discuss*, not *start cleaning*.

## Report shape

Keep it short:

```text
## Artifact
<what> — <one-line current job>

## Evidence
- Call sites: …
- Hidden life: … (DI/router/dynamic — or "none found")
- History/intent: … (or "unknown")

## Options
| Option | Cost | Risk | Notes |
|---|---|---|---|
| Keep | … | … | … |
| … | … | … | … |

## Recommendation
**Replace** (or Keep/Move/Delete/Extract) — <one line why>

Awaiting go-ahead before any edits.
```

## Hard rules

- Never delete or "clean up" as part of the investigation.
- Never expand into a repo-wide tidy pass from one suspicious file.
- Never treat unused-export warnings or a single grep miss as proof of death.
- Never propose a new abstraction when a local, existing pattern already fits (**Replace** / **Move** beat **Invent**).
- If the user also wants a forward plan for a feature, hand off to a research spike — this skill stays on the artifact.

## Anti-patterns

- Rewriting while explaining.
- Answering "why does this exist?" with a patch.
- Declaring victory after finding one alternative helper without mapping callers.
- Recommending delete because the design is ugly.
- Quietly "also fixing" neighbors discovered during the dig.
