# Buzz

Repo: https://github.com/block/buzz

## Layer

Human-agent collaboration / organizational control plane.

## Core idea

Humans, agents, workflows, git events, reactions, approvals, and project activity all become signed events in one relay/event log.

Agents are treated as workspace members with their own identity and audit trail rather than as opaque bots attached to a human account.

## Architectural themes

- Nostr-style signed event model
- relay as the shared source of truth
- channels / threads / DMs
- git events and patches
- workflows
- audit log
- CLI and ACP-facing agent surfaces

## Why it matters

Buzz is not primarily a computer runtime. Its importance to Agent OS research is the organizational model above the runtime:

- What is an agent's identity inside a team?
- How should human and agent work share history?
- Can git, chat, workflow, and approval live in one event model?

## Questions to test

- self-hosted relay experience
- ACP harness behavior with coding agents
- branch-as-room workflow
- identity and permission semantics for agents
- whether one event log remains usable as the workspace grows
