# Evaluation Framework

Use this checklist to evaluate systems in the gallery consistently.

## Identity

- What is the primary unit: human, agent, workspace, actor, process, machine?
- Does the agent have its own identity?
- Is the agent accountable to a human identity?

## State and persistence

- Where is authoritative state stored?
- Is process memory persistent?
- Is filesystem state persistent?
- Can state survive execution teardown?
- Can state move between workers or machines?

## Execution

- Full Linux, container, microVM, isolate, browser, JS runtime, or mixed?
- Is shell available?
- Can arbitrary binaries run?
- Can Docker run?
- Is local GPU/CUDA available?

## Lifecycle

- Create / destroy latency
- Suspend / resume support
- Snapshot semantics
- Migration / relocation
- Multiplexing / oversubscription
- Unattended restart and recovery

## Security

- Sandbox boundary
- Network egress defaults
- Secret storage
- Credential scope
- ACL vs capability model
- Ambient authority present?
- Human approval model
- Audit trail

## Tools and external systems

- CLI, REST, RPC, MCP, ACP, browser automation, UI automation?
- How are GitHub / Google / SaaS connections represented?
- Are tools global or introduced per task/resource?

## Model layer

- Supported providers
- Local models?
- Model routing abstraction
- Tool calling compatibility
- Reasoning compatibility
- Context-window handling
- Prompt caching / compaction
- Failure behavior when schemas differ

## Human experience

- Is there a workspace or control plane?
- Can humans inspect state and actions?
- Can humans and agents edit the same artifacts?
- Is approval synchronous or asynchronous?

## Infrastructure

- Physical-first or cloud-first?
- Self-hosting maturity
- Kubernetes dependency
- Cloud lock-in
- Tailscale / private network friendliness
- Observability

## Economics

Record real experiments when possible:

- model input/output tokens
- compute unit / neuron usage
- tool calls
- runtime duration
- cloud infrastructure cost
- idle cost

## Evidence levels

Tag findings when useful:

- **Observed** — reproduced in a demo
- **Source** — verified from code
- **Documented** — stated in project docs
- **Hypothesis** — interpretation that still needs testing
