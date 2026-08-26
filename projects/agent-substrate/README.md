# Agent Substrate

Upstream / working copy: https://github.com/Zephyr596/substrate

## Layer

Agent infrastructure scheduler / lifecycle manager.

## Most valuable abstraction

**[Actor-worker multiplexing](../../abstractions/actor-worker-multiplexing.md)**

Agent Substrate separates long-lived logical actors from the smaller pool of physical workers on which they happen to run.

## Core idea

Map many stateful actors onto fewer ready workers, relying on the fact that agent workloads are often idle.

Important concepts include:

- Actor
- Worker
- WorkerPool
- ActorTemplate
- suspend / resume
- snapshots
- routing
- request parking
- oversubscription
- gVisor / microVM isolation
- Kubernetes as infrastructure substrate

## Why it matters

Cloudflare Computer asks what a logical agent computer should be.

Agent Substrate asks a different question:

> What happens when there are hundreds or thousands of those logical environments and most of them are not actively computing?

The project explicitly targets coding-agent workloads such as Claude Code / Codex in addition to generic OCI workloads.

## Architectural significance

The key identity relationship is:

```text
actor identity != worker / pod identity
```

A logical agent can survive placement changes, suspend while idle, and resume on another worker.

This is a scheduler-level version of the broader state/compute separation seen elsewhere in the gallery.

## Current status

Early development; upstream APIs and architecture are expected to change.

## Experiments to run

- reproduce the counter demo locally with kind;
- reproduce the Claude Code multiplex demo;
- measure suspend/resume latency;
- inspect filesystem and RAM-state persistence semantics;
- compare gVisor and microVM paths;
- deploy the same demo to GKE;
- compare one-agent-one-pod versus multiplexed actors;
- test request parking under worker saturation.
