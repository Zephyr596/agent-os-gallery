# Working Taxonomy

The term **Agent OS** currently covers multiple layers that should not be collapsed into one category.

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

## 2. Agent System Layer

Software that makes the agent a first-class participant in the host system.

Example: Claw OS.

Typical responsibilities:

- process / log / network inspection
- system-level actions
- persistent memory
- model abstraction
- capability checks and approvals

## 3. Computer / Sandbox Abstraction

Defines the logical "computer" available to an agent independently of any one physical machine.

Example: Cloudflare Computer.

Typical primitives:

- filesystem
- shell / JavaScript / Linux execution
- durable state
- egress policy
- artifacts

A key design question is whether the computer is:

- one persistent VM, or
- durable state plus ephemeral execution.

## 4. Agent Runtime / Harness

The loop that turns model outputs into actions.

Examples include coding agents and custom harnesses.

Typical responsibilities:

- prompt/context assembly
- model routing
- tool calls
- retries
- compaction
- reasoning/tool-result lifecycle

## 5. Scheduling / Orchestration

Manages many long-lived agent processes or logical computers.

Example: Agent Substrate.

Typical responsibilities:

- create / destroy
- suspend / resume
- placement
- multiplexing
- routing
- snapshots
- autoscaling

## 6. Capability / Security Plane

Controls what agents and applications may access.

Cloudflare OS Gatekeepers are a strong example.

Questions:

- ambient authority vs explicit introduction
- ACL vs capability security
- credential scope
- auditability
- human approval
- simulated vs synchronous approval

## 7. Human-Agent Workspace / Control Plane

The interface in which humans supervise, collaborate with, and delegate to agents.

Examples:

- Buzz
- Cloudflare OS

Questions:

- Are agents bots or first-class identities?
- Where does organizational memory live?
- How are actions reviewed?
- How are git/workflow events represented?
- Can humans and agents operate on the same artifacts?

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

This taxonomy is intentionally provisional and should evolve as more systems are tested.
