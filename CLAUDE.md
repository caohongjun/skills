# CLAUDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.
## Overview
This is a personal Claude Code skills repository. Each skill is a self-contained directory that can be installed to `~/.claude/skills/` to extend Claude Code's capabilities.
## Repository Structure
```
hojun-skills/
├── skills/
│   ├── hojun-book/           # Book walkthrough + deep-dive analysis
│   │   └── SKILL.md
│   └── hojun-app-teardown/   # Mobile app competitive teardown
│       └── SKILL.md
├── .claude-plugin/
│   └── plugin.json           # Plugin metadata (required by skills CLI)
├── CLAUDE.md
└── README.md
```
## Skill Format
Each `SKILL.md` follows this structure:
```yaml
---
name: skill-name
description: "What this skill does. Use when user says..."
user_invocable: true|false
---
# Skill content in markdown...
```
## Skill Inventory
| Skill | Purpose | External Dependencies |
|-------|---------|----------------------|
| `hojun-book` | Book walkthrough + deep-dive analysis (outputs Markdown) | None |
| `hojun-app-teardown` | Mobile app competitive teardown (outputs Markdown) | None |
## Commands
### Install Skills (for users)
```bash
# Install all skills via skills CLI (recommended)
npx skills add caohongjun/skills -g --all
# Or install individual skills
npx skills add caohongjun/skills -g --skill hojun-book
```
## Architecture Notes
### Skill Invocation
- Skills with `user_invocable: true` can be triggered via natural language
- Trigger phrases are defined in each skill's `description` field
### Shared Conventions
**Markdown output** (hojun-book, hojun-app-teardown):
- Filenames: `{timestamp}--{title}__{type}.md`
- Output directory: `~/Documents/notes/`
- Timestamps: `date +%Y%m%dT%H%M%S`
## Development Guidelines
- Skills are atomic units — each skill directory is self-contained
- The `plugin.json` file with `"skills": "./skills"` field is required for the CLI to discover skills
## Testing Changes
After modifying a skill:
1. Reinstall via `npx skills add caohongjun/skills -g --all`
2. Check `ls -la ~/.claude/skills/ | grep hojun` to verify symlinks are created
3. Test via natural language trigger in Claude Code
