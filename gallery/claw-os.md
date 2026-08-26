# Claw OS

Repo: https://github.com/xiaoyu-work/claw-os

## Layer

Physical-machine agent system layer.

## Core idea

Run the agent as a privileged system daemon (`clawd`) with scoped access to processes, logs, networking, installed applications, and other host resources.

## Why it is different

Instead of giving the agent an isolated virtual computer, Claw OS asks what a traditional Linux machine should expose when the agent is a first-class system participant.

Important ideas include:

- system-level agent daemon
- persistent cross-app memory
- local-first model layer with cloud fallback
- app capability SDK
- capability checks and approval gates
- structured system primitives rather than UI scraping

## Deployment forms

- agent layer on Ubuntu
- WSL2 distribution
- Docker / container
- desktop / VM image
- cloud image paths

## Relevance to the physical-agent track

This is currently the most direct candidate for experimenting with a spare GPU laptop without first building a custom agent daemon.

A low-risk first experiment is Windows + WSL2 + Claw OS before repartitioning the machine or installing a dedicated Linux host.

## Questions to test

- GPU/CUDA visibility through WSL2 and bare Linux
- how credentials are scoped
- unattended boot and recovery
- real GitHub coding loop
- whether the capability model is usable under high-frequency autonomous work
- local model routing and fallback behavior
