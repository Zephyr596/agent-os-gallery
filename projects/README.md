# Projects

Project pages are implementation studies. The repository is organized **abstraction-first**: each project page links to the architectural idea that appears most valuable after reading the design and, where possible, running the software.

| Project | Primary layer | Most valuable abstraction |
|---|---|---|
| [Cloudflare OS](cloudflare-os/README.md) | human-agent workspace / capability OS | [Capability-mediated authority](../abstractions/capability-mediated-authority.md) |
| [Cloudflare Computer](cloudflare-computer/README.md) | virtual agent computer | [Durable state + ephemeral execution](../abstractions/durable-state-ephemeral-execution.md) |
| [Agent Substrate](agent-substrate/README.md) | scheduler / lifecycle infrastructure | [Actor-worker multiplexing](../abstractions/actor-worker-multiplexing.md) |
| [Claw OS](claw-os/README.md) | physical-machine agent system layer | [Agent as a system service](../abstractions/agent-as-system-service.md) |
| [Buzz](buzz/README.md) | human-agent collaboration plane | [Unified human-agent event log](../abstractions/unified-human-agent-event-log.md) |
| [Omarchy](omarchy/README.md) | human-centric host OS | [Human-centric host as agent substrate](../abstractions/human-centric-host.md) |

## Reading order

For a new reader, start with [Abstractions](../abstractions/README.md), then open project pages when you want the concrete implementation, evidence, and experiments behind an idea.

For a new contributor, a project page should answer four things:

1. What layer does this project actually belong to?
2. What is its strongest abstraction?
3. What evidence comes from real use rather than marketing/docs?
4. What should be tested next?
