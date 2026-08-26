# Unified Human-Agent Event Log

## Core question

If agents become real collaborators, should their actions live in separate bot transcripts, or in the same organizational history as human work?

## Reference implementation

[Buzz](../projects/buzz/README.md) treats messages, reactions, workflow steps, approvals, and git activity as signed events in one relay/event log.

```text
Human A ─┐
Agent B ─┼──► signed events ─► shared relay / history
CI      ─┤
Git     ─┤
Workflow ─┘
```

Agents have their own identities rather than impersonating the user who configured them.

## Why it matters

Most current systems scatter organizational state across:

- chat,
- git hosting,
- CI dashboards,
- agent transcripts,
- approval queues,
- issue trackers,
- workflow engines.

This makes it difficult to answer a basic question:

> Why did this change happen, who or what decided it, and what evidence was available at the time?

A unified event model makes human and agent activity queryable through the same history.

## Agent identity as an organizational primitive

The important distinction is:

```text
agent as bot owned by user
```

versus:

```text
agent as accountable workspace member
```

A first-class agent identity can have:

- its own keys,
- channel membership,
- permissions,
- audit trail,
- authored patches,
- workflow actions,
- reputation/history.

This avoids collapsing all autonomous behavior into the identity of one human administrator.

## Event log as memory

An Agent OS needs memory at multiple levels. A shared organizational event log is different from model memory:

```text
model memory
= compressed context for reasoning

organizational event log
= durable evidence of what happened
```

The latter should ideally remain inspectable, signed, searchable, and independent of whichever model later summarizes it.

## Design questions

1. Does the agent have its own identity?
2. Can its actions be attributed without exposing model internals?
3. Are chat, git, workflow, and approval events queryable together?
4. Can humans and agents subscribe to the same event stream?
5. How are private channels and resource boundaries represented?
6. Does a single event model remain manageable at large scale?
7. How should mutable objects be represented over an append-oriented history?
8. Can an agent answer questions with receipts linking back to source events?
9. How does the event log interact with shorter-term agent memory?

## Working conclusion

As agents move from assistants to collaborators, **organizational identity and durable provenance may matter as much as the execution runtime**.

An Agent OS should make it easy to answer not only what the agent knows, but what the agent did and why the organization accepted it.
