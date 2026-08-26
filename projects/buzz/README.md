# Buzz

Upstream: https://github.com/block/buzz

## Layer

Human-agent collaboration / organizational control plane.

## Most valuable abstraction

**[Unified human-agent event log](../../abstractions/unified-human-agent-event-log.md)**

Buzz treats humans, agents, workflows, git events, reactions, and approvals as participants in one signed event model rather than separating agent activity into an opaque bot layer.

## Core idea

Humans and agents share the same rooms and durable history.

Agents are modeled as workspace members with their own identity and audit trail rather than as hidden processes acting under a human account.

## Architectural themes

- signed event model,
- relay as shared source of truth,
- channels / threads / DMs,
- git events and patches,
- workflows,
- audit log,
- CLI and ACP-facing agent surfaces.

## Why it matters

Buzz is not primarily a computer runtime. Its importance is the organizational layer above execution:

- What is an agent's identity inside a team?
- How should human and agent work share history?
- Can chat, git, workflow, and approval be represented in one durable event system?
- Can agents answer with evidence from the same history humans use?

## Research contrast

Cloudflare OS focuses more heavily on capability mediation between an agent and external resources.

Buzz focuses more heavily on **shared identity, provenance, and collaboration once agents are inside the organization**.

These are complementary Agent OS concerns rather than competing runtime designs.

## Questions to test

- self-hosted relay experience;
- ACP harness behavior with coding agents;
- branch-as-room workflow;
- identity and permission semantics for agents;
- search and evidence retrieval over long histories;
- whether one event model remains usable as workspace complexity grows;
- how the event log interacts with model-specific memory and compaction.
