# Agent Substrate

Repo: https://github.com/Zephyr596/substrate

## Layer

Agent infrastructure scheduler / lifecycle manager.

## Core idea

Map many long-lived logical actors onto a smaller pool of ready workers. Actors can be suspended, resumed, moved, and multiplexed while preserving state.

## Key concepts

- Actor
- Worker
- WorkerPool
- ActorTemplate
- suspend / resume
- snapshots
- routing
- oversubscription
- gVisor / microVM isolation
- Kubernetes as the infrastructure substrate

## Why it matters

Cloudflare Computer asks what a logical agent computer should be. Agent Substrate asks what happens when there are hundreds or thousands of such long-lived logical environments and most are idle most of the time.

The project explicitly targets coding-agent workloads such as Claude Code / Codex as well as generic OCI workloads.

## Current status

Early development; APIs are expected to change.

## Experiments to run

- reproduce counter demo locally with kind
- reproduce Claude Code multiplex demo
- measure suspend/resume latency
- inspect filesystem and memory persistence semantics
- compare gVisor and microVM paths
- deploy the same demo to GKE
- compare one-agent-one-pod versus multiplexed actors
