---
name: cc-setup-guide
description: Interactive guide for installing Claude Code on Windows, macOS, and Linux. Use when users ask for help installing Claude Code, troubleshooting installation errors, or need step-by-step guidance through the native installation process. Triggers include "install Claude Code", "claude code setup", "can't install claude", installation errors like 'command not found', 'git not recognized', or permission issues.
---

# Claude Code Installation Guide

Help users install Claude Code using the native installation method (recommended by Anthropic).

## Workflow

### Step 1: Identify Operating System and Shell

Ask the user:
1. **Operating system**: Windows, macOS, or Linux/WSL?
2. **For Windows only**: Which shell are you using? (PowerShell, Command Prompt/CMD, Git Bash, or WSL)

If they don't know their shell, ask what app they opened and what text appears in the title bar.

### Step 2: Check Prerequisites

**macOS/Linux**: No prerequisites needed. Proceed to installation.

**Windows prerequisites**:
- Git for Windows is recommended (provides Git Bash)
- Run these checks if using CMD:
  ```cmd
  git --version
  curl --version
  ```
- If `git` not recognized → Install [Git for Windows](https://git-scm.com/downloads/win)
- If `curl` not recognized → Use PowerShell method instead

**Work/managed computers**: Warn that IT policies may block installations. Suggest trying from personal network or contacting IT.

### Step 3: Run Installation Command

Provide the appropriate command based on OS/shell:

| Platform | Command |
|----------|---------|
| macOS (Homebrew) | `brew install --cask claude-code` |
| macOS/Linux/WSL | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Windows PowerShell | `irm https://claude.ai/install.ps1 \| iex` |
| Windows CMD | `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd` |

**Important**: After running, user MUST close and reopen their terminal for PATH changes.

### Step 4: Verify Installation

Have user run:
```bash
claude --version
claude doctor
```

If `claude` not recognized after reopening terminal, check:
- **Windows**: `where claude`
- **macOS/Linux**: `which claude`

### Step 5: Login

Run `claude` to start interactive session. User chooses authentication:
- **Claude.ai** (Pro/Max subscription) - recommended for regular users
- **Claude Console** (API with pre-paid credits) - for developers/teams

## Troubleshooting Reference

For detailed error solutions, see [references/troubleshooting.md](references/troubleshooting.md).

### Quick Fixes

| Error | Solution |
|-------|----------|
| `'git' is not recognized` (Windows) | Install Git for Windows |
| `'curl' is not recognized` (Windows) | Use PowerShell: `irm https://claude.ai/install.ps1 \| iex` |
| `running scripts is disabled` (PowerShell) | Use CMD method or adjust ExecutionPolicy |
| `command not found: claude` after install | Close terminal completely and reopen |
| `bash.exe not found` (Windows) | Install Git for Windows or set `$env:CLAUDE_CODE_GIT_BASH_PATH` |
| Login loop / auth errors | Run `/logout`, delete `~/.config/claude-code/auth.json`, restart |

## Cleanup Commands

If user needs fresh start, see [references/cleanup.md](references/cleanup.md).

## Official Documentation

- Setup: https://code.claude.com/docs/en/setup
- Troubleshooting: https://code.claude.com/docs/en/troubleshooting
- Quickstart: https://code.claude.com/docs/en/quickstart
