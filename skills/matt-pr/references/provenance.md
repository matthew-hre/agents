# PR Workflow Provenance

Read this when revising Matt PR or checking tool assumptions.

## Authoritative sources

- GitHub stacked pull requests: https://docs.github.com/en/pull-requests/get-started/about-stacked-prs
- GitHub integration with Jujutsu and other tools: https://docs.github.com/en/pull-requests/reference/use-other-tools-with-stacked-pull-requests
- Jujutsu GitHub workflow and review updates: https://docs.jj-vcs.dev/latest/github/
- Jujutsu bookmarks: https://docs.jj-vcs.dev/latest/bookmarks/

GitHub's stack feature and `gh stack` CLI are public preview. Verify current behavior with official documentation and local `gh stack --help` before remote operations.

## Adapted workflow practices

- P-Stack plans one independently evidenced change per PR and uses root-to-trunk, child-to-parent topology.
- Obra's Superpowers puts a commit and verification gate at each implementation-task boundary and keeps review fixes in bounded, reverified rounds.
- Matt Pocock's ticketing guidance favors vertical, independently demonstrable tracer bullets and expand–migrate–contract for broad changes.
- Jujutsu encourages creating the commit stack before bookmarks and supports either visible review-fix commits or deliberate rewriting according to project preference.

The personal workflow combines these practices but keeps JJ as the sole local history owner. It does not transplant Git staging, interactive rebase, `HEAD~N`, force-push, or branch-checkout instructions into colocated JJ repositories.
