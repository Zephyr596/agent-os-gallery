# Physical vs Virtual Agent Computers

This gallery currently follows two practical research tracks.

## Track A — Physical Agent Computer

Goal: hand a real GPU laptop/workstation to an agent for long-lived use.

### Desired capabilities

- persistent filesystem
- shell
- git + GitHub CLI
- Docker
- local GPU / CUDA
- local model serving when useful
- remote access over a private network
- unattended boot/recovery
- durable credentials with narrow scope

### Candidate host setups

1. Windows + WSL2
2. Minimal / server-oriented Linux
3. Human-oriented Linux desktop such as Omarchy

### Candidate agent/system layers

- Claw OS
- coding-agent harnesses
- custom daemon + existing coding agents

### Baseline experiment

```text
Mac / phone
  → Tailscale
  → physical GPU laptop
  → agent receives issue/task
  → clone / edit / test
  → local or cloud inference
  → commit / PR
```

The main question is not just whether this can work, but whether it remains reliable when the human does not touch the machine for days.

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

- Cloudflare Computer — durable filesystem + pluggable execution
- Agent Substrate — actor lifecycle + worker multiplexing
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

A later benchmark should compare this against the simpler one-agent-one-VM model.

---

## Why both tracks matter

The physical track explores the strongest possible agent computer: a real machine with stable identity, local GPU, userland, credentials, and long-lived caches.

The virtual track explores the opposite optimization: decouple logical state from scarce compute so large numbers of mostly-idle agents can share infrastructure efficiently.

They should not be forced into one architecture prematurely.
