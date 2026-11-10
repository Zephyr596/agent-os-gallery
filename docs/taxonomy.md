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

For a dedicated physical agent node, native Linux is a cleaner experimental baseline than WSL2 because it removes an additional Windows/Hyper-V boundary from networking, devices, systemd, cgroups, Docker/containerd, GPU, and recovery behavior.

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

Defines the logical “computer” available to an agent independently of any one physical machine.

Strong references:

- [Cloudflare Computer](../projects/cloudflare-computer/README.md)
- [Rivet agentOS](../projects/rivet-agent-os/README.md)

Typical primitives:

- filesystem
- shell / JavaScript / Linux-like execution
- durable state
- process model
- networking / egress policy
- artifacts

A key 2026 distinction is that the logical computer can be implemented through very different execution boundaries:

### 3A. Durable state + pluggable execution

Cloudflare Computer keeps authoritative workspace state in a Durable Object / SQLite-backed filesystem and attaches container, isolate-shell, or codemode execution as needed.

Related abstraction: [Durable state + ephemeral execution](../abstractions/durable-state-ephemeral-execution.md).

### 3B. In-process OS-like sandbox

Rivet agentOS places a virtual filesystem, process table, pipes, PTYs, virtual networking, WASM utilities, and V8-isolated JavaScript inside the host application process.

This avoids a separate VM/container for many tasks, while retaining the option to escalate to a full sandbox when a browser, native binary, or full Linux environment is required.

Related abstraction: [In-process agent OS](../abstractions/in-process-agent-os.md).

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
- approvals
- scheduling / cron in some runtimes
- session persistence in some runtimes

Important distinction: a runtime/harness may call itself an “Agent OS” while still primarily belonging here. Naming is not architecture.

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
- identity
- network mediation

Agent Substrate's 1.0 design also makes security part of orchestration: gVisor or microVM isolation, per-agent identity, default-deny networking, and credential injection outside the sandbox.

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
- whether secrets ever enter untrusted agent execution

A notable complementary pattern appears in Agent Substrate: credentials can be injected by an egress gateway outside the sandbox, allowing outbound requests without placing reusable secrets inside the agent environment.

Related abstraction: **[Capability-mediated authority](../abstractions/capability-mediated-authority.md)**.

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
- secret/credential placement
- cold start / resume latency
- escalation from lightweight to heavyweight execution

## A useful distinction

A project can operate at one layer while contributing an abstraction relevant to several layers.

Cloudflare OS is a good example. As a product it sits near the human-agent workspace layer, but its most valuable contribution to this gallery is currently the Gatekeeper capability model in the security plane. It also serves as a dogfooding/reference environment for lower-level Workers primitives.

Agent Substrate is another example: it is primarily scheduler/runtime infrastructure, but its external credential-injection model is also directly relevant to the security plane.

This taxonomy is intentionally provisional and should evolve as more systems are tested.
