# Cloudflare Computer

Upstream: https://github.com/cloudflare/computer

## Layer

Virtual agent computer / sandbox abstraction.

## Most valuable abstraction

**[Durable state + ephemeral execution](../../abstractions/durable-state-ephemeral-execution.md)**

Cloudflare Computer's strongest idea is that a logical computer does not have to equal one permanently running VM or container.

## Core architecture

The authoritative filesystem lives in a Durable Object backed by SQLite. Execution backends attach to that state as needed.

Current backends include:

- full Linux container,
- isolate shell via Dynamic Worker,
- isolate JavaScript via Dynamic Worker.

```text
authoritative durable filesystem
             │
     ┌───────┼────────┐
     ▼       ▼        ▼
 isolate   isolate   Linux
 shell       JS      container
```

The container path projects the filesystem through FUSE while `computerd` synchronizes state over RPC.

## Why it matters

This is a useful candidate abstraction for cloud-hosted agent workspaces where agents are idle much of the time and should not require permanently active Linux environments.

The architectural bet is:

```text
durable logical state + replaceable execution
```

instead of:

```text
one logical agent = one permanently running machine
```

## Relationship to Cloudflare OS

Cloudflare Computer is not currently the execution layer underneath Cloudflare OS, but both explore the same broader Workers-native direction:

- isolate-first execution,
- persistent state independent of compute lifetime,
- escalation to heavier runtimes only when required.

Cloudflare OS is the product/security experiment; Computer is a cleaner lower-level computer abstraction experiment.

## Current status

Preview / experimental; the upstream project explicitly says it is not production-ready.

## Experiments to run

- run the tutorial end-to-end;
- compare worker shell and container execution on the same task;
- run real git/npm/build workloads;
- destroy the container and verify filesystem persistence;
- inspect network / egress enforcement;
- measure backend activation latency;
- evaluate cost versus one persistent VM per agent;
- explore whether GPU execution could become another backend.
