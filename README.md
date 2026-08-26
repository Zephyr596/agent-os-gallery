# Agent OS Gallery

A research gallery of **agent-native operating systems, runtimes, computers, sandboxes, orchestration layers, and human-agent workspaces**.

This repository is intentionally **research-first**. The current goal is not to build another Agent OS, but to map the design space, run real demos, compare architectural choices, and identify the gaps that remain after existing systems are understood.

## The core question

What does an AI agent actually need from an "operating system"?

At minimum, the emerging systems in this space seem to provide some combination of:

- long-lived identity
- persistent state and filesystem
- execution environments
- model access and routing
- tools and external capabilities
- permission / capability boundaries
- suspend, resume, migration, and scheduling
- human oversight and collaboration

The term **Agent OS** is currently overloaded. This gallery separates projects by layer instead of treating every project with "OS" in its name as a direct competitor.

## Research map

```text
Human / operator
      │
      ▼
Human-agent workspace / control plane
      │       Buzz · Cloudflare OS
      ▼
Agent runtime / harness
      │       Claude Code · Codex · OpenCode · custom agents
      ▼
Computer / sandbox abstraction
      │       Cloudflare Computer · Claw OS system layer
      ▼
Scheduling / orchestration
      │       Agent Substrate · Kubernetes / GKE
      ▼
Host OS / physical or virtual machine
              Ubuntu · WSL2 · Omarchy · GCE · containers · microVMs
```

See [docs/taxonomy.md](docs/taxonomy.md) for the working taxonomy.

## Initial gallery

| Project | Layer | Core bet |
|---|---|---|
| [Cloudflare OS](gallery/cloudflare-os.md) | company AI workspace / capability OS | Agents, Gadgets, and external resources should be governed by capability-based security |
| [Cloudflare Computer](gallery/cloudflare-computer.md) | virtual agent computer | A durable filesystem plus pluggable execution backends can replace one-VM-per-agent for much work |
| [Agent Substrate](gallery/agent-substrate.md) | agent infrastructure / scheduler | Many long-lived stateful actors can be multiplexed over fewer workers |
| [Claw OS](gallery/claw-os.md) | physical machine agent system layer | The agent should be a privileged, capability-gated system service, not an app sandbox |
| [Buzz](gallery/buzz.md) | human-agent collaboration plane | Humans, agents, workflows, and git events should share one identity/event model |
| [Omarchy](gallery/omarchy.md) | human-centric Linux host OS | A highly opinionated developer desktop can be the host environment, but is not itself an Agent OS |

## Two practical research tracks

### 1. Physical Agent Computer

Give a real GPU laptop or workstation to an agent for long-lived use.

Target loop:

```text
remote operator
  → private network
  → physical machine
  → agent
  → shell / git / GitHub / Docker / CUDA
  → test
  → commit / PR
```

Questions include unattended boot, GPU access, credential boundaries, machine recovery, filesystem persistence, and whether Windows+WSL2, minimal Linux, or a desktop distribution is the right host.

### 2. Managed / Virtual Agent Computer

Make the "computer" itself disposable, suspendable, migratable, and schedulable.

Target loop:

```text
control plane
  → logical agent computer
  → persistent state
  → ephemeral / pooled execution
  → suspend
  → resume elsewhere
  → state still intact
```

Cloudflare Computer and Agent Substrate are especially relevant to this track.

## Evaluation framework

Every project should eventually be evaluated against the same questions:

- What is the unit of identity: user, agent, workspace, process, actor, VM?
- Where is authoritative state stored?
- Is the filesystem persistent?
- Is execution full Linux, isolate, browser, VM, or something else?
- Can it use a local GPU?
- What is the sandbox / security boundary?
- How are external tools and credentials exposed?
- Is authority ambient, ACL-based, or capability-based?
- Can the agent suspend / resume / migrate?
- How are humans asked to approve side effects?
- How replaceable is the model provider?
- What happens when the model/tool schema does not match the runtime abstraction?
- What is the real cost of a non-trivial agent trajectory?

See [docs/evaluation-framework.md](docs/evaluation-framework.md).

## Experiments

The gallery is not README-only research. When possible, claims should be tested with runnable demos.

Current experiment log:

- [Cloudflare OS — first deployment and Gadget experiments](experiments/cloudflare-os/2026-08-26.md)

## Principles for this repo

1. **Demo over marketing.** Prefer observed behavior over product positioning.
2. **Compare by layer.** Do not compare an operating system distribution with a scheduler as if they solve the same problem.
3. **Separate persistent state from execution.** This is one of the central architectural choices in agent infrastructure.
4. **Treat model compatibility as a systems problem.** "Supports function calling" does not imply drop-in compatibility with an agent runtime.
5. **Do not build prematurely.** First map what exists and where the real gaps are.

## Status

Early research. APIs and projects in this space are changing quickly; notes are dated where behavior is likely to change.
