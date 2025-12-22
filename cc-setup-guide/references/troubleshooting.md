# Troubleshooting Reference

## Table of Contents
1. [Windows Errors](#windows-errors)
2. [macOS Errors](#macos-errors)
3. [Linux/WSL Errors](#linuxwsl-errors)
4. [Authentication Issues](#authentication-issues)
5. [Network/Proxy Issues](#networkproxy-issues)
6. [Multiple Installations](#multiple-installations)

---

## Windows Errors

### 'git' is not recognized as an internal or external command
**Cause**: Git for Windows not installed.
**Fix**: Install from https://git-scm.com/downloads/win, then reopen terminal.

### 'curl' is not recognized
**Cause**: Old Windows version or curl not available in CMD.
**Fix**: Use PowerShell instead:
```powershell
irm https://claude.ai/install.ps1 | iex
```

### "running scripts is disabled on this system" (PowerShell)
**Cause**: ExecutionPolicy restricts script execution.
**Fix options**:
1. Use CMD method instead
2. Run as Administrator: `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`
3. Contact IT if on managed computer

### 'claude' is not recognized after install
**Cause**: Terminal didn't reload PATH.
**Fix**: Close terminal completely (not just the tab) and reopen. Then:
```cmd
where claude
```
If still not found, the install may have failed—check for errors during installation.

### "bash.exe not found" or "Git Bash not detected"
**Cause**: Claude Code can't find Git Bash.
**Fix**: 
1. Ensure Git for Windows is installed
2. If using portable Git, set the path:
```powershell
$env:CLAUDE_CODE_GIT_BASH_PATH="C:\Program Files\Git\bin\bash.exe"
```

### Access denied / SmartScreen blocked
**Cause**: Windows security or antivirus blocking.
**Fix options**:
1. Run terminal as Administrator
2. Temporarily disable antivirus
3. Whitelist the installation directory
4. Contact IT for managed computers

---

## macOS Errors

### command not found: brew
**Cause**: Homebrew not installed.
**Fix**: Use curl method instead:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```
Or install Homebrew first: https://brew.sh

### Error: Cask 'claude-code' is unavailable
**Cause**: Outdated Homebrew cache.
**Fix**:
```bash
brew update
brew install --cask claude-code
```

### command not found: claude after install
**Cause**: PATH not updated.
**Fix**:
1. Close Terminal completely and reopen
2. If still failing, add to PATH manually:
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Permission denied / Operation not permitted
**Cause**: macOS permissions or managed device restrictions.
**Fix**: Check System Preferences → Security & Privacy. For managed Macs, contact IT.

---

## Linux/WSL Errors

### OS/platform detection issues in WSL
**Cause**: WSL using Windows npm.
**Fix**:
```bash
npm config set os linux
npm install -g @anthropic-ai/claude-code --force --no-os-check
```

### Node not found in WSL
**Cause**: WSL using Windows Node.js.
**Fix**: Check paths:
```bash
which npm
which node
```
Should show `/usr/` paths, not `/mnt/c/`. Install Node via nvm or apt.

### nvm version conflicts (WSL)
**Cause**: Windows PATH imported into WSL.
**Fix**: Add to `~/.bashrc` or `~/.zshrc`:
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
Then: `source ~/.bashrc`

### Permission errors on npm install
**Cause**: npm global prefix not user-writable.
**Fix**: Use native installer instead of npm:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

---

## Authentication Issues

### Login fails / browser doesn't open
**Fix sequence**:
1. Run `/logout` in Claude Code
2. Close Claude Code
3. Run `claude` again

### Login loop persists
**Fix**: Remove auth file:
```bash
# macOS/Linux/WSL
rm -rf ~/.config/claude-code/auth.json
claude

# Windows
del %USERPROFILE%\.config\claude-code\auth.json
```

### Which login option to choose?
- **Claude.ai subscription (Pro/Max)**: Personal use, unified subscription
- **Claude Console**: API access, pay-per-use, teams/enterprise

---

## Network/Proxy Issues

### SSL/certificate errors
**Cause**: VPN, proxy, or corporate firewall.
**Fix options**:
1. Disconnect VPN temporarily
2. Try mobile hotspot
3. Ask IT about proxy settings

### Download returns HTML / "Access denied"
**Cause**: Captive portal or corporate proxy intercepting.
**Fix**: Try different network (mobile hotspot).

### Timeout errors
**Cause**: Firewall blocking Anthropic URLs.
**Fix**: Whitelist:
- `claude.ai`
- `api.anthropic.com`
- `storage.googleapis.com` (for binary downloads)

---

## Multiple Installations

### Unexpected version or path
**Check which claude is running**:
```bash
# Windows
where claude

# macOS/Linux
which claude
```

### Remove conflicting npm installation
```bash
npm uninstall -g @anthropic-ai/claude-code
```

### Remove native installation
**Windows (PowerShell)**:
```powershell
Remove-Item -Path "$env:LOCALAPPDATA\Programs\claude-code" -Recurse -Force
Remove-Item -Path "$env:LOCALAPPDATA\Microsoft\WindowsApps\claude.exe" -Force
```

**macOS (Homebrew)**:
```bash
brew uninstall --cask claude-code
```

**macOS/Linux (native/curl)**:
```bash
rm -f ~/.local/bin/claude
rm -rf ~/.claude-code
```

### Run doctor after cleanup
```bash
claude doctor
```
