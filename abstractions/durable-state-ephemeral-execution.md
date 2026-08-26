# Durable State + Ephemeral Execution

## Core question

Does an agent need one permanently running machine, or does it mainly need **persistent state that can be attached to execution on demand**?

## Reference implementation

[Cloudflare Computer](../projects/cloudflare-computer/README.md) provides a strong concrete example:

```text
Durable Object + SQLite
        │
        ▼
authoritative filesystem state
        │
        ├── Dynamic Worker shell
        ├── Dynamic Worker JavaScript
        └── Linux container
```

The logical computer persists even when the execution backend does not.

## Why it matters

A traditional agent computer often implies:

```text
1 agent = 1 VM = 1 persistent process environment
```

That is simple, but expensive and operationally heavy when agents spend most of their time idle.

Separating state from execution changes the model to:

```text
logical computer
    = durable state
    + execution when required
```

This enables cheaper idle periods, backend specialization, faster horizontal scaling, and replacement of failed workers without making compute identity equal state identity.

## The abstraction boundary

The key distinction is between:

- **authoritative state** — filesystem, artifacts, metadata, durable working state
- **execution state** — processes, memory, kernel state, temporary caches

Cloudflare Computer makes the filesystem authoritative outside the container. Agent Substrate explores a stronger model in which even volatile execution state may be snapshotted.

## Design spectrum

```text
Persistent VM
│  filesystem + RAM + processes persist together
│
├── Persistent filesystem + replaceable runtime
│      Cloudflare Computer
│
└── Snapshottable actor state + pooled workers
       Agent Substrate
```

## Questions to evaluate

1. Which state must survive executor loss?
2. Is process memory part of identity or disposable cache?
3. How quickly can execution be reattached?
4. How are filesystem consistency and concurrent access handled?
5. Can different execution backends safely share one logical workspace?
6. Can a task escalate from isolate to container without changing identity?
7. What happens to network connections and long-running processes?
8. Is GPU execution another backend, or does GPU state require a different abstraction?

## Working conclusion

For many agent workloads, **machine identity and compute lifetime should probably not be the same thing**.

The durable object may be more important than the durable process.
