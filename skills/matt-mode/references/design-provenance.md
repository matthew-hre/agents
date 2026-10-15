# Design Provenance

Read this only when changing the Matt workflow.

The stack deliberately adapts these public practices:

- Cursor P-Stack: real-surface proof, aggressive removal of narration comments, and adjudicated independent review.
  - https://github.com/cursor/plugins/tree/main/pstack
- Matt Pocock's `code-review`, `diagnosing-bugs`, and `retro`: review of intent and standards, red-capable bug reproduction, and routing lessons to checks or tooling.
  - https://github.com/mattpocock/skills
- Obra's Superpowers `verification-before-completion`, `requesting-code-review`, and `writing-skills`: fresh evidence before completion claims, implementer reports treated as untrusted, and RED-GREEN-REFACTOR pressure testing for skill changes.
  - https://github.com/obra/superpowers

The original router, preflight receipts, and five playbooks were removed after Cursor sessions showed conversational questions turning into formal workflows and routine tasks spawning unnecessary agents. Keep one independent reviewer at the end of substantial implementation to check personal, team, and project guidance and major correctness gaps. Small edits and questions stay direct; reflection is requested rather than automatic.

The October 2026 consolidation uses Matthew's `codebase-standards.md`, `pr-standards.md`, `venn.md`, and `pre-pr-review-draft` as directional research, not universal policy or proof of preventable review volume. Matt Mode owns implementation quality and handoff; Matt Review owns review and verification; Matt PR owns publication and optional stacking; Matt Reflect remains opt-in. The retired preflight entry was removed and standalone verification became a reference.

The Purelend expectations reference is personal incubation, pending any separately approved team adoption. Keep current team/repository policy authoritative. Do not restore hard diff-size gates, diff-only reviewers, blanket test-per-branch requirements, or automatic stop-hook review loops. These add ceremony or false assurance without establishing correctness.

Avoid importing these practices wholesale:

- Mandatory routes, receipts, and skipped-stage lists.
- Multi-model panels for every change.
- Automatic persistent edits during reflection.
- Contradictory policies to both continue autonomously and stop on every ambiguity.
- Aggressive restructuring without intent, evidence, or repository support.
