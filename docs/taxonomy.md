# Working Taxonomy

The term **Agent OS** currently covers multiple layers that should not be collapsed into one category.

This document classifies **where a system operates**. The [abstraction index](../abstractions/README.md) captures **what architectural idea is worth carrying forward** from each system.

## 1. Host OS / Machine Layer

The actual machine substrate: physical or virtual.

Examples:

- Ubuntu / Ubuntu Server
- Windows + WSL2
- Omarchy
- GCE VM
- containers / microVMs

Questions:

- unattended boot and recovery
- GPU / CUDA access
- local disk and persistence
- device access
- remote administration

Related abstraction: [Human-centric host as agent substrate](../abstractions/human-centric-host.md).

## 2. Agent System Layer

Software that makes the agent a first-class participant in the host system.

Strong reference: [Claw OS](../projects/claw-os/README.md).

Typical responsibilities:

- process / log / network inspection
- system-level actions
- persistent memory
- model abstraction
- capability checks and approvals

Related abstraction: [Agent as a system service](../abstractions/agent-as-system-service.md).

## 3. Computer / Sandbox Abstraction

Defines the logical "computer" available to an agent independently of any one physical machine.

Strong reference: [Cloudflare Computer](../projects/cloudflare-computer/README.md).

Typical primitives:

- filesystem
- shell / JavaScript / Linux execution
- durable state
- egress policy
- artifacts

A key design question is whether the computer is:

- one persistent VM, or
- durable state plus ephemeral execution.

Related abstraction: [Durable state + ephemeral execution](../abstractions/durable-state-ephemeral-execution.md).

## 4. Agent Runtime / Harness

The loop that turns model outputs into actions.

Examples include Claude Code, Codex, OpenCode, and custom harnesses.

Typical responsibilities:

- prompt/context assembly
- model routing
- tool calls
- retries
- compaction
- reasoning/tool-result lifecycle

Important cross-cutting lesson from the Cloudflare OS experiment: model compatibility is part of runtime design. Context windows, output budgeting, tool schemas, reasoning representations, and provider adapters can all leak through a supposedly generic model interface.

## 5. Scheduling / Orchestration

Manages many long-lived agent processes or logical computers.

Strong reference: [Agent Substrate](../projects/agent-substrate/README.md).

Typical responsibilities:

- create / destroy
- suspend / resume
- placement
- multiplexing
- routing
- snapshots
- autoscaling

Related abstraction: [Actor-worker multiplexing](../abstractions/actor-worker-multiplexing.md).

## 6. Capability / Security Plane

Controls what agents and generated applications may access.

Strong reference: [Cloudflare OS Gatekeepers](../projects/cloudflare-os/README.md).

Questions:

- ambient authority vs explicit introduction
- ACL vs capability security
- credential possession vs delegated authority
- resource-level scope
- auditability
- human approval
- simulated vs synchronous approval
- revocation / delegation

Related abstraction: **[Capability-mediated authority](../abstractions/capability-mediated-authority.md)**.

This layer deserves special treatment because autonomous agents are neither ordinary applications nor ordinary human users. A useful security model should allow a human principal to delegate narrow authority to an accountable agent without handing over every credential the human possesses.

## 7. Human-Agent Workspace / Organizational Control Plane

The interface and durable organizational layer in which humans supervise, collaborate with, and delegate to agents.

Strong references:

- [Buzz](../projects/buzz/README.md)
- [Cloudflare OS](../projects/cloudflare-os/README.md)

Questions:

- Are agents bots or first-class identities?
- Where does organizational memory live?
- How are actions reviewed?
- How are git/workflow events represented?
- Can humans and agents operate on the same artifacts?
- How is authority introduced and audited?

Related abstraction: [Unified human-agent event log](../abstractions/unified-human-agent-event-log.md).

## Cross-cutting concerns

Some questions span every layer:

- identity
- state
- persistence
- filesystem
- networking
- GPU
- model routing
- observability
- cost
- security
- recovery

## A useful distinction

A project can operate at one layer while contributing an abstraction relevant to several layers.

Cloudflare OS is a good example. As a product it sits near the human-agent workspace layer, but its most valuable contribution to this gallery is currently the Gatekeeper capability model in the security plane. It also serves as a dogfooding/reference environment for lower-level Workers primitives.

This taxonomy is intentionally provisional and should evolve as more systems are tested.
