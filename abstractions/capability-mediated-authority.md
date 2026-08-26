# Capability-Mediated Authority

## Core question

How should an agent gain access to external systems without inheriting the full ambient authority of the human user or host process?

This is one of the most important security questions in Agent OS design.

## The common failure mode: ambient authority

A typical agent setup looks like this:

```text
user account
   │
   ├── GitHub token
   ├── Google credentials
   ├── Slack credentials
   ├── cloud credentials
   └── MCP servers
          │
          ▼
        agent
```

Once the tools are configured, the agent can often see a broad capability surface in every session. The effective permission model becomes:

```text
"the agent can do whatever the configured account can do"
```

This is convenient, but weak as a security boundary.

The risk increases with autonomous agents because:

- prompts are untrusted input,
- generated code is untrusted software,
- tools may produce adversarial content,
- long-running trajectories amplify mistakes,
- a human may not observe every intermediate action.

## Cloudflare OS's answer: explicit introduction

Cloudflare OS provides a useful reference design through **Gatekeepers**.

The key shift is:

```text
identity owns broad authority
        │
        ▼
user explicitly introduces a resource
        │
        ▼
Gatekeeper constructs narrow capability
        │
        ▼
agent / Gadget receives only that capability
```

For example, instead of giving an agent general GitHub access, a user may introduce one repository. The agent receives an object representing operations over that resource rather than the user's raw credential.

This is conceptually closer to capability-based security than traditional account-wide ACL inheritance.

## Gatekeeper as a security adapter

A Gatekeeper sits between an agent/application and an external system:

```text
Agent / Gadget
     │
     │ narrow RPC capability
     ▼
 Gatekeeper
     │
     ├── authentication / OAuth
     ├── resource scoping
     ├── API normalization
     ├── audit logging
     ├── side-effect policy
     └── approval handling
     │
     ▼
External service
```

The important abstraction is not Cloudflare Workers itself. It is the separation between:

- **credential possession**
- **resource authority**
- **capability delegation**

The agent should receive the third, not the first.

## Default deny

The strong version of this model starts from:

```text
Agent has access to nothing.
```

Configured integrations do not automatically become ambient agent tools.

Authority is introduced as needed for a task.

This matters because installing an integration and delegating authority are two different actions.

## Resource-scoped capabilities

A useful capability should answer:

- Which resource does it refer to?
- Which operations are allowed?
- Which principal introduced it?
- Is it transferable to another agent or app?
- Can it expire?
- Can it be revoked?
- What actions has it performed?

This suggests that an Agent OS should treat capabilities as first-class objects rather than just configuration entries.

## Human approval without blocking the trajectory

Cloudflare OS also explores an unusual approval model.

The conventional pattern is synchronous approval:

```text
agent wants side effect
       │
       ▼
     STOP
       │
wait for human
       │
       ▼
approve / reject
       │
       ▼
agent continues
```

This is safe but operationally poor for asynchronous agents. The human returns later and discovers the agent stopped five minutes after the task began.

Gatekeepers propose a different model:

```text
agent proposes side effect
        │
        ▼
Gatekeeper records pending action
        │
        ├── simulates result for the agent
        │
        ▼
agent continues reasoning
        │
        ▼
more pending actions accumulate
        │
        ▼
human reviews later
```

This separates **planning progress** from **commit authority**.

The abstraction resembles transactions or speculative execution:

```text
plan / simulate  !=  commit
```

This may be particularly valuable for agents that operate asynchronously for minutes or hours.

## The hard problem: simulation correctness

The model raises an important systems question:

> If the real side effect has not happened, what exactly should later reads return?

Suppose an agent:

1. creates a GitHub issue,
2. reads the issue back,
3. adds a label to it,
4. references its issue number in another action.

If step 1 is pending approval, the Gatekeeper must maintain a coherent speculative world for steps 2–4.

This requires more than a confirmation dialog. It implies some combination of:

- virtual resource identifiers,
- shadow state,
- deterministic simulation,
- dependency graphs between proposed actions,
- reconciliation when only some actions are approved,
- rollback semantics when reality differs from simulation.

This is an important area for further investigation.

## Capability security applies to generated apps too

Cloudflare OS does not only restrict the conversational agent. **Gadgets** — applications written or modified by the agent — also begin without ambient network authority.

This creates a useful security composition:

```text
untrusted model output
      │
      ▼
generated application
      │
      ├── sandboxed execution
      └── no ambient internet authority
              │
              ▼
      explicitly introduced capability
```

This is stronger than trying to make AI-generated code trustworthy through review alone.

The system assumes generated code may be wrong or hostile and constrains what it can reach.

## Generalized Agent OS security model

The Cloudflare OS implementation suggests a broader architecture:

```text
                 Human principal
                       │
                       │ delegates
                       ▼
                Capability graph
              /        │        \
             /         │         \
        Agent A     Gadget B     Agent C
           │            │           │
           ▼            ▼           ▼
        GitHub       Google Doc    Scheduler
        repo X        document Y   job Z
```

Authority becomes a graph of explicit delegations rather than a flat bag of credentials.

This provides several useful properties:

- least authority,
- provenance,
- revocation,
- auditability,
- delegation boundaries,
- human accountability.

## Design questions for other systems

When evaluating an Agent OS or harness, ask:

1. Does connecting an integration automatically expose it to every agent?
2. Does the model ever receive raw credentials?
3. Is authority scoped by account or by resource?
4. Can two agents owned by the same human have different permissions?
5. Can generated code receive less authority than the agent that created it?
6. Can capabilities be delegated, revoked, and audited?
7. Is approval synchronous, asynchronous, or transactional?
8. What happens to dependent actions when an earlier side effect is rejected?
9. Can prompt injection expand the agent's authority, or only misuse authority already introduced?
10. Is the security boundary enforced below the model/harness layer?

## Working conclusion

A useful Agent OS should probably not model an agent as merely another logged-in user.

A better model is:

> An agent is an actor accountable to a human principal, operating through explicitly delegated capabilities whose authority is narrower than the principal's own credentials.

Cloudflare OS's Gatekeepers are currently one of the clearest implementations of this idea in the gallery.

## Reference project

- [Cloudflare OS](../projects/cloudflare-os/README.md)
- Upstream: https://github.com/cloudflare/cloudflare-os
