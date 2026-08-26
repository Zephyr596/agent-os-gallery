# Agent as a System Service

## Core question

What changes when the agent is not merely an application inside the OS, but a first-class system participant with structured access to host state?

## Reference implementation

[Claw OS](../projects/claw-os/README.md) runs its agent layer as a privileged daemon (`clawd`) with scoped access to processes, logs, networking, applications, and other system resources.

```text
traditional model
OS → applications → assistant inside one app

agent-native system model
OS
├── applications
├── system services
└── agent service
      ├── process view
      ├── logs
      ├── network
      ├── app capabilities
      └── model layer
```

## Why it matters

An app-level assistant can only reason over the context an application chooses to expose. A system-level agent can answer questions about the machine itself and coordinate actions across applications.

Examples include:

- diagnosing network failures,
- inspecting resource usage,
- understanding crashes,
- managing development environments,
- coordinating multiple applications,
- using local GPU resources.

## Privilege is not the abstraction

The interesting idea is not simply "run the agent as root".

A useful Agent OS should separate:

```text
visibility
     from
mutation authority
     from
high-risk system authority
```

System-level access should therefore be mediated through structured primitives, capability checks, policy, and approvals rather than arbitrary unrestricted shell access alone.

## Physical-machine relevance

This model is especially relevant for a dedicated physical Agent Computer where hardware itself matters:

- GPU / CUDA,
- local disks,
- USB or device access,
- local model runtimes,
- build caches,
- persistent repositories,
- network topology.

A virtual cloud sandbox cannot fully substitute for those resources when the physical machine is intentionally part of the system.

## Design questions

1. Which host observations are safe by default?
2. Which mutations require explicit authority?
3. Can the agent inspect credentials without possessing them?
4. How is persistent memory separated from raw application history?
5. Can applications expose structured capabilities instead of relying on UI automation?
6. How does the system recover if the agent daemon misbehaves?
7. What is the emergency human override?
8. How is unattended reboot handled?
9. Can the same model work in WSL, bare Linux, containers, and cloud VMs?

## Working conclusion

A physical Agent OS may need to add a new system actor alongside users, services, and applications:

> an agent that is accountable to a human, has broad system visibility, but receives mutation authority through explicit capabilities rather than unrestricted ambient privilege.
