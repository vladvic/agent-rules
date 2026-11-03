# Agent Instructions

## Live policy and startup

- The primary agent is the active user-facing owner of the current task. “Policy files” means `AGENTS.md` and `RULES.md` in the `agent-rules` repository. The workspace-root files are symlinks to the live `/home/vladimir/work/TriceTel/agent-rules` checkout on `main`; never edit through those symlinks or expose an unreviewed candidate there.
- Only the primary agent synchronizes live policy; reviewer sessions and subagents never pull it. Before task work, the primary verifies the live checkout is on `main` with origin `https://github.com/vladvic/agent-rules.git` and a clean worktree, rechecks that it is still clean, then runs `git -C /home/vladimir/work/TriceTel/agent-rules pull --ff-only --no-autostash origin main`. On any failed check or command, do not pull: report the failure and use the committed policy from local `refs/heads/main` via `git show`, never working-tree policy. Never stash, reset, discard, or auto-resolve another session's work. If the pull advances `main`, report the old and new commits and reread both policy files.
- Every primary, delegated, or local spawned agent reads the live `RULES.md` in full once before work. Reread both policy files after any operation that changes them or when explicitly told they changed. Fresh external reviewers receive only the review role, scope, exact local refs, and verbatim copies of the current risk-severity and stop-condition rules; reading that text is not policy synchronization.

## Knowledge base

- Read `/home/vladimir/work/TriceTel/knowledge-base/` only when a task needs architecture or project facts. Start with `/home/vladimir/work/TriceTel/knowledge-base/README.md` and open only the relevant documents.
- Update the knowledge base only after behavior is merged to the owning repository's default branch. Label any documented unmerged behavior with its branch and commit, follow `/home/vladimir/work/TriceTel/knowledge-base/MAINTENANCE.md`, and report separately when an update is local-only because the knowledge base is not version-controlled.

## Delegation and resources

- Delegate only independent, bounded work with a clear result. Keep short or dependent steps with the primary agent, prevent overlapping file edits, and verify delegated results before relying on them.
- A reviewer reports findings and does not edit or commission another reviewer unless the user explicitly requests review-and-fix or additional reviewers. The commissioning agent makes the exact refs available locally and states the role.
- Keep all CPU-heavy work started by the agent—builds, tests, indexers, local reviewers, and delegated agents—within half the available logical CPUs, accounting for other visible heavy jobs and lowering repository-prescribed worker counts when necessary. Do not add affinity ceremony to lightweight commands.

## Maintaining these files

- Only the primary agent modifies tracked `agent-rules` files. Add or change a rule when explicit user feedback identifies a clear, reusable problem; ask when the intended lesson is ambiguous.
- Before adding a rule, require that it is likely to recur, applies workspace-wide, is actionable, and is not already covered. Propose repository-specific instructions or CI changes unless the current task authorizes editing that repository; put workspace facts in the knowledge base and mechanically checkable constraints in automation.
- Keep `RULES.md` at no more than 15 bullets, with one coherent topic per bullet. Retiring or weakening a rule requires explicit user approval; name every retired or weakened safeguard to the user before committing and in the commit message.
- Prepare every change in an isolated non-`main` branch and worktree based on a freshly fetched `origin/main`. Record the baseline and candidate commits, keep the live checkout clean and unchanged, edit only candidate-worktree paths, and inspect the complete candidate diff for unexpected changes before each commit.
- Review the exact candidate refs under `RULES.md`. If `origin/main` advances, rebase only the current task's candidate commits without autostash; at any conflict, abort and ask. After any rebase, reread both candidate policy files and repeat the required fresh reviews.
- Only after the review stop condition is met, fetch `origin main` and verify that the candidate `HEAD` equals the reviewed candidate commit and `origin/main` equals the reviewed baseline. If either differs or the push is rejected, stop and restart candidate review. Otherwise push the candidate `HEAD` to `origin/main` without force, then fast-forward the live checkout with `pull --ff-only --no-autostash origin main`. Tell the user exactly what changed and verify the live and remote `main` commits match.
