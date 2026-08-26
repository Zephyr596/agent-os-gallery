# Cloudflare OS

Repo: https://github.com/cloudflare/cloudflare-os

## Layer

Human-agent workspace / capability-oriented Agent OS.

## Core idea

Cloudflare OS treats agents, user-built applications (Gadgets), external services (Gatekeepers), and persistent workspace state as OS-like primitives.

Its own analogy is:

- backend → kernel
- Gatekeepers → device drivers
- frontend → shell
- Gadgets → processes
- Blueprints → executables/templates

## Important primitives

- **Durable Objects** — persistent state and coordination
- **Gadgets** — per-user application instances generated or modified by agents
- **Blueprints** — shareable application code/templates used to instantiate Gadgets
- **Gatekeepers** — capability-aware wrappers around external resources
- **Dynamic Workers / Facets** — sandboxed execution for Gadget code
- **Code Mode agent** — performs work by generating and executing code

## Security bet

Agents and Gadgets should not inherit broad ambient access. Resources are explicitly introduced, and Gatekeepers scope access, audit operations, and mediate side effects.

## Observed in demo

See [experiment log](../experiments/cloudflare-os/2026-08-26.md).

Notable observations:

- one-click deployment provisions multiple Workers, storage, AI Gateway, and Access configuration
- the frontend relies on a persistent RPC/WebSocket connection to the backend
- stale Access authentication can leave static UI loaded while the RPC plane stays in `Reconnecting…`
- model/runtime compatibility is not automatic even among models hosted by the same provider

## Questions to revisit

- How portable is a production deployment to standalone `workerd`?
- How does Gatekeeper simulation preserve correctness across chained side effects?
- Can the Gadget/Dynamic Worker model support heavyweight developer workflows?
- Will Cloudflare Computer become a lower-level execution substrate for Cloudflare OS?
