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

1. Restate the observable outcome and important non-goals. Do not merely repeat the proposed implementation.
2. Follow `/matt-preflight` to establish effective policy, ownership, risk, and proof before editing.
3. Select exactly one primary route:
   - Read-only explanation or investigation: `playbooks/investigate.md`
   - Why an artifact exists or whether to keep, move, replace, or delete it: `/codebase-archaeology`
   - Defect or regression: `playbooks/bug-fix.md`
   - Feature or refactor: `playbooks/feature-refactor.md`
   - Review without implementation: `playbooks/review-only.md`
   - Uncertain feasibility or technology question: `playbooks/spike.md`
4. State the route and risk in one line.
5. For nontrivial work, create a visible todo list from the selected playbook. Keep omitted stages visible as `skip: <specific reason>`.

If the task does not fit, construct the smallest workflow that preserves the gates below. Do not force it into the closest playbook.

## Specialist Skills

Matt Mode orchestrates applicable specialist skills; it does not absorb or replace them.

- Use an installed specialist skill when its trigger matches more precisely than a generic playbook. Examples include `codebase-archaeology`, `verify-findings`, `jujutsu`, `interface-review`, `hunk-review`, and framework-specific skills.
- For Purelend work, include relevant team-wide skills from the installed Purelend plugin. When its source checkout exists at `~/Repos/agents`, treat `skills/skill-placement/SKILL.md` as the authority for deciding whether reusable guidance belongs in the shared plugin, one repository, deterministic enforcement, personal incubation, or nowhere shared.
- Follow specialist constraints inside the selected playbook. A read-only or approval-gated specialist remains read-only or approval-gated.
- Prefer amending an applicable existing skill over creating an overlapping one.

## Required Gates

For code changes, completion normally requires:

1. **Grounding:** inspect ownership, callers, data shapes, nearby patterns, and relevant history when intent is unclear.
2. **Implementation:** make the smallest complete change that satisfies the outcome; do not optimize for a tiny diff when it would leave a broken boundary or dead code.
3. **Diff hygiene:** inspect the actual diff for accidental scope, weak names, narration comments, unsupported guards, casts, duplication, and repository-rule violations.
4. **Initial proof:** follow `/matt-verify` against the changed surface.
5. **Independent review:** follow `/matt-review`. The reviewer must judge requirements and artifacts, not trust the author's summary.
6. **Adjudication:** classify each finding as `fix`, `consider`, or `dismiss`, with evidence. Fix confirmed issues; do not blindly apply reviewer patches.
7. **Reverification:** rerun checks affected by review fixes and repeat focused review when behavior or architecture changed materially.

Low-risk mechanical work may combine gates, but never silently omit applicable policy, diff inspection, or fresh verification.

## Review Independence

- Prefer one read-only independent reviewer with clean context for normal work.
- Use two independent reviewers or model families only for security, authorization, money, migrations, concurrency, shared protocols, subtle state logic, or genuinely contested architecture.
- Give reviewers the intent, pinned diff, applicable rules, and verification evidence. Do not give them the author's defense of the implementation.
- If independent review is unavailable, perform the structured review anyway and disclose that it was self-review.

## Hunk Feedback

When the user says they left feedback in a live Hunk session:

1. Follow `/hunk-review` to read user-authored inline comments and map them to the current diff. Do not clear or rewrite their comments unless asked.
2. Treat each comment as evidence to inspect, not a patch instruction to apply blindly. Classify it as `fix`, `ask`, or `dismiss` for the current change.
3. Address confirmed current-change issues, run affected verification, and report the disposition of every comment.
4. Follow `/matt-reflect` for comments that express a recurring preference, expose a missed existing policy, or identify a reusable failure class. One comment can justify a proposal when the user explicitly says it is a standing preference, but persistence still requires approval.

## Autonomy

Proceed through reversible local investigation and edits without asking for routine approval. Ask when the answer changes product behavior, crosses an irreversible or shared-state boundary, needs unavailable credentials, or cannot be established from authoritative evidence.

Do not commit, push, open or update pull requests, edit tickets, message teammates, deploy, migrate shared data, or change shared infrastructure unless the user requests that action.

## Finish

Report:

- What changed and why.
- The independent review verdict and any dismissed findings that matter.
- Fresh verification evidence, including the real surface exercised.
- Anything incomplete, inconclusive, or intentionally skipped.
- `Ready for teammate PR review: yes | no | inconclusive`, with the reason.

Do not say "done," "fixed," "working," or "passes" more broadly than the evidence proves.

Use `/matt-reflect` after explicit user correction, repeated steering, a failed quality gate, or when the user asks to improve the workflow. Reflection never edits persistent skills without approval.
