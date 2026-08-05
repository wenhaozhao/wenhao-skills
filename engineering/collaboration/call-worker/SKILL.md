---
name: call-worker
description: Assess whether a task would benefit from luna-worker, sol-worker, or the wenhao-skills:code-review skill; request explicit user authorization before using any of them; and execute only the approved scope. Use when the main agent recommends delegation or formal review, or when the user invokes call-worker to evaluate those options. Invoking this skill alone is not authorization.
---

# Call Worker

Keep decision authority with the user. Treat automatic and manual skill invocation as a request to assess options, not as permission to execute them.

## Assess

Perform only read-only investigation needed to make a recommendation. Do not launch, prewarm, test, or dispatch a sub-agent before authorization.

Recommend one or more options only when their benefit exceeds their coordination cost:

- **Main agent:** Prefer for read-only work, small changes, and tasks the main agent can safely verify.
- **`luna-worker`:** Recommend for bounded, local, low- or medium-risk implementation.
- **`sol-worker`:** Recommend for high-complexity, architectural, cross-protocol, security-critical, production, device, or otherwise high-risk work.
- **`wenhao-skills:code-review`:** Recommend when an independent formal review of repository changes is useful.

Respect more specific project instructions. If they require a worker or review, explain the requirement and request authorization; do not bypass the gate or continue restricted work without approval.

## Request authorization

For each recommended action, state:

- the exact worker or skill;
- the reason and expected benefit;
- the bounded task or review scope;
- known persistent effects such as a branch, worktree, or commit;
- explicit exclusions such as push, pull request, merge, production, device, or irreversible actions;
- the main consequence of declining;
- a direct authorization question.

Offer main-agent execution when it remains safe. Keep these permissions independent unless the user explicitly approves them together:

- using `luna-worker` or `sol-worker` for the stated task;
- invoking `wenhao-skills:code-review` for the stated review.

Disclose that `wenhao-skills:code-review` uses sub-agents. Let that skill control its own prerequisites, process, and report; do not redefine them here.

## Interpret authorization

- Treat invoking `call-worker`, including explicit `$call-worker` use, as evaluation only.
- Treat clear natural-language approval that identifies the action and scope as authorization; do not ask twice.
- Do not infer authorization from general requests such as “finish,” “implement,” “fix,” or “continue.”
- Do not reuse authorization from another task.
- Ask again if the scope, worker, review target, side effects, or risk materially changes.
- Never extend authorization to push, pull request, merge, production, device, irreversible, or out-of-scope actions.

## Execute

After authorization, perform only the approved action and scope.

- For worker execution, provide the minimum task-local context and preserve all project gates.
- For review, use `wenhao-skills:code-review` and follow its `SKILL.md` completely.
- Report what was authorized, what ran, and any remaining boundary or blocker.
