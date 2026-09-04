---
name: matt-mode
description: "Runs Matthew's personal engineering workflow for investigation, implementation, review, and verification. Use only when explicitly invoked with /matt-mode."
compatibility: Cursor and Agent Skills-compatible coding agents with filesystem and shell access.
disable-model-invocation: true
icon: code
color: purple
---

# Matt Mode

Run a proportional, evidence-driven coding workflow. Remain in Matt Mode for the conversation until the user asks to leave it.

## Authority

Apply instructions in this order:

1. The user's requested outcome, constraints, and explicit decisions.
2. Repository `AGENTS.md`, applicable Cursor rules, ADRs, and documented contracts.
3. Current code, tests, CI, and neighboring implementations.
4. `references/personal-preferences.md` as defaults, never as a reason to fight intentional repository design.
5. General engineering conventions.

Call out a material conflict instead of silently choosing the lower-priority instruction.

## Start Every Task

1. Check for the trivial fast path: wording, typo, formatting, or tightly scoped semantic-preserving rename with no behavior or contract change. For these, inspect the exact scope, apply relevant policy, edit directly, inspect the diff, run the cheapest direct proof, and report. Do not produce a preflight receipt, todo list, subagent, architecture pass, or formal review.
2. Otherwise, restate the observable outcome and important non-goals. Do not merely repeat the proposed implementation.
3. Follow `/matt-preflight` to establish effective policy, ownership, effort, risk, and proof before editing.
4. Select exactly one primary route:
   - Read-only explanation or investigation: `playbooks/investigate.md`
   - Why an artifact exists or whether to keep, move, replace, or delete it: `/codebase-archaeology`
   - Defect or regression: `playbooks/bug-fix.md`
   - Feature or refactor: `playbooks/feature-refactor.md`
   - Review without implementation: `playbooks/review-only.md`
   - Structure, open, or maintain commits and pull requests: `/matt-pr`
   - Uncertain feasibility or technology question: `playbooks/spike.md`
5. State the route, effort, and risk in one line.
6. For nontrivial work, create a visible todo list from the selected playbook. Keep omitted stages visible as `skip: <specific reason>`.

If the task does not fit, construct the smallest workflow that preserves the gates below. Do not force it into the closest playbook.

## Specialist Skills

Matt Mode orchestrates applicable specialist skills; it does not absorb or replace them.

- Use an installed specialist skill when its trigger matches more precisely than a generic playbook. Examples include `codebase-design`, `codebase-archaeology`, `verify-findings`, `jujutsu`, `interface-review`, `hunk-review`, and framework-specific skills.
- Use `/codebase-design` when introducing or moving a seam, splitting a mixed file, deepening a shallow module cluster, or choosing where an interface should live. Prefer its vocabulary (`module`, `interface`, `seam`, `adapter`, `depth`, `leverage`, `locality`) and run Design It Twice when the seam placement is contested or expensive to reverse. Do not run it as ceremony on routine feature work that follows an established pattern.
- For Purelend work, include relevant team-wide skills from the installed Purelend plugin. When its source checkout exists at `~/Repos/agents`, treat `skills/skill-placement/SKILL.md` as the authority for deciding whether reusable guidance belongs in the shared plugin, one repository, deterministic enforcement, personal incubation, or nowhere shared.
- Follow specialist constraints inside the selected playbook. A read-only or approval-gated specialist remains read-only or approval-gated.
- Prefer amending an applicable existing skill over creating an overlapping one.

## Required Gates

For code changes, completion normally requires:

1. **Grounding:** inspect ownership, callers, data shapes, nearby patterns, and relevant history when intent is unclear. Treat a forbid-import or module-graph test as current isolation, not as the source contract. When it conflicts with the ADR, project guidelines, or a required shared error type, re-read the isolation sentence.
2. **Implementation:** make the smallest complete change that satisfies the outcome; do not optimize for a tiny diff when it would leave a broken boundary or dead code.
3. **Diff hygiene:** inspect the actual diff for accidental scope, weak names, comments outside the keep-list in `references/personal-preferences.md`, unsupported guards, casts, duplication, dense declaration clusters that lack blank-line separation when personal preferences require it, and repository-rule violations. Do not add a comment during implementation unless it matches that keep-list.
4. **Initial proof:** follow `/matt-verify` against the changed surface.
5. **Proportional review:** use inline diff review for trivial and low-effort work. Follow `/matt-review` with an independent reviewer for normal and high-risk work. The reviewer must judge requirements and artifacts, not trust the author's summary.
6. **Adjudication:** classify each finding as `fix`, `consider`, or `dismiss`, with evidence. Fix confirmed issues; do not blindly apply reviewer patches.
7. **Reverification:** rerun checks affected by review fixes and repeat focused review when behavior or architecture changed materially.

Trivial and low-effort work may combine gates, but never silently omit applicable policy, diff inspection, or fresh proportionate verification.

## Proportional Agent Use

- **Trivial:** wording, typo, formatting, or a tightly scoped semantic-preserving rename with no contract or behavior change. Work directly. Inspect the diff and run only the cheapest check that proves the edit. Do not create a subagent, todo plan, architecture pass, or formal review.
- **Low:** localized mechanical code change following an established pattern with no sensitive boundary. Work directly. Perform inline diff review and targeted verification. Do not create a review or verification subagent.
- **Normal:** meaningful behavior, multiple coupled files, new logic, or a new local seam. Use one read-only independent reviewer with clean context after initial verification.
- **High risk:** security, authorization, money, migrations, concurrency, shared protocols, destructive operations, or difficult-to-reverse architecture. Use one independent reviewer; add a second only for a distinct unresolved invariant.
- Give reviewers the intent, pinned diff, applicable rules, and verification evidence. Do not give them the author's defense of the implementation.
- If independent review is unavailable, perform the structured review anyway and disclose that it was self-review.
- Never spawn a subagent merely because a gate exists. Escalate a trivial or low task only when direct inspection discovers behavior risk, ambiguity, or a wider blast radius; state the evidence that changed the classification.

## Hunk Feedback

When the user says they left feedback in a live Hunk session:

1. Follow `/hunk-review` to read user-authored inline comments and map them to the current diff. Do not clear or rewrite their comments unless asked.
2. Treat each comment as evidence to inspect, not a patch instruction to apply blindly. Classify it as `fix`, `ask`, or `dismiss` for the current change.
3. Address confirmed current-change issues, run affected verification, and report the disposition of every comment.
4. Follow `/matt-reflect` for comments that express a recurring preference, expose a missed existing policy, or identify a reusable failure class. One comment can justify a proposal when the user explicitly says it is a standing preference, but persistence still requires approval.

## Autonomy

Proceed through reversible local investigation and edits without asking for routine approval. Ask when the answer changes product behavior, crosses an irreversible or shared-state boundary, needs unavailable credentials, or cannot be established from authoritative evidence.

In a Jujutsu repository, an implementation request authorizes local `jj describe` and `jj new` checkpoints needed to keep completed units in separate reviewable changes. Follow `/jujutsu` and `/matt-pr`, preserve unrelated work, and never rewrite a pushed change silently. This does not authorize bookmarks, pushes, pull requests, or other remote actions.

Outside that JJ checkpoint exception, do not commit. Never push, create or move remote bookmarks, open or update pull requests, edit tickets, message teammates, deploy, migrate shared data, or change shared infrastructure unless the user requests that action.

## Finish

Report:

- What changed and why.
- The review level used—inline or independent—its verdict, and any dismissed findings that matter.
- Fresh verification evidence, including the real surface exercised.
- Anything incomplete, inconclusive, or intentionally skipped.
- `Ready for teammate PR review: yes | no | inconclusive`, with the reason.

Do not say "done," "fixed," "working," or "passes" more broadly than the evidence proves.

Use `/matt-reflect` after explicit user correction, repeated steering, a failed quality gate, or when the user asks to improve the workflow. Reflection never edits persistent skills without approval.
