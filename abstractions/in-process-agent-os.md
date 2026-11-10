# In-process Agent OS

## Definition

An **in-process agent OS** implements enough “computer” semantics for an agent — filesystem, processes, shell-like tools, networking, terminals, and isolation — **inside the host application's process**, rather than provisioning a separate VM or container for every agent.

Strong reference: [Rivet agentOS](../projects/rivet-agent-os/README.md).

## Core idea

Traditional agent sandboxes commonly look like:

```text
application
   │
   ▼
sandbox API
   │
   ▼
container / microVM
   │
   ▼
Linux userspace
```

An in-process design instead moves the logical computer boundary inward:

```text
application process
   │
   ├─ virtual filesystem
   ├─ virtual process table / pipes / PTYs
   ├─ virtual network
   ├─ WASM utilities
   └─ V8 isolates for agent code
```

The advantage is not that it becomes “more real” than Linux. The advantage is that many agent tasks do not require a full Linux machine in the first place.

## Why it matters

Coding and tool-using agents spend substantial time on lightweight operations:

- reading/writing files,
- grep/sed/jq-style transforms,
- running small scripts,
- calling HTTP APIs,
- maintaining session state,
- invoking host-provided tools.

If these operations can execute inside an isolated in-process virtual computer, the system can avoid VM/container cold starts, image pulls, network hops, and per-sandbox memory overhead.

## The key boundary

The abstraction is useful only if it is clear what **does not** belong inside it.

A lightweight in-process OS is a poor fit for workloads that require:

- arbitrary native binaries,
- a real browser stack,
- kernel features,
- privileged networking,
- Docker,
- CUDA/GPU access,
- exact Linux compatibility.

A strong design therefore needs **escalation**:

```text
in-process agent computer
        │
        ├─ lightweight task → stay local
        │
        └─ heavyweight task → full sandbox / container / VM
```

## Comparison with other abstractions

### vs Cloudflare Computer

Cloudflare Computer separates authoritative state from execution backends.

Rivet agentOS instead asks how much of the execution environment can be implemented cheaply inside one application process.

These ideas are compatible: persistent state and in-process execution could be composed.

### vs Agent Substrate

Agent Substrate optimizes many isolated stateful workloads across a worker pool.

An in-process OS reduces the cost of each lightweight execution environment before cluster-level multiplexing is even considered.

Again, these are not mutually exclusive.

### vs native host access

A physical agent host gives the agent access to a real OS and devices.

An in-process OS deliberately presents a narrower virtual machine. It is therefore primarily a **sandbox abstraction**, not a replacement for a physical Agent Computer when the task requires real system control.

## Research questions

- Which POSIX semantics are actually required by coding agents?
- How much Linux compatibility can be removed before common agents break?
- Can WASM/V8 isolation provide a strong enough boundary for untrusted agent code?
- Where should persistent filesystem state live?
- How should host tools be exposed without recreating ambient authority?
- When should execution escalate to a container/microVM?
- Can this model support reproducible deterministic replay?
- How do cold-start, memory, and isolation costs compare with gVisor and microVMs?
