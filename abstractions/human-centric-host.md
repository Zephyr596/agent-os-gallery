# Human-Centric Host as Agent Substrate

## Core question

Should a physical Agent Computer be optimized as an unattended server, or remain a developer workstation that a human can comfortably sit down and use?

## Reference implementation

[Omarchy](../projects/omarchy/README.md) is useful here as a host-environment reference rather than as an Agent OS runtime.

It represents the developer-workstation end of the spectrum:

```text
human-centric desktop
      │
      ├── terminal / editor / browser
      ├── coding agents
      ├── desktop session
      └── local GPU / devices
```

The opposite end is closer to:

```text
minimal unattended Linux
      │
      ├── systemd services
      ├── SSH / Tailscale
      ├── containers
      └── no required local desktop session
```

## Why it matters

A spare laptop or workstation may serve two very different roles:

1. a 24/7 appliance primarily operated by agents and remote humans;
2. a shared machine that sometimes becomes a normal local developer workstation.

Those roles imply different choices for encryption, login, sleep, desktop services, updates, recovery, and resource usage.

## Design tradeoff

Human convenience often introduces lifecycle assumptions that hurt unattended operation:

- graphical login sessions,
- screen locking,
- sleep/hibernate defaults,
- full-disk encryption requiring local boot input,
- desktop-specific network behavior,
- user-session-bound daemons.

Conversely, a minimal server removes many of those problems but loses local ergonomics.

## Design questions

1. Must the machine recover from power loss without a person present?
2. Does disk encryption block unattended reboot?
3. Are agent services bound to a graphical user session?
4. Can GPU workloads run without a logged-in desktop session?
5. How are updates and rollback handled?
6. Is local GUI use actually part of the target workflow?
7. How much disk space is consumed by maintaining dual-boot or desktop environments?
8. Can the host remain secure while exposing enough authority to a long-running agent?

## Working conclusion

A beautiful developer desktop and a reliable Agent appliance optimize for different things.

A physical Agent OS experiment should decide explicitly whether the human is a **local primary user** or a **remote operator** before choosing the host distribution.
