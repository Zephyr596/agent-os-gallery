# Agent Substrate

Upstream: https://github.com/agent-substrate/substrate

## Layer

Zero-trust agent infrastructure runtime / scheduler / lifecycle manager on Kubernetes.

## Most valuable abstraction

**[Actor-worker multiplexing](../../abstractions/actor-worker-multiplexing.md)**

Agent Substrate separates long-lived logical actors from the smaller pool of physical workers on which they happen to run.

## Core idea

Map many stateful actors onto fewer ready workers, relying on the fact that agent workloads are often idle.

Important concepts now include:

- Actor
- Worker
- WorkerPool
- suspend / resume
- full-state snapshots
- routing
- oversubscription
- gVisor / Cloud Hypervisor microVM isolation
- Kubernetes as infrastructure substrate
- per-agent cryptographic identity
- default-deny networking
- external credential injection at egress

## Why it matters

Cloudflare Computer asks what a logical agent computer should be.

Agent Substrate asks a different question:

> What happens when there are hundreds, thousands, or millions of those logical environments and most of them are not actively computing?

The project explicitly targets coding-agent workloads such as Claude Code and Codex in addition to generic OCI workloads.

## Architectural significance

The key identity relationship is:

```text
actor identity != worker / pod identity
```

A logical agent can survive placement changes, suspend while idle, and resume on another worker.

This is a scheduler-level version of the broader state/compute separation seen elsewhere in the gallery.

## Security model

The 1.0 design makes a second idea unusually important: **the sandbox should not need to possess reusable credentials**.

Actors run inside gVisor or Cloud Hypervisor microVM isolation. The platform gives active actors short-lived identity and mediates network access. Approved outbound requests can receive credentials at the egress gateway, outside the untrusted sandbox.

```text
untrusted actor
    │
    │ outbound request
    ▼
egress policy / gateway
    │
    ├─ authorize actor identity
    ├─ inject approved credential
    ▼
external service
```

This is complementary to capability-mediated authority: narrow authority can be enforced without placing the underlying secret directly in agent-controlled memory or files.

## Current status — November 2026

Upstream states that **Agent Substrate has reached 1.0** and commits to compatibility of its public API, CLI, and configuration formats across minor releases.

The project also documents:

- sub-500 ms resume operations,
- more than 500 activations per second,
- 10x–30x oversubscription in its demo path,
- gVisor or Cloud Hypervisor microVM sandboxes,
- per-agent SPIFFE X.509 identity,
- external egress credential injection,
- default-deny network policy.

These are upstream claims until reproduced here.

## Experiments to run

- reproduce the counter demo locally with kind;
- reproduce a coding-agent multiplex demo;
- measure suspend/resume latency;
- inspect filesystem and RAM-state persistence semantics;
- compare gVisor and microVM paths;
- verify that long-lived credentials never enter the sandbox;
- test egress policy and credential injection;
- deploy the same demo to GKE;
- compare one-agent-one-pod versus multiplexed actors;
- test request behavior under worker saturation.
