# Rivet agentOS

Upstream: https://github.com/quake/agent-os

Product/docs: https://rivet.dev/agentos/

## Layer

In-process agent computer / sandbox library.

## Most valuable abstraction

**[In-process agent OS](../../abstractions/in-process-agent-os.md)**

Rivet agentOS asks whether a useful agent “computer” can be embedded as a library inside the host process rather than provisioned as a separate container or microVM.

## Core architecture

Upstream describes an in-process operating-system-like kernel written in JavaScript.

It combines:

- a virtual filesystem,
- process table,
- pipes and PTYs,
- virtual networking,
- WebAssembly-compiled POSIX utilities,
- V8 isolates for JavaScript/TypeScript agent code.

```text
host application
      │
      ▼
  agentOS kernel
      │
 ┌────┼───────────────┐
 ▼    ▼               ▼
VFS  WASM tools    V8 isolates
     POSIX-ish      agent code
```

Nothing needs to execute directly on the host for the lightweight path.

## Why it matters

Most agent infrastructure assumes that “give the agent a computer” means allocating a container, microVM, or VM.

Rivet agentOS creates a different design point:

> A large class of agent tasks may only need computer semantics, not a dedicated Linux machine.

This can potentially reduce:

- cold-start latency,
- per-agent memory,
- infrastructure cost,
- network hops between the agent runtime and its tools.

The system also supports escalation to external full sandboxes when a workload needs browsers, native binaries, or richer Linux behavior.

## Security model

The upstream design is deny-by-default and exposes granular permissions for filesystem, network, process, and environment access.

The important research question is whether WASM + V8 isolation is sufficient for the threat model of autonomous, potentially prompt-injected agent code — and where a microVM/gVisor boundary remains necessary.

## Compatibility model

The project provides agent integrations and a unified session layer, while the underlying computer abstraction remains smaller than full Linux.

This makes compatibility a central question:

- Which coding agents can run unchanged?
- Which commands need WASM ports?
- Which workflows require escalation to a real sandbox?
- How much POSIX behavior is enough?

## Current status — November 2026

The upstream repository is active and presents agentOS as an Apache-2.0 open-source package.

Current upstream claims include:

- single-digit-millisecond cold starts for the in-process path,
- substantially lower memory use than full sandbox baselines,
- WASM/V8 isolation,
- filesystem mounts backed by external systems,
- host-defined tools,
- cron/webhooks/queues/workflows,
- optional integration with full sandbox providers.

Benchmark numbers in upstream documentation should be treated as **project claims until independently reproduced**.

## Experiments to run

- run a small coding agent entirely inside agentOS;
- test filesystem/process/network compatibility against common coding-agent workflows;
- measure cold start and memory locally;
- attempt git/npm/build workloads and record where compatibility breaks;
- test deny-by-default filesystem and egress controls;
- inspect whether host tools accidentally recreate ambient authority;
- trigger escalation to a full sandbox and examine state handoff;
- compare the same task across agentOS, a container, gVisor, and a microVM.
