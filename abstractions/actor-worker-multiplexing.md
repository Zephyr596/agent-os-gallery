# Actor-Worker Multiplexing

## Core question

How can a platform host many long-lived logical agents when only a fraction of them are actively computing at any moment?

## Reference implementation

[Agent Substrate](../projects/agent-substrate/README.md) models workloads as **Actors** that are mapped onto a smaller set of ready **Workers**.

```text
many logical actors
A A A A A A A A A A
 \ | /   \ | /   /
   scheduler / router
          │
          ▼
   smaller worker pool
      W   W   W
```

Actors can be created, suspended, resumed, moved, and routed independently from the lifecycle of the worker infrastructure.

## Why it matters

Coding agents and autonomous workflows are bursty:

- active while thinking, compiling, testing, or browsing,
- idle while waiting for a model, human, webhook, or timer,
- potentially long-lived in identity and filesystem state.

Provisioning one permanently active pod or VM per logical agent wastes capacity.

## Oversubscription as an Agent OS primitive

The important idea is not merely Kubernetes scheduling. It is **agent-aware oversubscription**:

```text
logical concurrency >> physical concurrency
```

This requires lifecycle semantics above ordinary containers:

- suspend / resume,
- snapshot / restore,
- request parking,
- placement,
- routing to the currently active worker,
- migration between workers,
- state transfer.

## Separation of identity and placement

An actor should remain the same logical agent even when its physical worker changes.

```text
actor identity != pod identity
```

That mirrors the state/compute separation explored by Cloudflare Computer, but at a scheduler and kernel/runtime level rather than a filesystem API level.

## Design questions

1. What state must be checkpointed: filesystem only, memory, sockets, processes?
2. What is suspend/resume latency?
3. How many actors can safely oversubscribe a worker pool?
4. What happens when all workers are occupied?
5. Can inbound requests wait rather than fail?
6. How does scheduling account for GPU, memory, and locality?
7. What security boundary separates actors sharing infrastructure?
8. How does the system recover from a worker crash during checkpoint/restore?
9. Does an agent runtime need to know it has migrated?

## Working conclusion

At scale, an Agent OS likely needs a scheduler that treats **long-lived logical identity** and **short-lived physical placement** as different concepts.

Traditional pod scheduling is necessary infrastructure, but not necessarily the right abstraction boundary for autonomous agents.
