---
name: call-worker
description: Choose the execution mode, model, and reasoning effort for a task; assess whether work should stay with the main agent, move to a dedicated Codex task, use the portable luna-worker or sol-worker execution profile, or invoke wenhao-skills:code-review; request explicit user authorization before delegation or review; and execute only the approved scope. Use when the main agent recommends delegation or formal review, or when the user invokes call-worker to evaluate those options. Invoking this skill alone is not authorization.
---

# Call Worker

Keep decision authority with the user. Treat automatic and manual invocation as evaluation, not permission to delegate or review.

## Assess

Perform only the read-only investigation needed to choose all of:

1. execution mode;
2. model;
3. reasoning effort;
4. whether `wenhao-skills:code-review` is warranted.

Never leave model or reasoning effort implicit. If work stays in the current thread, confirm its active combination is suitable or recommend a switch before execution.

Record every decision with this shape:

```yaml
execution_profile:
  mode: main-agent | dedicated-thread | luna-worker | sol-worker
  model: exact-model
  reasoning_effort: exact-effort
  selection_basis: concise-reason
  profile_authorization:
    required: true-or-false
    source: whitelist | current-ui-selection | explicit-user-approval
    revision: integer
```

Compare efforts in this order: `none < minimal < low < medium < high < xhigh < max < ultra`.

Only these profiles avoid an additional model-profile authorization:

- `gpt-5.6-luna` with an effort below `max`;
- `gpt-5.6-sol` with an effort below `medium`.

Every other model-effort profile requires explicit user approval. The user may approve it in the same decision as the dedicated task or worker action. A model profile selected by the user in the UI authorizes only the current main thread; do not transfer it to a dedicated task or worker. The whitelist waives only the extra profile approval, never an action approval otherwise required for delegation.

Validate the exact profile against the target host before execution. If unsupported, stop and request a supported choice; never substitute silently. Treat any model or effort change as a new profile requiring a higher authorization revision. Do not select `ultra` unless the user also authorizes any additional agent behavior it may enable.

Prefer these execution modes:

- **Main agent:** Read-only work, small changes, or tightly coupled critical-path work the current agent can safely verify.
- **Dedicated Codex task:** Default for a coherent development outcome that benefits from isolated context, a worktree, long-running execution, or parallelism.
- **`luna-worker`:** Short, bounded, low- or medium-risk side work whose result can be summarized.
- **`sol-worker`:** Short but complex, architectural, cross-protocol, security-critical, or otherwise high-risk side work.
- **`wenhao-skills:code-review`:** Stable review workflow owned by the current task thread after implementation and validation. Treat it as an opaque authorized workflow, not as a model-profile execution mode controlled by this skill.

Treat `luna-worker` and `sol-worker` as portable logical execution profiles, not custom-agent names that must exist in `~/.codex/agents/` or `.codex/agents/`. Spawn Codex's built-in `worker` and explicitly pass the selected model, reasoning effort, bounded task instructions, and authorization envelope:

| Logical profile | Base agent | Required model |
| --- | --- | --- |
| `luna-worker` | built-in `worker` | `gpt-5.6-luna` |
| `sol-worker` | built-in `worker` | `gpt-5.6-sol` |

Do not read, install, require, or rely on same-named local custom-agent files. Validate that the target host exposes the built-in worker capability and supports the exact model-effort combination before requesting or executing delegation. If either capability is unavailable, return `UNSUPPORTED_EXECUTION_PROFILE` and request a new choice; never fall back to another agent, model, or effort silently.

Read [references/dedicated-thread-protocol.md](references/dedicated-thread-protocol.md) before recommending or coordinating a dedicated task. Use its task-fit guidance and authorization gate for every mode. Treat worker names as roles and explicitly pass the selected model and reasoning effort; do not rely on static agent defaults.

Respect more specific project instructions. If they require delegation or review, explain the requirement and request authorization; do not bypass the gate.

## Request authorization

For every recommendation, state:

- execution mode, exact model, reasoning effort, and selection basis;
- whether the profile needs approval, its authorization source, and revision;
- bounded task or review scope;
- expected benefit and the consequence of declining;
- persistent effects such as a task, worktree, branch, or commit;
- explicit exclusions such as push, pull request, merge, production, device, or irreversible actions;
- a direct authorization question.

For dedicated development tasks, request these permissions together but separately selectable:

1. create and run the dedicated task with the stated model, effort, scope, and side effects;
2. let that task owner invoke `wenhao-skills:code-review` after implementation and validation.

Keep dedicated-task, worker, and review permissions independent unless the user explicitly approves them together. Disclose that `wenhao-skills:code-review` uses sub-agents under its own stable process. Do not inject, constrain, or redefine reviewer models or reasoning effort from this skill.

## Interpret authorization

- Treat invoking `call-worker`, including `$call-worker`, as evaluation only.
- Treat clear natural-language approval identifying the action and scope as authorization; do not ask twice.
- Treat a user-selected current UI profile as authorization only for the current main thread.
- Do not infer authorization from “finish,” “implement,” “fix,” or “continue.”
- Do not reuse authorization from another task.
- Ask again if scope, execution mode, model, reasoning effort, review target, side effects, or risk materially changes.
- Never extend authorization to push, pull request, merge, production, device, irreversible, or out-of-scope actions.

## Execute

After authorization, perform only the approved action and scope.

- For a dedicated task, follow the dedicated-thread protocol and give the task owner the authorization envelope.
- For a worker, spawn the built-in `worker` under the approved logical profile and explicitly pass the selected model, reasoning effort, minimum task-local context, and authorization envelope. Do not address or depend on a same-named custom agent.
- For review, let the current task owner use the unchanged `wenhao-skills:code-review` and follow its `SKILL.md` completely. The review authorization covers invoking that stable workflow; do not attach reviewer execution profiles.
- Report the execution mode, model, effort, authorization, stable task identity, and remaining blocker.
