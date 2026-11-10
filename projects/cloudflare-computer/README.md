# Cloudflare Computer

Upstream: https://github.com/cloudflare/computer

## Layer

Virtual agent computer / sandbox abstraction.

## Most valuable abstraction

**[Durable state + ephemeral execution](../../abstractions/durable-state-ephemeral-execution.md)**

Cloudflare Computer's strongest idea is that a logical computer does not have to equal one permanently running VM or container.

## Core architecture

The authoritative virtual filesystem lives in a Durable Object backed by SQLite. Execution backends attach lazily to that state through a single runtime surface.

Current upstream backends include:

- **Container** — full Linux userland, real binaries, network, with the workspace projected through FUSE and synchronized by `computerd`;
- **Isolate shell** — `just-bash` inside a Dynamic Worker, talking directly to the authoritative workspace over Workers RPC;
- **Codemode** — ECMAScript modules in a fresh Dynamic Worker with workspace-backed filesystem access and host modules.

```text
authoritative durable workspace state
                │
       workspace.runtime.exec(...)
                │
      ┌─────────┼──────────┐
      ▼         ▼          ▼
  container   isolate    codemode
   / FUSE      shell       JS
```

The core architectural bet remains:

```text
durable logical state + replaceable execution
```

instead of:

```text
one logical agent = one permanently running machine
```

## Why it matters

The model allows a logical workspace to persist independently of which execution engine is active.

That makes several escalation strategies possible:

- run cheap shell-like work in an isolate;
- run structured code in codemode;
- attach a full Linux container only when native binaries or a richer userland are required.

This is not just suspend/resume. It is **execution-class substitution over one authoritative workspace**.

## Relationship to Cloudflare OS

Cloudflare Computer and Cloudflare OS remain separate projects, but both explore the broader Workers-native direction:

- isolate-first execution,
- persistent state independent of compute lifetime,
- escalation to heavier runtimes only when required,
- explicit security and egress boundaries.

Cloudflare OS is the product/security dogfooding environment; Computer is a cleaner lower-level computer abstraction.

## Current status — November 2026

The upstream README now says the top-level `@cloudflare/computer` package is **suitable for production use**, while several components and backends — including the container path and Worker shell pieces — remain experimental.

The repository also now includes examples for:

- container and Worker-shell execution,
- codemode,
- matching egress policies across backends,
- MCP,
- agent integrations,
- runtime comparison,
- artifacts and generated projects.

The correct reading is therefore no longer “the whole project is preview-only”; maturity is component-specific.

## Experiments to run

- run the tutorial end-to-end;
- compare codemode, Worker shell, and container execution on the same task;
- run real git/npm/build workloads;
- destroy/recreate execution and verify workspace persistence;
- inspect egress enforcement across backends;
- measure backend activation latency;
- benchmark metadata-heavy vs sequential filesystem behavior;
- evaluate cost versus one persistent VM per agent;
- explore whether GPU execution should be another backend or remain outside this abstraction.
