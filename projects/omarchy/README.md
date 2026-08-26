# Omarchy

Upstream: https://github.com/basecamp/omarchy

## Layer

Human-centric Linux host OS / developer workstation distribution.

## Most valuable abstraction

**[Human-centric host as agent substrate](../../abstractions/human-centric-host.md)**

Omarchy is useful in this gallery not because it is an Agent OS runtime, but because it represents one end of the physical-host design spectrum: a developer desktop that is pleasant for a human and also capable of running coding agents locally.

## Core idea

Omarchy is an opinionated Arch + Hyprland developer desktop with integrated development, shell, browser, container, and AI tooling.

For Agent OS research, the relevant question is:

> Can a human-first workstation also serve reliably as a long-lived machine delegated to agents?

## Why it matters

A physical Agent Computer may still need to serve a human occasionally.

Omarchy therefore contrasts with a minimal Ubuntu/server host:

```text
Omarchy / desktop-first
      vs
minimal Linux / appliance-first
```

## Relevant characteristics

- opinionated desktop workflow,
- integrated development tools,
- coding-agent launch experience,
- Docker and shell tooling,
- dual-boot guidance,
- system snapshots,
- desktop-first ergonomics,
- native GPU access.

## Tradeoffs for unattended hosting

Potential negatives compared with a minimal server-oriented host include:

- desktop/session assumptions,
- encrypted-disk boot may require human presence,
- sleep/lock behavior needs explicit hardening,
- more moving parts for a 24/7 appliance,
- some of the distribution's value disappears if nobody sits at the machine.

## Questions to test

- reliability as a 24/7 unattended host;
- behavior after reboot / power loss;
- agent CLI usability over SSH/Tailscale;
- GPU workloads without an active local session;
- unattended disk unlock and recovery strategy;
- whether desktop convenience justifies the additional operational complexity.
