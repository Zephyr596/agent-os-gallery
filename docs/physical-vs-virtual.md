# Physical vs Virtual Agent Computers

This gallery currently follows two practical research tracks.

## Track A — Physical Agent Computer

Goal: hand a real GPU laptop/workstation to an agent for long-lived use.

### Current baseline — November 2026

The working physical node is a ThinkBook 16p with Windows/Ubuntu dual boot.

The current preference is:

> **Run the agent on native Ubuntu; retain Windows as a fallback rather than using WSL2 as the primary agent environment.**

Reason: the goal is to give the agent a real machine, not merely a Linux userspace nested inside a human Windows workstation. Native Ubuntu keeps systemd, cgroups, Docker/containerd, network namespaces, TUN/TAP, device access, GPU behavior, sleep/recovery, and kernel-visible behavior closer to the actual host.

```text
remote operator
  → Tailscale / private network
  → ThinkBook 16p
  → native Ubuntu
  → agent runtime / daemon
  → shell / git / GitHub / Docker / CUDA
  → test
  → commit / PR
```

Windows remains useful for occasional interactive use, vendor tooling, or fallback recovery, but it is not the preferred substrate for the dedicated agent node.

### Desired capabilities

- persistent filesystem
- shell
- git + GitHub CLI
- Docker / containerd
- local GPU / CUDA
- local model serving when useful
- remote access over a private network
- unattended boot/recovery
- durable credentials with narrow scope
- audit/logging
- reproducible service startup via systemd

### Candidate host setups

1. **Native Ubuntu — current preferred baseline**
2. Windows + WSL2 — useful developer compatibility baseline
3. Minimal / server-oriented Linux
4. Human-oriented Linux desktop such as Omarchy

### Candidate agent/system layers

- Claw OS
- coding-agent harnesses
- custom daemon + existing coding agents

### Baseline experiment

```text
Mac / phone
  → private network
  → physical GPU laptop
  → agent receives issue/task
  → clone / edit / test
  → local or cloud inference
  → commit / PR
```

The main question is not just whether this can work, but whether it remains reliable when the human does not touch the machine for days.

### Storage implication

A dedicated physical agent node should keep its active workspace, Docker data, caches, logs, databases, and model files on a Linux-native filesystem rather than treating Ubuntu as a small boot partition around a shared NTFS data disk.

For a multi-terabyte dual-boot machine, Linux should therefore receive a substantial share of the available SSD capacity if it is the primary agent host.

---

## Track B — Managed / Virtual Agent Computer

Goal: make the computer itself a logical resource that can be created, suspended, moved, and scaled.

### Desired capabilities

- logical long-lived identity
- persistent state independent of compute
- ephemeral or pooled execution
- suspend / resume
- isolation
- routing
- horizontal scale
- optional GPU workers

### Relevant systems

- Cloudflare Computer — authoritative durable workspace state + pluggable execution
- Agent Substrate — actor lifecycle + worker multiplexing + zero-trust isolation
- Rivet agentOS — in-process OS-like sandbox for lightweight execution
- GCE / containers / microVMs — conventional baselines

### Baseline experiment

```text
control plane
  → create logical agent environment
  → write state/files
  → execute task
  → suspend
  → execution disappears
  → resume on an available worker
  → state remains intact
```

A later benchmark should compare this against the simpler one-agent-one-VM model and against in-process WASM/V8 execution.

---

## Why both tracks matter

The physical track explores the strongest possible agent computer: a real machine with stable identity, local GPU, userland, credentials, devices, and long-lived caches.

The virtual track explores the opposite optimization: decouple logical state from scarce compute so large numbers of mostly-idle agents can share infrastructure efficiently.

The new in-process design point adds a third tradeoff: some “computer” semantics may be cheap enough to embed directly into the host process, escalating to a full sandbox only when required.

They should not be forced into one architecture prematurely.
