# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains publicly available Claude Code skills. Skills are self-contained packages that extend Claude Code's capabilities through guided workflows, reference documentation, and reusable scripts.

## Skill Structure

Each skill is a directory containing:

```
skill-name/
├── SKILL.md           # Main skill file with YAML frontmatter (name, description) and workflow
└── references/        # Supporting documentation loaded on-demand
```

### SKILL.md Format

Skills use YAML frontmatter for metadata:
```yaml
---
name: skill-name
description: Short description with trigger phrases for when to invoke
---
```

The body contains the workflow steps, quick reference tables, and links to supporting documentation in `references/`.

## Adding New Skills

1. Create a directory with the skill name (lowercase, hyphenated)
2. Add `SKILL.md` with frontmatter and workflow documentation
3. Add supporting docs to `references/` subdirectory if needed
4. Reference files use relative paths: `[references/filename.md](references/filename.md)`
