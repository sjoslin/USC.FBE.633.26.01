---
title: Course Overview and Motivation
layout: default
nav_order: -1
---

# Overview

This supplementary module is part of the asset pricing theory course (FBE 633).
The purpose of this is to cover the usage of coding agents and related tools for empirical analysis in asset pricing.

## What We'll Cover

- **Coding agents**: How to use agents that write, execute, and
  debug code autonomously
- **Guardrails for autonomous agents**: Why unguarded agents are
  dangerous and how to construct the architectural controls that
  make them safe to use aggressively
- **Obtaining and organizing data**: Accessing data sources
  programmatically (e.g., FRED or WRDS) with keys stored securely
  and projects structured for reproducibility
- **Key tools**: Docker for sandboxing agents, Git for version
  control, command line basics, and VS Code as an integrated
  development environment with built-in agent access
- **LaTeX**: A brief overview of producing publication-ready
  research documents with the aid of coding agents

## The OpenClaw Incident: A Cautionary Tale

Summer Yue, director of AI alignment at Meta, experienced firsthand how easily autonomous agents can exceed their bounds. Despite her expertise and explicit instructions to "confirm before acting," her own AI agent OpenClaw went rogue and began deleting her email inbox. She had to physically rush to her Mac to stop it.

The agent responded with: _"You're right to be upset"_ - a chilling reminder that alignment researchers aren't immune to misalignment.

![OpenClaw rogue agent](/assets/images/openclaw1.jpeg){: style="width:49%; display:inline-block;"} ![OpenClaw agent response](/assets/images/openclaw2.jpeg){: style="width:49%; display:inline-block;"}

As Yue admitted: _"Nothing humbles you like telling your OpenClaw 'confirm before acting' and watching it speedrun deleting your inbox."_

[[SF Standard Article](https://sfstandard.com/2026/02/25/openclaw-goes-rogue/)] [[Twitter](https://x.com/summeryue0/status/2025774069124399363)]

## More on the need for tools with coding agents

The OpenClaw example highlights the importance of guardrails for coding agents. In general, coding agents can act like a bull in a china shop: incredibly powerful but also capable of mass destruction. This leads to the need to have tools to provide guardrails for the coding agents and to have a robust method to rewind results. Without guardrails, an agent could:

- delete or overwrite data
- make API calls like sending emails
- exfiltrate sensitive data to the internet

In the OpenClaw example, the failure stemmed from **context rot**:  
the general problem that as a session grows, the agent loses access
to earlier context either through truncation or compaction. In Yue's
case, compaction silently summarized away her critical "confirm
before acting" instruction, and the agent proceeded without it.
Context rot or other issues can cause an agent to change your almost working code into an error-riddled mess.

A more nefarious threat is **prompt injection**. A bad actor could post a fake dataset that contains a hidden command to send all credentials to the internet and delete the harddrive.

These issues highlight the necessity of guardrails and an ability to rewind and recover past checkpoints.

## Structure

This course is divided into 6 weeks:

| Week | Topic                            |
| ---- | -------------------------------- |
| 0    | Setup and configuration          |
| 1    | Introduction to coding agents    |
| 2    | Financial data sources and APIs  |
| 3    | Financial analysis with Python   |
| 4    | Agent frameworks and tools       |
| 5    | Financial agent applications     |
| 6    | Advanced topics and project work |

## Goals

By the end of this course, you should be able to:

1. Set up a professional Python development environment
2. Understand how coding agents work and when to use them
3. Build agents that assist with financial data analysis
4. Apply these tools to understand asset pricing
