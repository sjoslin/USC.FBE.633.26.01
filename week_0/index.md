---
title: Week 0 - Setup
layout: default
nav_order: 0
---

# Week 0 - Course Setup

## Overview

This week covers all the necessary setup for the course.

## Windows Users

1. Install Windows Terminal from Microsoft Store
2. Install VS Code
3. Open Windows Terminal and run:
   ```bash
   wsl --install
   ```
4. Restart your computer
5. After restart, set up your Ubuntu username and password
6. Open terminal and verify you can open an Ubuntu tab

Reference: [WSL Installation Guide](https://learn.microsoft.com/en-us/windows/wsl/install)

## Mac Users

1. Install Homebrew (if not already installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Install VS Code:
   ```bash
   brew install --cask visual-studio-code
   ```

## All Students

1. [Setup FRED API Key](#fred-api-key)
2. [Request API Access](#api-access)

---

## FRED API Key

To access economic data from the Federal Reserve Economic Data (FRED) platform:

1. Go to [FRED Account Registration](https://fredaccount.stlouisfed.org/)
2. Create an account
3. Once logged in, navigate to API Keys
4. Generate a new API key
5. Store your key securely (do not commit to version control)

## API Access

If you do not have a subscription to Claude or OpenAI ChatGPT:

USC students can request API access through the [USC AI Access Initiative](https://ai-access.usc.edu/).

For Claude API access:
- Check if your USC email provides free access through the [Anthropic AI Education Program](https://www.anthropic.com/education)

For OpenAI API access:
- Apply for the [OpenAI APIGrant Program](https://openai.com/index/api-grant-program/)

---

## Verification Checklist

- [ ] Windows: WSL installed and Ubuntu terminal working
- [ ] Mac: Homebrew and VS Code installed
- [ ] FRED API key obtained
- [ ] AI API access arranged
