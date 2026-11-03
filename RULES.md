# Agent Rules

These rules are workspace-wide. Repository-specific facts and procedures belong in the owning repository, its CI, or the knowledge base.

## Authority and scope

- A request to review, diagnose, explain, or report authorizes inspection and reporting, not edits, commits, pushes, deployments, or fixes unless the user also asks for them.
- Resolve repository boundaries with `git rev-parse --show-toplevel` and confirm the active checkout; never infer ownership from directory names or the mere presence of `.git`.
- For mutating work, fetch the remote default branch and use a dedicated branch or worktree based on its current tip. On an existing pull request, stay on its branch and do not rebase or force-push shared history unless the user requests it and the ownership is clear.
- Keep each change or pull request to one stated purpose. Split unrelated hardening, redesigns, migrations, and drive-by fixes into separate work.

## Testing

- Test lasting contracts rather than incidental current values; a test that reproduces current behavior is not evidence that the behavior is correct.
- Synchronize timing and concurrency tests on observable events or barriers, never scheduler assumptions or short sleeps. Run the relevant CI command and repeat race-prone tests before pushing.
- When a safety check emulates another tool's parsing or resolution, test it against the real tool with adversarial inputs, delegate to that tool, or reject ambiguous input. Test supported runtime environments and invocation paths rather than hypothetical ones.
- Do not use wall-clock or transaction timestamps as unique revision identifiers. Use unique tokens or a counter kept strictly monotonic under the mutation lock.

## Reviews

- A whole-code or whole-repository review covers the complete resulting tree, including inherited unchanged code; reviewers report findings and fix them only when explicitly asked.
- Independent reviewers use a fresh context containing only the review scope and repository/ref. They do not inherit implementation conclusions, modify the workspace, or recursively commission more reviewers unless requested.
- Escalate review cost in tiers: use cheaper reviewers first, fix findings, and repeat until a complete round has no high- or medium-risk findings. Use one max-effort reviewer at the end when warranted, never in parallel with cheaper tiers; after about three rounds on one finding chain, review the rest of the diff afresh.
- Before declaring a pull request ready, perform one final full `base..head` review. For high-risk changes—trust boundaries, production delivery, persistent state, concurrency or failure handling, cross-service contracts, or shared-history rewrites—also review the exact merge result and follow `checklists/high-risk.md`.
- When fixing findings, search for every instance of the pattern, compare each fix with the last reviewed version for regressions, verify delegated “fixed” claims against the diff, and add a failing-before test where the issue is testable. Report the verdict and evidence for each finding.

## Delivery

- A build, publication, or dispatch is an intermediate result. Release work is complete only when the intended artifact and configuration are identified and the downstream outcome is verified; security controls count only when enforced at the relevant trust boundary and verified.
