---
name: handoff
description: Transfer one active long-running software task between Codex sessions using evidence-backed persistent state and a temporary session handoff. Use only when preparing to leave the current session or starting a new session from an existing handoff; do not use for routine progress tracking or work that can continue effectively in the current conversation.
---

# Handoff

Use this skill only at a session boundary. While the current session can continue effectively, rely on its conversation context and do not create or update handoff state.

Maintain one active long-running task per worktree using:

- `.ai/PROJECT_STATUS.md` for durable task state
- a temporary handoff file for session-specific context

## Select the phase

Use `prepare` when the user is about to switch sessions.

Use `resume` when the user starts a new session from an existing project status or handoff.

If no phase is explicit, infer it from context:

- an ending or session-switch request means `prepare`
- a new-session continuation request or supplied handoff path means `resume`

Do not expose additional modes.

## Evidence rules

Before transferring state, reconcile claims using this order:

1. current files and relevant project artifacts
2. Git status and relevant diffs
3. observed test, build, lint, benchmark, or validation results
4. existing project status
5. conversation context

Do not convert unverified conversation claims into facts. Use explicit qualifications such as `verified`, `implemented but unverified`, `partial`, `blocked`, `failed`, or `unknown`.

Reference existing specifications, ADRs, issues, commits, reports, and documentation instead of copying them. Do not persist secrets or unnecessary sensitive information.

## Prepare

1. Inspect relevant repository and Git state.
2. Inspect verification already performed during the session.
3. Run additional verification only when proportionate, feasible, and authorized.
4. Create or reconcile `.ai/PROJECT_STATUS.md`.
5. Write a session handoff in the operating system's temporary directory.
6. Return the handoff's absolute path to the user.

Do not continue implementation after a requested handoff except to leave the workspace in a truthful, understandable state.

### Persistent state

Use this structure:

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
- record available handoff facts such as branch, HEAD, relevant uncommitted changes, and timestamp;
- never invent unavailable values.

### Temporary handoff

Use this structure:

```markdown
# Session Handoff

## Persistent State

Read `.ai/PROJECT_STATUS.md`.

## Current Focus

## Work in Progress

## Immediate Next Action

## Session-only Context

## Suggested Skills

## Warnings
```

Include only information that would otherwise be lost at the session boundary. Do not duplicate the complete project status. Treat this file as disposable working memory.

## Resume

1. Read `.ai/PROJECT_STATUS.md`.
2. Read the supplied temporary handoff, if available.
3. Inspect current Git state and relevant uncommitted changes.
4. Compare the workspace with the recorded handoff point.
5. Reconcile material conflicts before editing.
6. Briefly report the recovered objective, workspace condition, and important discrepancies.
7. Identify the first executable next action and continue within the user's existing authorization.

Do not recreate the previous conversation. Do not re-investigate settled decisions unless current evidence conflicts with them. Do not repeat a failed approach unless its retry condition is satisfied or new evidence justifies it.

After resuming, rely on the new conversation context. Do not keep updating the project status until another session handoff is requested.
