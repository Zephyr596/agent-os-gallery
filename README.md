# Agent OS Gallery

A research gallery of **agent-native operating systems, runtimes, computers, sandboxes, orchestration layers, security models, and human-agent workspaces**.

This repository is intentionally **research-first**. The goal is not to build another Agent OS yet, but to map the design space, run real demos, compare architectural choices, and extract the abstractions that remain useful beyond any one project.

## Start here: the abstractions

The main output of this repository is not a list of projects. It is a map of the architectural ideas those projects are testing.

| Abstraction | Strong reference | Core question |
|---|---|---|
| **[Capability-mediated authority](abstractions/capability-mediated-authority.md)** | Cloudflare OS / Gatekeepers | How should agents receive narrow authority without inheriting a user's full credentials? |
| **[Durable state + ephemeral execution](abstractions/durable-state-ephemeral-execution.md)** | Cloudflare Computer | Does an agent need a permanent VM, or durable state plus execution on demand? |
| **[Actor-worker multiplexing](abstractions/actor-worker-multiplexing.md)** | Agent Substrate | How can many long-lived logical agents share a smaller worker pool? |
| **[In-process agent OS](abstractions/in-process-agent-os.md)** | Rivet agentOS | When can a POSIX-like agent computer live inside the application process instead of in a container or VM? |
| **[Agent as a system service](abstractions/agent-as-system-service.md)** | Claw OS | What changes when an agent is part of the host system instead of an app sandbox? |
| **[Unified human-agent event log](abstractions/unified-human-agent-event-log.md)** | Buzz | How should humans, agents, git, workflows, and approvals share identity and history? |
| **[Human-centric host as agent substrate](abstractions/human-centric-host.md)** | Omarchy | Should a physical Agent Computer be a developer workstation or an unattended appliance? |

See the full [abstraction index](abstractions/README.md).

## Why abstraction-first?

The term **Agent OS** is overloaded.

Cloudflare OS, Cloudflare Computer, Agent Substrate, Rivet agentOS, Claw OS, Buzz, and Omarchy can all be relevant to Agent OS research while solving fundamentally different problems. Treating them as direct competitors hides the useful architectural decisions.

A better question is:

> **What does an AI agent actually need from an operating system, and which layer should provide it?**

The emerging stack appears to include some combination of:

```text
Human / operator
      │
      ▼
Human-agent workspace / organizational control plane
      │
      ▼
Capability / security plane
      │
      ▼
Agent runtime / harness
      │
      ▼
Computer / sandbox abstraction
      │
      ▼
Scheduling / lifecycle / multiplexing
      │
      ▼
Host OS / physical or virtual machine
```

An important 2026 refinement is that the **computer / sandbox abstraction no longer implies a separate machine boundary**. It can be implemented as:

- a durable logical filesystem with pluggable execution backends (Cloudflare Computer),
- a stateful actor restored onto a worker pool (Agent Substrate), or
- an in-process WASM/V8 operating-system-like kernel (Rivet agentOS).

See [docs/taxonomy.md](docs/taxonomy.md) for the working taxonomy.

## Project studies

Project pages are evidence for the abstraction map. Each page highlights the project's **most valuable abstraction**, observed behavior, and experiments to run next.

| Project | Primary layer | Key abstraction |
|---|---|---|
| [Cloudflare OS](projects/cloudflare-os/README.md) | company AI workspace / capability OS / Workers reference architecture | [Capability-mediated authority](abstractions/capability-mediated-authority.md) |
| [Cloudflare Computer](projects/cloudflare-computer/README.md) | virtual agent computer | [Durable state + ephemeral execution](abstractions/durable-state-ephemeral-execution.md) |
| [Agent Substrate](projects/agent-substrate/README.md) | zero-trust agent infrastructure runtime / scheduler | [Actor-worker multiplexing](abstractions/actor-worker-multiplexing.md) |
| [Rivet agentOS](projects/rivet-agent-os/README.md) | in-process agent computer / sandbox library | [In-process agent OS](abstractions/in-process-agent-os.md) |
| [Claw OS](projects/claw-os/README.md) | physical-machine agent system layer | [Agent as a system service](abstractions/agent-as-system-service.md) |
| [Buzz](projects/buzz/README.md) | human-agent collaboration plane | [Unified human-agent event log](abstractions/unified-human-agent-event-log.md) |
| [Omarchy](projects/omarchy/README.md) | human-centric Linux host | [Human-centric host as agent substrate](abstractions/human-centric-host.md) |

See the [project index](projects/README.md).

## Current landscape notes — November 2026

Three changes since the first version of this gallery are especially important:

1. **Agent Substrate has matured into a 1.0 runtime** with explicit zero-trust isolation, per-agent identity, credential injection outside the sandbox, default-deny networking, and actor hibernation/resume.
2. **Cloudflare Computer has become a clearer production-oriented abstraction**: authoritative SQLite-backed workspace state plus pluggable container, isolate-shell, and codemode execution backends. The top-level package is now described upstream as suitable for production use, while several backends remain experimental.
3. **Rivet agentOS adds a distinct design point**: a POSIX-like agent computer implemented as an in-process JavaScript kernel with WebAssembly utilities and V8 isolates, with optional escalation to a full sandbox for workloads that require native Linux.

This shifts the research question from “which Agent OS wins?” toward:

> **Which state, execution, isolation, authority, and lifecycle boundaries belong together — and which should remain separable?**

## A current example: how to read Cloudflare OS

The hosted product experience itself is not the most interesting part of Cloudflare OS.

A stronger interpretation is:

> **Cloudflare OS is a dogfooding environment and reference architecture for Cloudflare's agent infrastructure.**

It combines Access, Durable Objects, Dynamic Workers, Facets, Gatekeepers, Cap'n Web, AI Gateway, Workers AI, KV, and R2 in a real internal workload. The project is valuable because it exposes what the Workers team is trying to learn about AI-generated software, capability security, stateful agents, and isolate-first execution.

Its most important abstraction in this gallery is the **Gatekeeper security model**:

```text
human owns broad authority
        │
        ▼
explicitly introduce one resource
        │
        ▼
Gatekeeper creates narrow capability
        │
        ▼
agent / generated app receives capability
        │
        ▼
audited / approval-mediated side effects
```

Read: **[Capability-mediated authority](abstractions/capability-mediated-authority.md)**.

## Two practical research tracks

### 1. Physical Agent Computer

Give a real GPU laptop or workstation to an agent for long-lived use.

Current baseline:

```text
remote operator
  → private network
  → native Ubuntu on physical ThinkBook
  → long-lived agent runtime
  → shell / git / GitHub / Docker / CUDA
  → test
  → commit / PR
```

The working preference is **native Ubuntu rather than WSL2** for the dedicated agent node, while retaining Windows as a dual-boot fallback. This removes the extra Windows/Hyper-V/WSL boundary when studying system services, networking, containers, cgroups, device access, GPU behavior, and unattended recovery.

See [Physical vs Virtual](docs/physical-vs-virtual.md).

### 2. Managed / Virtual Agent Computer

Make the logical computer disposable, suspendable, migratable, and schedulable.

```text
control plane
  → logical agent computer
  → persistent state
  → ephemeral / pooled execution
  → suspend
  → resume elsewhere
  → state still intact
```

Cloudflare Computer, Agent Substrate, and Rivet agentOS represent three different ways to reduce the cost of “one agent = one permanent VM”.

## Experiments

The gallery is not README-only research. When possible, claims should be tested with runnable demos.

Current experiment log:

- [Cloudflare OS — first deployment and Gadget experiments](experiments/cloudflare-os/2026-08-26.md)

The first Cloudflare OS experiment already surfaced useful systems findings:

- static UI and RPC/control-plane health can diverge;
- one generated Slides task consumed hundreds of thousands of model tokens;
- arbitrary model replacement failed for two different abstraction reasons: context budgeting and message/tool schema compatibility.

These failures are more useful to this repository than polished screenshots because they reveal where platform abstractions actually leak.

## Evaluation framework

Every project should eventually be evaluated against the same questions:

- What is the unit of identity: user, agent, workspace, process, actor, VM?
- Where is authoritative state stored?
- Is the filesystem persistent?
- Is execution full Linux, isolate, browser, VM, WASM, or mixed?
- Is the isolation boundary the process, isolate, container, gVisor sandbox, or microVM?
- Can it use a local GPU?
- What is the sandbox / security boundary?
- How are external tools and credentials exposed?
- Is authority ambient, ACL-based, or capability-based?
- Can credentials remain outside untrusted agent code?
- Can the agent suspend / resume / migrate?
- How are humans asked to approve side effects?
- How replaceable is the model provider?
- What happens when model/tool schemas do not match the runtime abstraction?
- What is the real cost of a non-trivial agent trajectory?

See [docs/evaluation-framework.md](docs/evaluation-framework.md).

## Principles for this repo

1. **Abstractions over logos.** The durable research output is the architectural idea, not the project name.
2. **Demo over marketing.** Prefer observed behavior over product positioning.
3. **Compare by layer.** Do not compare a Linux distribution with a scheduler as if they solve the same problem.
4. **Separate persistent state from execution.** This is one of the central architectural choices in agent infrastructure.
5. **Treat authority as a first-class systems problem.** Agents should not automatically inherit every credential their human can access.
6. **Treat isolation as a spectrum, not a boolean.** WASM/V8, gVisor, microVMs, containers, and real hosts make different tradeoffs.
7. **Treat model compatibility as a systems problem.** “Supports function calling” does not imply drop-in compatibility with an agent runtime.
8. **Do not build prematurely.** First map what exists, test it, and find the real gaps.

## Status

Active research. APIs and projects in this space are changing quickly; notes are dated where behavior is likely to change.
