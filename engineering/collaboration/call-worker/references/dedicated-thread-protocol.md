# Dedicated Thread Protocol

Use this protocol for every dedicated Codex development task.

`luna-worker` and `sol-worker` are portable logical profiles owned by `call-worker`. They use Codex's built-in `worker` with an explicitly supplied model and reasoning effort; they never require same-named files under `~/.codex/agents/` or `.codex/agents/`.

## Select model, effort, and authorization

First choose the lowest-cost profile that safely fits the task:

| Task shape | Model | Reasoning effort |
| --- | --- | --- |
| Read-only lookup, mechanical edit, or narrow deterministic check | `gpt-5.6-luna` | `low` or `medium` |
| Bounded implementation with clear acceptance and local impact | `gpt-5.6-luna` | `high` |
| Routine multi-file implementation or moderate ambiguity | `gpt-5.6-terra` | `high` |
| Complex architecture, cross-module reasoning, or difficult diagnosis | `gpt-5.6-sol` | `high` or `xhigh` |
| Security-critical, production, device, irreversible, or unusually broad work | `gpt-5.6-sol` | `xhigh` or `max` |

Do not select `max` merely because it is available. If the requested host does not support the selected combination, stop and request a new choice instead of silently substituting one.

Then apply the authorization gate. Compare efforts in this order:

```text
none < minimal < low < medium < high < xhigh < max < ultra
```

Only these profiles avoid an additional model-profile authorization:

- `gpt-5.6-luna` with `none`, `minimal`, `low`, `medium`, `high`, or `xhigh`;
- `gpt-5.6-sol` with `none`, `minimal`, or `low`.

Every other profile requires explicit user approval. Combine that approval with the task or review request when practical. The whitelist never authorizes creating a task, dispatching a worker, or starting review. A UI-selected profile authorizes only the current main thread and never transfers into this dedicated task.

Record:

```yaml
execution_profile:
  mode: dedicated-thread
  model: exact-model
  reasoning_effort: exact-effort
  selection_basis: concise-reason
  profile_authorization:
    required: true-or-false
    source: whitelist | explicit-user-approval
    revision: 1
```

Validate the exact profile on the destination host. Do not silently substitute an unsupported combination. Any later model or effort change requires a higher authorization revision. Use `ultra` only when the user also authorizes any additional agent behavior it may enable.

For a logical worker profile, also verify that the destination host exposes the built-in `worker` capability. If the capability or exact model-effort combination is unavailable, return `UNSUPPORTED_EXECUTION_PROFILE` and request a new choice before delegation.

## Create and identify the task

Use an existing project Task ID. Otherwise generate `TASK-YYYYMMDD-HHMMSS`. Set the title to `<task-id> <role> <outcome>`.

Before creation, use `list_projects` and select the exact project. For a Git project, default to a dedicated worktree; otherwise use its local environment. Pass the approved model and reasoning effort to `create_thread`.

Register:

```yaml
task_id: TASK-...
client_thread_id: optional pending identity
thread_id: stable identity when ready
host_id: execution host
project_id: exact project
title: generated title
status: pending-or-running
execution_profile: exact approved profile and revision
```

Use `thread_id` plus `host_id` for all messages, reads, and waits. Never use a title as the stable address. If creation returns only `clientThreadId`, keep the task pending and reconcile it with `list_threads` using the unique Task ID, exact title, and project ID; confirm the initial prompt with `read_thread` before binding the returned `threadId`. If it is not ready, report pending instead of guessing. Use `send_message_to_thread` for follow-ups and `wait_threads` with cursors for incremental supervision.

## Send the task contract

Include the objective, acceptance criteria, allowed and prohibited scope, required reading, baseline, validation, allowed persistent effects, parent thread identity, selected model and effort, and this authorization envelope:

```yaml
authorization:
  revision: 1
  granted_by: user
  relayed_by: supervisor
  task_id: TASK-...
  implementation: allowed-or-denied
  code_review: allowed-or-denied
  execution_profile:
    mode: dedicated-thread
    model: exact-model
    reasoning_effort: exact-effort
    selection_basis: concise-reason
    profile_authorization:
      required: true-or-false
      source: whitelist | explicit-user-approval
      revision: 1
  scope: exact paths and requirements
  persistent_effects: explicit allowed effects
  prohibited: explicit denied effects
  expires_on: completion, scope change, risk change, or ownership transfer
delegation:
  depth: 0
  create_threads: false
  spawn_workers: false
  message_parent: true
```

The task owner must not repeat an already authorized action. It must not transfer authorization to another task or worker.

## Prevent recursive delegation

The dedicated task is a leaf executor. It may use `call-worker` only to assess an unapproved expansion. In that case, use `send_message_to_thread` with the recorded parent thread identity to send:

```yaml
status: AUTHORIZATION_REQUIRED
task_id: TASK-...
requested_action: exact action
requested_model: exact model
requested_reasoning_effort: exact effort
profile_authorization_required: true-or-false
reason: concise basis
scope_change: exact change
can_continue_without_it: true-or-false
```

Pause only the affected work. The parent supervisor obtains the user's decision and sends an `AUTHORIZATION_UPDATE` with the same Task ID and a higher revision. Reject stale, mismatched, or broader updates.

An update that changes the model or effort must carry a higher profile authorization revision. Approval of the current thread's UI profile is not valid evidence for a dedicated task, worker, or reviewer.

## Finish and review

The current task owner performs implementation, validation, and—when authorized—invokes the unchanged `wenhao-skills:code-review` in the same task thread. Before review, fix the intended baseline and verify the current HEAD and diff. Treat the review skill as an opaque stable workflow: follow its `SKILL.md` completely and do not inject, constrain, or redefine its internal reviewer models or reasoning effort.

Return the Task ID, thread ID, model, reasoning effort, changed paths, commit or HEAD, validation evidence, review result when authorized, and any remaining blocker to the parent supervisor.
