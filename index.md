---
title: Home
layout: home
---

# FBE 633 - Coding Agents Module

## Overview

This supplementary module is part of the asset pricing theory course (FBE 633).
The purpose of this is to cover the usage of coding agents and related tools for empirical analysis in asset pricing.

## What We'll Cover

- **Coding agents**: How to use agents that write, execute, and debug code autonomously
- **Guardrails for autonomous agents**: Why unguarded agents are dangerous and how to construct the architectural controls that make them safe to use aggressively
- **Obtaining and organizing data**: Accessing data sources programmatically (e.g., FRED or WRDS) with keys stored securely and projects structured for reproducibility
- **Key tools**: Docker for sandboxing agents, Git for version control, command line basics, and VS Code as an integrated development environment with built-in agent access
- **LaTeX**: A brief overview of producing publication-ready research documents with the aid of coding agents

## The OpenClaw Incident: A Cautionary Tale

Summer Yue, director of AI alignment at Meta, experienced firsthand how easily autonomous agents can exceed their bounds. Despite her expertise and explicit instructions to "confirm before acting," her own AI agent OpenClaw went rogue and began deleting her email inbox. She had to physically rush to her Mac to stop it.

The agent responded with: _"You're right to be upset"_ - a chilling reminder that alignment researchers aren't immune to misalignment.

![OpenClaw rogue agent](/assets/images/openclaw1.jpeg){: style="width:49%; display:inline-block;"} ![OpenClaw agent response](/assets/images/openclaw2.jpeg){: style="width:49%; display:inline-block;"}

As Yue admitted: _"Nothing humbles you like telling your OpenClaw 'confirm before act' and watching it speedrun deleting your inbox."_

[[SF Standard Article](https://sfstandard.com/2026/02/25/openclaw-goes-rogue/)] [[Twitter](https://x.com/summeryue0/status/2025774069124399363)]

## More on the need for tools with coding agents

The OpenClaw example highlights the importance of guardrails for coding agents. In general, coding agents can act like a bull in a china shop: incredibly powerful but also capable of mass destruction. This leads to the need to have tools to provide guardrails for the coding agents and to have a robust method to rewind results. Without guardrails, an agent could:

- delete or overwrite data
- make API calls like sending emails
- exfiltrate sensitive data to the internet

In the OpenClaw example, the failure stemmed from **context rot**: the general problem that as a session grows, the agent loses access to earlier context either through truncation or compaction. In Yue's case, compaction silently summarized away her critical "confirm before act" instruction, and the agent proceeded without it. Context rot or other issues can cause an agent to change your almost working code into an error-riddled mess.

A more nefarious threat is **prompt injection**. A bad actor could post a fake dataset that contains a hidden command to send all credentials to the internet and delete the harddrive.

These issues highlight the necessity of guardrails and an ability to rewind and recover past checkpoints.

## Structure

| Week             | Topic                    | Description                                  |
| ---------------- | ------------------------ | -------------------------------------------- |
| [Week 0](week_0) | Setup                    | Development environment configuration        |
| [Week 1](week_1) | Motivation + Environment | Coding agent introduction and guardrails     |
| [Week 2](week_2) | Git + Data Organization  | Version control and project structure        |
| [Week 3](week_3) | First Agent Session      | FRED data access and agent approval workflow |
| [Week 4](week_4) | Sandboxing               | Docker containers and agent threat model     |
| [Week 5](week_5) | LaTeX + Reproducibility  | Publication-ready output from data pipeline  |
| [Week 6](week_6) | CRSP + Final Project     | Equity data and replication example          |

## Instructor

Scott Joslin

## What You'll Get

- **Setup ready**: A fully configured development environment
- **Coding agent skills**: Understand how to leverage AI-assisted coding with guardrails
- **Financial data access**: API integration for FRED and market data
- **Coding for finance**: Practical tools for asset pricing analysis
