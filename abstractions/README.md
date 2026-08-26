# Abstractions

This directory is the center of the gallery.

Projects come and go. Product names change. The useful output of this research is the set of **architectural abstractions that survive those implementations**.

Each project page should point to the abstraction that appears most valuable or distinctive after reading the design and, where possible, running the software.

## Current abstraction map

| Abstraction | Strong reference | Question it answers |
|---|---|---|
| [Capability-mediated authority](capability-mediated-authority.md) | Cloudflare OS / Gatekeepers | How should an agent receive external authority without inheriting all of a user's credentials? |
| [Durable state + ephemeral execution](durable-state-ephemeral-execution.md) | Cloudflare Computer | Does an agent need a permanently running VM, or only durable state plus execution on demand? |
| [Actor-worker multiplexing](actor-worker-multiplexing.md) | Agent Substrate | How can many long-lived logical agents share a smaller pool of physical workers? |
| [Agent as a system service](agent-as-system-service.md) | Claw OS | What changes when an agent is part of a real machine's system layer rather than trapped in an app sandbox? |
| [Unified human-agent event log](unified-human-agent-event-log.md) | Buzz | How should humans, agents, git activity, workflows, and approvals share identity and history? |
| [Human-centric host as agent substrate](human-centric-host.md) | Omarchy | When does a developer desktop make sense as the physical host for an always-on agent? |

## Why abstraction-first?

Calling all of these systems "Agent OS" hides the important differences.

For example:

- Cloudflare OS is primarily interesting for capability security and AI-generated application isolation.
- Cloudflare Computer is primarily interesting for separating durable computer state from compute.
- Agent Substrate is primarily interesting for lifecycle and multiplexing at scale.
- Claw OS is primarily interesting for making the agent a first-class participant in a physical host.
- Buzz is primarily interesting for organizational identity and shared event history.
- Omarchy is primarily a host-environment reference, not an agent runtime.

The purpose of the project pages is therefore not just to summarize repositories. They should provide evidence for, limitations of, and real-world observations about these abstractions.

## Adding a new project

Before adding a project, ask:

1. Which layer does it actually operate at?
2. What is the strongest architectural idea it contributes?
3. Is that idea already represented by an existing abstraction?
4. What can be verified by a demo rather than repeated from a README?
5. What failure modes or tradeoffs does the implementation reveal?

If a project does not add a new abstraction, it can still be valuable as a contrasting implementation of an existing one.
