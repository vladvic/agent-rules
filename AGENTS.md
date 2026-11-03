# Agent Instructions

## Required Rules Reading

- Each primary, delegated, or spawned agent must read `RULES.md` in full once when it starts a new session or run in this workspace.
- Do not reread `RULES.md` for every prompt or task within the same session or run.
- If an agent edits `RULES.md` during its session, or is explicitly told that it changed since it was last read, the agent must reread the updated file before continuing work.

## Knowledge Base Usage

- Read `knowledge-base/` only when the task requires architecture or project facts. Do not read it for unrelated tasks.
- When the knowledge base is needed, use `knowledge-base/README.md` to select and read only the documents relevant to the task. Read the entire knowledge base only when the task genuinely requires workspace-wide architecture or project context.
- After completing each material change, update the relevant documents in `knowledge-base/` when the change affects architecture, externally visible behavior, operational procedures, constraints, or important project decisions. Do not document routine implementation details already evident from the code.
- Before committing, verify that `knowledge-base/` accurately reflects the changes being committed and update it if necessary.

## Dynamic Rules Maintenance

- Treat `RULES.md` as a living set of feedback-driven rules that may change during the work.
- When the user explicitly says that an agent did a great job, add a good-practice rule only when the feedback or task history clearly identifies a reusable process or behavior. If the reason for the praise is unclear, ask what should be preserved before adding a rule.
- When the user says that an agent acted incorrectly, worked in an unintended way, or did a bad job, first determine whether the feedback already states the exact reason clearly enough to produce an unambiguous corrective rule.
- If the exact reason is crystal clear from the user's initial feedback, add the corrective rule without asking a redundant follow-up question. Otherwise, ask the user exactly what was wrong and do not guess or add a rule before receiving clarification.
- Once the exact problem is clear, add a concise, actionable rule to `RULES.md` describing what to avoid or what not to do.
- Generalize feedback into reusable guidance instead of recording task-specific details.
- Preserve existing rules and consolidate overlapping guidance instead of adding duplicates or contradictions.

## Subagent Delegation

- Use subagents for independent, clearly bounded subtasks that can run in parallel and have a clear expected result, when the expected benefit outweighs the coordination overhead.
- Keep short tasks and dependent steps with the primary agent. Do not delegate solely because a task requires understanding existing code, adding a feature, or fixing a bug.
- Do not assign overlapping edits to the same files without explicit coordination. The primary agent remains responsible for integrating and verifying delegated work.
