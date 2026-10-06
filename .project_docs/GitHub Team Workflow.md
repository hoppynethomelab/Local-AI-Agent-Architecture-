# GitHub Team Workflow

This guide explains where to put team work, how to keep after-hours tasks small,
and how to find status without duplicating the source of truth.

## Source of truth

- **Issues** hold the user story, acceptance criteria, estimate, owner, blockers,
  dependencies, and completion evidence. Keep an issue open until its criteria
  pass QA.
- **Pull requests** hold code review and the CI status for a proposed change.
  Feature branches target `dev`; reviewed, tested release work is promoted to
  `main` through a pull request.
- **Actions** holds complete test and deployment logs.
- **Discussions** is the team conversation stream for questions, decisions,
  handoffs, blockers, and automated CI/deployment summaries. Record durable
  decisions back on the affected issue or documentation. Never post secrets,
  private keys, tokens, or raw sensitive logs.
- **Wiki** is for maintained how-to guides, onboarding, architecture decisions,
  and operational runbooks. Keep volatile task status in issues/Projects, not
  duplicated wiki tables.

The repository's [Team Ops discussion](https://github.com/hoppynethomelab/Local-AI-Agent-Architecture-/discussions/58)
is the team-wide operations channel.

## After-hours task sizing

Use Planning Poker with the standard Fibonacci sequence, but do not confuse
points with hours. To fit work around other commitments, split tasks into
finishable slices of roughly 30-90 minutes and estimate each slice in owner
hours as well as provisional points. A useful local sizing convention is:

| Timebox | Planning points | Intended scope |
|---|---:|---|
| 30 minutes | 0.5 | One bounded edit/check or decision with a clear output |
| 60 minutes | 1 | A small vertical slice with focused validation |
| 90 minutes | 1.5 | Largest planned after-hours chunk; split anything larger |

These are planning aids, not measured velocity. Estimates remain provisional
until the owner and implementer review/vote. Do not infer burndown or delivery
forecasts from points until completed work and actual capacity are recorded.

## Delivery board design

Use the linked **Local AI Agent** Project as the source of delivery status rather
than maintaining multiple competing boards.

Recommended fields:

- **Status:** Backlog, Ready, In progress, In review, In test, Blocked, Done.
- **Priority:** P0, P1, P2, P3.
- **Estimate:** owner-hours per small task; keep Fibonacci points in the issue
  body until a Project number field for points is deliberately configured.
- **Milestone:** use the existing roadmap milestones.
- **Assignees:** use GitHub's assignee field as the owner.

Saved views currently provided:

1. **All Work:** full project table and backlog.
2. **Team Board — Kanban:** board grouped by Status.
3. **Ready to Start**, **In Progress**, and **In Test / QA:** status-filtered queues.
4. **My Tasks:** table filtered to open work assigned to the current user.

For charts, use **Work by Status** for the current item breakdown and **Burn up**
for work growth/completion over time. A burnup/burndown view is meaningful only
when issue completion dates and estimates are maintained; show it as a trend,
not a promise, until several weeks of actual throughput are available. Keep the
chart's scope explicit (for example, one milestone).

## Wiki structure

Keep the Wiki concise and link each page to the authoritative repo document or
issue set:

- **Home:** links to the board, issue tracker, Team Ops discussion, Actions,
  development workflow, and architecture guide.
- **Start Here:** local setup, supported hardware/model decisions, and first
  issue to pick up.
- **How We Deliver:** branch/PR/CI/release flow, QA expectations, and poker
  sizing.
- **Architecture & Decisions:** approved diagrams and dated decision records;
  link back to the issue that accepted a decision.
- **Runbooks:** deployment, health checks, backup/restore, and failure
  diagnosis. Keep secrets out of the Wiki.
- **Agent Roles:** when to use Business, Project Manager, Engineering
  Supervisor, and QA; link to `.github/agents/` sources.

## Failure and progress reporting

The `Team ops notifications` workflow listens for completed CI and production
workflows. It posts failed CI checks and every production deployment result to
the Team Ops Discussion with the branch, short commit ID, result, and Actions
run link. If the repository Actions secret `DISCORD_WEBHOOK_URL` is configured,
it also posts the same safe summary to the configured Discord channel. The
Discord webhook provides **one-way alerts only**; it does not connect an
assistant account for two-way Discord conversations. If the secret is missing,
the workflow logs a notice and skips the Discord post. Never put the webhook
URL in source code, issues, Discussions, or the Wiki; add it as a repository
Actions secret and rotate it if exposed. The notifications deliberately exclude
raw logs and secret values. Open the linked run for detailed diagnostics; post
any sanitized diagnosis or owner action in the Team Ops discussion and update
the affected issue.

For work status, use the Project Status field and add an issue comment at
start, substantial progress, blocker, QA handoff, and completion. **In test**
means QA is actively evaluating acceptance criteria; a green CI check alone is
not a QA pass. Close issues only after their acceptance criteria and required
QA evidence are satisfied.
