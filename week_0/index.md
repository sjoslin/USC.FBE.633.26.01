---
title: Week 0 - Setup
layout: default
nav_order: 0
---

# Week 0 - Course Setup

## Overview

This week covers all the necessary setup for the course.

---

## Setup Checklist

You will need to complete the following steps to be ready for Week 1:

1. Complete the operating system specific installs below for [Windows](#windows) and [macOS](#macos)
2. Request API keys and register for WRDS

---

## Windows

We will need to install VS Code, an editor and IDE (integrated development environment), to use the coding agents. We will also install **Windows Subsystem for Linux 2** (WSL2). This installs a virtual machine with a Linux operating system that sits alongside the Windows operating system. While you can use coding agents without WSL2, we will see that it greatly enhances our guardrails. An alternative to WSL2 is to use Git Bash. However, with Git Bash, Claude Code's sandbox currently is not supported.

1. Install [Windows Terminal](https://aka.ms/terminal) from Microsoft Store
2. Install [VS Code](https://code.visualstudio.com/download)
3. Open Windows Terminal and run:
   ```bash
   wsl --install
   ```
4. Restart your computer
5. After restart, set up your Ubuntu username and password
6. Open terminal and verify you can open an Ubuntu tab

Reference: [WSL Installation Guide](https://learn.microsoft.com/en-us/windows/wsl/install)

---

## macOS

We will install VS Code, an editor and IDE (integrated development environment), to use the coding agents. We will also install Homebrew, which is a package manager for macOS (similar to pip for Python, for example). We will use this for VS Code now and later for other packages.

1. Install Homebrew (if not already installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Install [VS Code](https://code.visualstudio.com/download):
   ```bash
   brew install --cask visual-studio-code
   ```

---

## API Keys

There are several popular choices for coding agents. The most popular are Anthropic's Claude Code and OpenAI's Codex. Both of these require a pro subscription. However, USC has a license with OpenAI that should allow access to Codex. Another choice is to use local inference with models such as Deepseek or Qwen along with a model runner, such as ollama, with a coding agent interface like OpenCode.

### FRED API Key

To access economic data from the Federal Reserve Economic Data (FRED) platform:

1. Go to [FRED Account Registration](https://fredaccount.stlouisfed.org/)
2. Create an account
3. Once logged in, navigate to API Keys
4. Generate a new API key
5. Store your key securely (do not commit to version control)

### AI API Access

USC students can request access to OpenAI Codex through [ITS](https://itservices.usc.edu/ai/ai-faqs/).

### WRDS Account

To access the Wharton Research Data Services (WRDS) platform:

1. Go to [WRDS Registration](https://wrds-www.wharton.upenn.edu/register/)
2. Select "PhD" as your account type
3. Select "USC" from the institution dropdown
4. Complete the registration form

If you have any questions, please contact Eduardo Tinoco at [etinoco@usc.edu](mailto:etinoco@usc.edu).

---

## Verification Checklist

- [ ] Windows: WSL installed and Ubuntu terminal working
- [ ] macOS: Homebrew and VS Code installed
- [ ] FRED API key obtained
- [ ] AI API access arranged
- [ ] WRDS account registered


