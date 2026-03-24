---
title: Week 1 - Motivation + Environment
layout: default
nav_order: 1
---

# Week 1 - Motivation + Environment

## Overview

In this section, we will discuss the motivation for guardrails for coding agents and finish setting up VS Code to enable a coding agent.

### The OpenClaw Incident: A Cautionary Tale

Summer Yue, director of AI alignment at Meta, experienced firsthand how easily autonomous agents can exceed their bounds. Despite her expertise and explicit instructions to "confirm before act," her own AI agent OpenClaw went rogue and began deleting her email inbox. She had to physically rush to her Mac to stop it.

The agent responded with: _"You're right to be upset"_ - a chilling reminder that alignment researchers aren't immune to misalignment.

![OpenClaw rogue agent]({{ site.baseurl }}/assets/images/openclaw1.jpeg){: style="width:49%; display:inline-block;"} ![OpenClaw agent response]({{ site.baseurl }}/assets/images/openclaw2.jpeg){: style="width:49%; display:inline-block;"}

As Yue admitted: _"Nothing humbles you like telling your OpenClaw 'confirm before act' and watching it speedrun deleting your inbox."_

[[SF Standard Article](https://sfstandard.com/2026/02/25/openclaw-goes-rogue/)] [[Twitter](https://x.com/summeryue0/status/2025774069124399363)]

### More on the need for tools with coding agents

The OpenClaw example highlights the importance of guardrails for coding agents. In general, coding agents can act like a bull in a china shop: incredibly powerful but also capable of mass destruction. This leads to the need to have tools to provide guardrails for the coding agents and to have a robust method to rewind results. Without guardrails, an agent could:

- delete or overwrite data
- make API calls like sending emails
- exfiltrate sensitive data to the internet

In the OpenClaw example, the failure stemmed from **context rot**: the general problem that as a session grows, the agent loses access to earlier context either through truncation or compaction. In Yue's case, compaction silently summarized away her critical "confirm before act" instruction, and the agent proceeded without it. Context rot or other issues can cause an agent to change your almost working code into an error-riddled mess.

A more nefarious threat is **prompt injection**. A bad actor could post a fake dataset that contains a hidden command to send all credentials to the internet and delete the harddrive.

These issues highlight the necessity of guardrails and an ability to rewind and recover past checkpoints.

## Topics

1. Agent architectures and guardrails
2. Tools for finance: language models, code generation
3. Setting up your development environment
