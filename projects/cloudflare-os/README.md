# Cloudflare OS

Upstream: https://github.com/cloudflare/cloudflare-os

## Layer

Human-agent workspace / capability-oriented Agent OS / Workers agent-platform reference architecture.

## Most valuable abstraction

**[Capability-mediated authority](../../abstractions/capability-mediated-authority.md)**

Cloudflare OS's strongest contribution is not the chat UI or office-like experience. It is the security model around agents, generated applications, and external resources.

Agents and Gadgets begin with no ambient authority. A user explicitly introduces resources, and Gatekeepers turn broad account credentials into narrow resource capabilities with audit and approval semantics.

## Secondary abstraction

Cloudflare OS also provides a concrete model for AI-generated, user-owned applications:

```text
Blueprint
   │
   ▼
per-user Gadget instance
   │
   ├── generated / editable code
   ├── sandboxed Dynamic Worker execution
   └── Durable Object state
```

The assumption is that generated code is not trusted. Safety comes from the runtime and capability boundary rather than from expecting model-generated software to be correct.

## Product experience vs research value

After running the hosted deployment and building a real Slides Gadget, the product itself did not feel unusually compelling as an end-user productivity environment.

At the surface it provides familiar pieces:

- agent chat,
- workspaces,
- generated Slides / Sheets / other Gadgets,
- external integrations,
- model selection.

The more important interpretation is that Cloudflare OS is a **dogfooding environment and reference implementation for Cloudflare's agent infrastructure**.

It exercises a large fraction of the Workers platform in one real workload:

```text
Cloudflare Access
      │
      ▼
human identity
      │
      ▼
frontend / RPC control channel
      │
      ▼
Workshop backend
      │
      ├── Durable Objects          → state / coordination
      ├── Dynamic Workers / Facets → generated app isolation
      ├── Gatekeepers              → capability security
      ├── KV / R2                  → metadata / content
      ├── AI Gateway               → model routing / observability
      └── Workers AI               → inference
```

The project README explicitly notes that Dynamic Workers, Facets, and other runtime features were developed in part to support this workload. That makes Cloudflare OS useful as a window into how the Workers team thinks an agent-native application platform should evolve.

## OS analogy

Cloudflare OS describes itself using an operating-system analogy:

- backend → kernel
- Gatekeepers → device drivers
- frontend → shell
- Gadgets → processes
- Blueprints → executables/templates
- users → users
- sharing permissions → ACL-like permissions
- agents → a new first-class actor not cleanly represented in traditional OSes

The analogy is imperfect but useful because the backend genuinely coordinates programs, state, users, external capabilities, and isolation boundaries.

## Important primitives

### Durable Objects

Workspace and Gadget state are long-lived and coordinated through stateful Durable Objects.

### Dynamic Workers / Facets

AI-generated server code runs in isolated Workers rather than sharing one application process.

### Gadgets

A Gadget is closer to a user-owned application instance than a plugin or document. Code, runtime, and state belong to that instance and can be modified by the agent.

### Blueprints

Blueprints distribute application code/templates rather than one centralized SaaS instance.

### Gatekeepers

Gatekeepers mediate external authority. They are the project's most important security abstraction; see [Capability-mediated authority](../../abstractions/capability-mediated-authority.md).

## Observed demo results

See the dated [experiment log](../../experiments/cloudflare-os/2026-08-26.md).

Notable observations:

- one-click deployment provisioned multiple Workers, storage resources, AI Gateway configuration, and Cloudflare Access;
- static UI can remain healthy while the WebSocket/RPC control plane is broken;
- re-authenticating Cloudflare Access restored the RPC channel in our test;
- a Slides Blueprint successfully created the first Gadget;
- the Slides task used roughly 343k input tokens + 53k output tokens and about 55k Workers AI neurons;
- model compatibility is a runtime systems problem, not just a model feature checkbox;
- Qwen3 30B failed because the runtime reserved a 32k output budget against a 32k context window;
- GPT-OSS 20B failed on structured multi-turn/tool-call message schema compatibility.

These failures are useful evidence that a generic Agent OS needs a stronger model abstraction layer than "provider + model id".

## Research interpretation

The project is most interesting when asking:

> What assumptions about future agent infrastructure is Cloudflare trying to validate through a real internal product?

A working hypothesis is:

> The future will contain large numbers of AI-generated, stateful, intermittently active, untrusted applications and agents. Giving every workload a permanent VM/container and broad user credentials is the wrong default.

Cloudflare OS explores an alternative based on:

- cheap isolate-first execution,
- durable state,
- explicit capabilities,
- user-owned generated software,
- platform-level identity and audit.

## Relationship to Cloudflare Computer

Cloudflare OS and Cloudflare Computer are currently separate projects rather than a direct dependency chain.

Conceptually, however, they appear to explore adjacent parts of the same thesis:

```text
Cloudflare OS
= product/workspace/security experiment

Cloudflare Computer
= durable-computer abstraction experiment
```

Cloudflare Computer's separation of durable filesystem state from replaceable execution may eventually be relevant to heavier developer workloads that do not fit naturally inside Dynamic Workers.

## Questions to revisit

- How portable is a production deployment to standalone `workerd`?
- How does Gatekeeper speculative execution preserve correctness across dependent side effects?
- What are the revocation and delegation semantics of introduced capabilities?
- Can the Gadget model support heavyweight software-development workflows?
- Will Cloudflare Computer become a lower-level execution substrate for Cloudflare OS?
- How should arbitrary model context/output limits be represented instead of relying on provider-level defaults?
- What is the long-term economics of high-token agent trajectories?
