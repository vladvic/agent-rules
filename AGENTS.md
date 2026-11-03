# Agent Instructions

## Startup and synchronization

- Workspace-root `AGENTS.md` and `RULES.md` are symlinks into this `agent-rules` repository. Never replace them with regular files.
- After this bootstrap file is loaded, the primary agent should try `git -C agent-rules pull --ff-only` before task work only when the rules worktree is clean. If the pull advances the repository, reread both policy files before continuing. If the repository is dirty, offline, or cannot fast-forward, continue with the local copy, report the condition, and never stash, reset, discard, or auto-resolve another session's work.
- Only the primary agent synchronizes `agent-rules`. Local subagents use the existing checkout; fresh external reviewers receive only the review scope and repository/ref and must not modify the workspace or commission further reviewers.
- Read `RULES.md` once after the synchronization attempt. Reread it only after editing it or when explicitly told it changed.

## Knowledge base

- Read `knowledge-base/` only when a task needs architecture or project facts. Start with `knowledge-base/README.md` and open only the relevant documents.
- When an intended change affects documented architecture, behavior, operations, constraints, or decisions, update the relevant documentation according to `knowledge-base/MAINTENANCE.md` and distinguish current behavior from proposed or unmerged behavior.

## Delegation and resources

- Delegate only independent, bounded work with a clear result. Keep short or dependent steps with the primary agent, prevent overlapping file edits, and verify delegated results before relying on them.
- For local CPU-intensive work, do not stack heavy jobs and cap total build or test workers at half the available logical CPUs unless the host enforces a stricter quota. Do not add affinity ceremony to lightweight commands.

## Maintaining these rules

- Only the primary agent edits these policy files. Add or change a rule when explicit user feedback identifies a clear, reusable problem; ask when the intended lesson is ambiguous.
- Before adding a rule, require that it is likely to recur, applies workspace-wide, is actionable, and is not already covered. Put repository-specific procedures in that repository's instructions or CI, workspace facts in the knowledge base, and mechanically checkable constraints in automation.
- Keep `RULES.md` at no more than 15 bullets. Adding a rule requires consolidating or retiring an existing rule when necessary; superseded rules may be removed.
- After editing a policy file, reread the resulting file and tell the user what changed.
- Direct commits to `agent-rules/main` are the explicit exception to the normal no-direct-main rule. Stage only intended policy files, commit them, and push without force. If a push is rejected, fetch and inspect the divergence; do not stash, reset, discard changes, or resolve a non-trivial conflict without user direction.
