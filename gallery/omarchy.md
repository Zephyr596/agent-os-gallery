# Omarchy

Repo: https://github.com/basecamp/omarchy

## Layer

Human-centric Linux host OS / developer workstation distribution.

## Core idea

Omarchy is an opinionated Arch + Hyprland developer desktop with integrated development and AI tooling.

It is useful in this gallery mostly as a **host environment candidate**, not as an Agent OS runtime.

## Why it matters

A physical agent machine may still need to serve a human occasionally. Omarchy represents the "developer workstation first" end of the spectrum, compared with a minimal/server-oriented Linux host.

## Relevant characteristics

- opinionated desktop workflow
- integrated development tools
- AI/coding-agent launch experience
- Docker and shell tooling
- dual-boot guidance
- system snapshots
- desktop-first ergonomics

## Tradeoffs for unattended agent hosting

Potential negatives compared with a minimal Linux server:

- desktop/session assumptions
- encrypted-disk boot may require human presence
- more moving parts for a machine expected to run unattended
- GPU is native, but server lifecycle still needs explicit hardening

## Questions to test

- reliability as a 24/7 unattended host
- behavior after reboot / power loss
- agent CLI usability over SSH
- how much of its value disappears once no human sits at the machine
