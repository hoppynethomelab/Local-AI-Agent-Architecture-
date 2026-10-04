# Agent Operating Model

The repository provides four optional Copilot custom agents under
`.github/agents/`, with shared guidance in `.github/copilot-instructions.md`.
The agent definitions are available under the MIT License in
`.github/agents/LICENSE`. These prompts are public, version-controlled
artifacts intended to be inspectable and reusable. They are role definitions,
not autonomous background services, and do not continuously watch GitHub
issues.

The agent prompts themselves do not include a model or guarantee free usage.
Prefer a compatible, owner-approved local open-weight runtime where the active
client supports it. Do not silently fall back to a paid/remote model. The
actual model, cost, and token/quota limits are controlled by the Copilot client
and configured provider, not by these Markdown files.

## Roles

- **business** owns the explanation and clarification of project intent,
  outcomes, scope, and business acceptance criteria. The project owner is the
  final authority when the written sources do not answer a question.
- **project-manager** triages questions, keeps issue requirements and
  dependencies actionable, tracks evidence-based progress, routes questions,
  and coordinates releases.
- **engineering-supervisor** unblocks code and infrastructure work and
  delegates bounded technical investigation or research.
- **qa** independently checks an issue's acceptance criteria against the
  actual change and reports PASS/FAIL with evidence on the ticket.

## Ticket flow

1. The project manager reads the ticket and classifies a question.
2. Business questions go to business; development or infrastructure blockers
   go to engineering supervision; completed changes go to QA.
3. Record decisions, blockers, estimates, validation evidence, and QA status
   in the relevant issue when the required GitHub tools are available. Never
   claim a write succeeded unless it did.
4. QA failure returns the issue to its original assignee for rework. QA pass
   recommends a PR to `dev`; it does not merge or deploy.
5. The project manager coordinates promotion from `dev` to `main`, following
   required CI and human review.

When work begins, makes substantial progress, changes state, or pauses, the
responsible agent should post the shared `WORK STATUS` checkpoint template to
the ticket. It records verified completed work, branch/commit and worktree
state, tests, remaining actions, blockers, and quota information. If GitHub
write access is unavailable, provide ready-to-post text and state that it was
not posted. Report quota exhaustion or a reset timestamp only when the active
provider/client explicitly exposes that information; otherwise use **not
reported** or **unknown**. A paused agent will not resume automatically; a
person or orchestrator must invoke it again when capacity is available.

## Estimation and reporting

Use Fibonacci Planning Poker points. A point estimate is provisional until
the people doing/reviewing the work agree on it. Keep points, elapsed
owner-hours, and calendar forecasts distinct; do not imply that points convert
exactly to hours. Report burndown only from actual dated issue completion data.
When velocity or owner capacity is unknown, say so and label dates provisional.

## Tool and permission limits

Agent capabilities depend on the current Copilot client, enabled tools, and
repository permissions. In this repository, GitHub writes may not be available
to every agent, and an assistant identity may not be assignable as a GitHub
issue owner. Provide a ready-to-post update and report the limitation instead
of pretending the action occurred.

The local/open-weight model preference is only enforceable when the selected
client/provider supports that runtime. If the active session uses a
quota-limited or remote model, disclose that limitation rather than implying
these repository prompts bypass it.

Agents must follow the development flow documented in
[Development Workflow](./Development%20Workflow.md): feature branches target
`dev`; only reviewed, tested release work is promoted from `dev` to `main`.
No agent should bypass CI, branch rules, human decisions, or the production
deployment controls.
