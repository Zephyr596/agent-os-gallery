# Cloudflare Computer

Repo: https://github.com/cloudflare/computer

## Layer

Virtual agent computer / sandbox abstraction.

## Core idea

The logical computer is not necessarily one persistent VM. Cloudflare Computer keeps authoritative filesystem state in a Durable Object backed by SQLite and attaches execution backends as needed.

Current backends include:

- full Linux container
- isolate shell via Dynamic Worker
- isolate JavaScript via Dynamic Worker

## Core architectural bet

```text
durable logical state
      +
replaceable execution
```

instead of:

```text
one agent = one permanently running VM/container
```

The container can project the authoritative filesystem through FUSE while `computerd` synchronizes state back over RPC.

## Why it matters

This is a useful candidate abstraction for cloud-hosted agent workspaces where most agent time is idle and expensive full Linux execution should only be used when necessary.

## Current status

Preview / experimental; not production-ready.

## Questions to test

- latency of switching execution backends
- behavior with real git/npm/build workloads
- persistence semantics across container loss
- networking / egress enforcement
- whether GPU execution can eventually become another backend
- cost versus one persistent VM per agent
