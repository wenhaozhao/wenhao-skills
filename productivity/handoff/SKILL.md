---
name: handoff
description: Transfer one active long-running software task between Codex sessions using evidence-backed state stored under the current repository's `.ai/` directory. Use only at a session boundary. Require a clean Git worktree before starting, and treat preparation as successful only when the worktree is clean afterward; do not use for routine progress tracking or work that can continue in the current conversation.
---

# Handoff

Use this skill only at a session boundary. While the current session can continue effectively, rely on its conversation context and do not create or update handoff state.

Maintain one active long-running task per worktree using these repository-local files:

- `<repo>/.ai/PROJECT_STATUS.md` for durable task state
- `<repo>/.ai/HANDOFF.md` for session-boundary context

Resolve `<repo>` with `git rev-parse --show-toplevel`. Never save the handoff document in the operating system's temporary directory.

## Select the phase

Use `prepare` when the user is about to switch sessions.

Use `resume` when the user starts a new session from an existing handoff.

If no phase is explicit, infer it from context:

- an ending or session-switch request means `prepare`
- a new-session continuation request means `resume`

Do not expose additional modes.

## Enforce a clean worktree

Before any handoff work:

1. Confirm that the current directory belongs to a Git worktree. Stop if it does not.
2. Run `git status --porcelain=v1 --untracked-files=all` from the repository root.
3. Stop immediately if the command reports any staged, unstaged, untracked, conflicted, or submodule change.
4. Report the dirty paths and ask the user to resolve them outside the handoff process.

When the initial worktree is dirty, do not inspect or update handoff documents and do not continue with `prepare` or `resume`.

Never stash, commit, reset, clean, delete, or otherwise alter pre-existing workspace changes merely to pass this gate.

## Establish the storage policy

Before writing either `.ai` file, determine how each path can be updated while leaving Git clean:

- If a path is ignored by Git, it may be updated without creating worktree changes.
- If a path is tracked and its content must change, obtain explicit user authorization for a dedicated handoff commit before writing. Stage and commit only `.ai/PROJECT_STATUS.md` and `.ai/HANDOFF.md`.
- If a path is untracked and not ignored, stop before writing and ask the user to choose whether the handoff artifacts should be ignored or tracked and committed.

Do not silently modify `.gitignore`, Git exclude rules, the index, or commit history to satisfy the cleanliness requirement.

Before writing, preserve the prior contents and existence state of both handoff files so changes created by this run can be rolled back if finalization fails. Never roll back unrelated paths.

## Evidence rules

Reconcile claims using this order:

1. current files and relevant project artifacts
2. the clean Git branch and HEAD
3. verification results already observed during the session
4. existing project status
5. conversation context

Do not convert unverified conversation claims into facts. Use explicit qualifications such as `verified`, `implemented but unverified`, `partial`, `blocked`, `failed`, or `unknown`.

Reference existing specifications, ADRs, issues, commits, reports, and documentation instead of copying them. Do not persist secrets or unnecessary sensitive information.

Do not run new implementation work during handoff preparation. Run additional verification only when necessary, authorized, and known not to leave generated workspace changes.

## Prepare

1. Pass the clean-worktree gate.
2. Establish a storage policy that can end clean.
3. Inspect the clean repository state and verification already performed during the session.
4. Create or reconcile `.ai/PROJECT_STATUS.md`.
5. Create or replace `.ai/HANDOFF.md`.
6. If either changed file is tracked, create the explicitly authorized handoff commit containing only the two allowed `.ai` paths.
7. Run `git status --porcelain=v1 --untracked-files=all` again from the repository root.

Treat `prepare` as successful only when the final status output is empty.

If final status is not clean, do not report a completed handoff. Restore only handoff-file changes created by this run when that can be done without touching unrelated changes, verify status again, and report the failure and exact remaining state.

On success, report:

- the absolute paths of both handoff files;
- the recorded branch and HEAD;
- the handoff commit hash, if a commit was created;
- confirmation that the final worktree is clean.

### Persistent state

Use this structure for `.ai/PROJECT_STATUS.md`:

```markdown
# Project Status

## Objective

## Acceptance Criteria

## Current State

## Decisions and Constraints

## Relevant Artifacts

## Verification

## Blockers and Known Issues

## Failed Attempts

## Next Actions

## Handoff Point
```

Keep it concise:

- preserve durable facts required by a future session;
- record only decisions that constrain future work;
- record failed approaches only when they prevent repeated work;
- include retry conditions when known;
- distinguish verified and unverified work;
- replace stale information instead of appending a diary;
- record the timestamp, branch, HEAD, and `Worktree: clean` at the handoff point;
- never invent unavailable values.

### Session handoff

Use this structure for `.ai/HANDOFF.md`:

```markdown
# Session Handoff

## Persistent State

Read `.ai/PROJECT_STATUS.md`.

## Workspace Baseline

## Current Focus

## Immediate Next Action

## Session-only Context

## Suggested Skills

## Warnings
```

Include only information that would otherwise be lost at the session boundary. Do not duplicate the complete project status. Record the clean branch and HEAD in `Workspace Baseline`.

## Resume

1. Pass the clean-worktree gate before consuming the handoff.
2. Read `.ai/PROJECT_STATUS.md` and `.ai/HANDOFF.md` from the repository root.
3. Compare the current branch and HEAD with the recorded workspace baseline.
4. Reconcile material conflicts before editing.
5. Briefly report the recovered objective, current clean workspace condition, and important discrepancies.
6. Identify the first executable next action and continue within the user's existing authorization.

Stop if either handoff file is missing or the worktree is dirty. Do not recreate the previous conversation. Do not re-investigate settled decisions unless current evidence conflicts with them. Do not repeat a failed approach unless its retry condition is satisfied or new evidence justifies it.

After resuming, rely on the new conversation context. Normal authorized work may then change the workspace; the final-clean requirement applies to the completed `prepare` phase, not to subsequent implementation. Do not update the handoff files again until another session switch is requested.
