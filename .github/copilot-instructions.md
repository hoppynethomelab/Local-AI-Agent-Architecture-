# Shared agent instructions

These repository instructions apply to project agents alongside their
role-specific `.github/agents/*.agent.md` prompts.

## Project context and source of truth

- This is a privacy-first local AI agent project. The architecture and current
  delivery flow are documented in `.project_docs/`.
- The project owner is the authority for unresolved product, scope, and
  infrastructure decisions.
- Distinguish recorded facts, assumptions, and recommendations. Never invent
  issue updates, test results, provider quota state, or decisions.
- Follow the repository's `feature branch -> dev -> main` flow. Never bypass
  review, required CI, or production controls.

## Open-source agent prompts and model usage

- Keep role prompts and project operating instructions in this public
  repository so they can be inspected, versioned, forked, and reused.
- The Markdown agent definitions are open-source project artifacts; they do
  not themselves provide a model, remove a provider's subscription limits, or
  guarantee zero-cost inference.
- Prefer a compatible locally hosted open-weight model/provider when the
  configured client supports it and the owner has selected and tested it.
  Never silently send project data to a paid or remote model as a fallback.
- If the session is tied to a quota-limited provider, or local inference is
  unavailable, explain the limitation and stop or request an approved model
  choice. Do not claim the agent is running locally unless the active runtime
  confirms it.
- These prompts cannot inspect account billing, token balances, or provider
  reset metadata unless the active client explicitly exposes that information.
  Never estimate an exact quota reset time from usage patterns.

## Ticket progress and quota checkpoints

When assigned issue work begins, make substantial progress, changes state, or
must pause or hand off, add a concise comment to the relevant issue if GitHub
write tools are available. If not, prepare the comment text and explicitly tell
the user it has not been posted.

Use this format:

```text
WORK STATUS: IN PROGRESS | BLOCKED | PAUSED | COMPLETE
Owner/agent: <role or contributor>
Issue: #<number>
Updated: <timestamp with timezone>

Completed:
- <verified work so far>

Current state:
- Branch/PR/commit: <reference, or none>
- Tests/checks: <actual commands and results, or not run>
- Worktree/commit status: <clean, uncommitted paths, or unknown>

Remaining:
- <specific next actions>

Blocker / pause reason:
- <exact technical/business/infrastructure/quota reason, or none>

Quota status:
- <provider-reported exhausted/rate-limited state, or not reported>
- Resume available: <provider-reported timestamp and timezone, or unknown>
- Evidence/source: <provider/client message, or unavailable>
```

Rules:

- Report quota exhaustion only when the provider/client explicitly indicates
  it. If work is merely paused for another reason, say so.
- Copy any provider reset time exactly, including timezone and its source. If
  no reset time is exposed, write **unknown**; do not guess.
- Do not imply that an agent will resume automatically. Resume requires an
  available session/model and a person or orchestrator to invoke it again.
- Before stopping, preserve enough verified context for a later session to
  resume safely: the ticket, branch, changed files/commit, tests run, and next
  action. Never describe uncommitted work as saved.
- Do not close an issue just because a session ran out of tokens. Keep it open
  and blocked or paused until work resumes or the owner decides otherwise.
