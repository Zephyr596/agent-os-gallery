# Claw OS

Upstream: https://github.com/xiaoyu-work/claw-os

## Layer

Physical-machine agent system layer.

## Most valuable abstraction

**[Agent as a system service](../../abstractions/agent-as-system-service.md)**

Claw OS asks what changes when the agent is a first-class participant in the operating system rather than an assistant trapped inside one application.

## Core idea

Run the agent as a privileged system daemon (`clawd`) with scoped access to processes, logs, networking, applications, and host resources.

Important ideas include:

- system-level agent daemon,
- persistent cross-app memory,
- local-first model layer with cloud fallback,
- app capability SDK,
- capability checks and approvals,
- structured system primitives instead of relying only on UI scraping.

## Why it matters

This is currently the strongest reference in the gallery for the **physical Agent Computer** track.

A real machine has resources that matter directly:

- GPU / CUDA,
- local storage,
- build caches,
- Docker,
- Git repositories,
- SSH / network topology,
- hardware devices.

Instead of virtualizing those resources away, Claw OS exposes the host itself to the agent through a system layer.

## Deployment forms

- agent layer on Ubuntu,
- WSL2 distribution,
- Docker / container,
- desktop / VM image,
- cloud image paths.

## Security connection

Claw OS and Cloudflare OS approach different substrates but converge on an important principle:

> System visibility does not have to imply unrestricted mutation authority.

This makes Claw OS useful for comparing capability models on a real host with Cloudflare OS's resource-oriented Gatekeepers in a cloud workspace.

## First practical experiment

A low-risk starting point is:

```text
Windows laptop
   ↓
WSL2
   ↓
Claw OS agent layer
   ↓
Git / gh / Docker / CUDA / local models
```

This can test the agent-owned-machine idea before repartitioning the laptop or replacing Windows.

## Questions to test

- GPU/CUDA visibility through WSL2 and bare Linux;
- how credentials are scoped;
- unattended boot and recovery;
- full GitHub coding loop from issue to PR;
- whether capability checks remain usable under high-frequency autonomous work;
- local model routing and cloud fallback;
- emergency human override and failure recovery;
- differences between WSL2 and bare-metal Linux for a 24/7 host.
