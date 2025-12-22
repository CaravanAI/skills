# Cleanup Commands

Use these commands to completely remove Claude Code for a fresh installation.

## Remove Installation

### Windows (PowerShell)
```powershell
# Remove native installation
Remove-Item -Path "$env:LOCALAPPDATA\Programs\claude-code" -Recurse -Force
Remove-Item -Path "$env:LOCALAPPDATA\Microsoft\WindowsApps\claude.exe" -Force
```

### Windows (CMD)
```cmd
rmdir /s /q "%LOCALAPPDATA%\Programs\claude-code"
del "%LOCALAPPDATA%\Microsoft\WindowsApps\claude.exe"
```

### macOS (Homebrew)
```bash
brew uninstall --cask claude-code
```

### macOS/Linux (native/curl install)
```bash
rm -f ~/.local/bin/claude
rm -rf ~/.claude-code
```

### npm installation (any OS)
```bash
npm uninstall -g @anthropic-ai/claude-code
```

---

## Remove Configuration

⚠️ **Warning**: This deletes all settings, allowed tools, MCP configs, and session history.

### Windows (PowerShell)
```powershell
Remove-Item -Path "$env:USERPROFILE\.claude" -Recurse -Force
Remove-Item -Path "$env:USERPROFILE\.claude.json" -Force
```

### Windows (CMD)
```cmd
rmdir /s /q "%USERPROFILE%\.claude"
del "%USERPROFILE%\.claude.json"
```

### macOS/Linux
```bash
rm -rf ~/.claude
rm -f ~/.claude.json
```

---

## Remove Project-Specific Settings

Run from within the project directory:

### Any OS
```bash
rm -rf .claude
rm -f .mcp.json
```

---

## Complete Fresh Start

Run all applicable removal commands, then reinstall using the appropriate installation command.
