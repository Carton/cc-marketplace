# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **Claude Code plugin marketplace** - a monorepo containing example plugins demonstrating different plugin capabilities. Users install this marketplace to access all contained plugins.

## Architecture

### Two-Level Structure

The marketplace has a **registry** (`.claude-plugin/marketplace.json`) that references individual **plugins** (`plugins/*/`). Each plugin is self-contained with its own metadata.

```
.claude-plugin/marketplace.json   → Central registry (lists all plugins)
plugins/python-dev/               → Comprehensive Python dev plugin (skills + commands)
```

### Plugin Types & Key Differences

1. **Slash Commands** (user-invoked): Filename in `commands/*.md` becomes command name (e.g., `hello.md` → `/hello`)
2. **Agent Skills** (autonomous): Must be in `skills/skill-name/SKILL.md` format. Claude decides when to invoke.
3. **MCP Servers** (external tools): Configured in plugin's `plugin.json` under `"mcp"` key with command/args

### Required Plugin Files

Every plugin needs:
- `.claude-plugin/plugin.json` - Metadata (name, version, author, description)
- `README.md` - User documentation

## Adding a New Plugin

1. **Copy template**:
   ```bash
   cp -r plugins/python-dev plugins/my-plugin       # Use python-dev as template
   ```

2. **Update metadata** in `plugins/my-plugin/.claude-plugin/plugin.json`:
   - Change `name`, `description`, `version`
   - Update `author` (name and email)

3. **Register in `.claude-plugin/marketplace.json`**:
   - Add entry to `plugins` array
   - Set `source` to `"./plugins/my-plugin"`
   - Choose appropriate `category`: development, testing, documentation, productivity, devops

4. **Test locally** before committing:
   ```bash
   cp -r plugins/my-plugin ~/.claude/plugins/
   # Reload Claude Code or use /plugin list
   ```

## Validation Commands

```bash
# Validate marketplace.json structure
cat .claude-plugin/marketplace.json | jq .

# Validate all plugin.json files
find plugins -name "plugin.json" -exec jq . {} \;
```

## Critical Implementation Details

### Slash Command Naming
The `.md` filename **IS** the command name (without extension). Example:
- `commands/python-setup.md` → `/python-setup` command
- File contains markdown instructions that Claude follows when command is invoked

### Agent Skill Structure
Directory structure matters for skills:
```
skills/
└── skill-name/           ← Directory name is the skill name (hyphen-case)
    └── SKILL.md         ← Must be named exactly "SKILL.md"
```

**CRITICAL**: SKILL.md must start with YAML frontmatter:
```yaml
---
name: skill-name         # Must match directory name exactly
description: Complete description (1-3 sentences) explaining what, when, and key capabilities
---
```

Skills should define:
- **Capabilities**: What the skill can do
- **Usage**: When to activate (trigger conditions)
- **Approach**: Step-by-step execution plan
- **Output Format**: Expected result structure

See `SKILL_BEST_PRACTICES.md` for complete requirements.

### MCP Server Configuration
MCP servers are defined in the plugin's `plugin.json`:
```json
{
  "mcp": {
    "servers": {
      "server-name": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem"],
        "description": "Provides filesystem access",
        "env": {
          "API_KEY": "${API_KEY}"
        }
      }
    }
  }
}
```
- Use `npx -y` for npm packages (auto-installs if missing)
- Reference environment variables with `${VAR_NAME}` syntax
- Never commit secrets or API keys

## Marketplace Owner

Current owner: **Tasanakorn Phaipool** (tasanakorn@gmail.com)

Users install this marketplace with:
```bash
/plugin marketplace add tasanakorn/cc-marketplace  # or your-username/cc-marketplace
```

## Reference Documentation

- `README.md` - Installation and quick start
- `CONTRIBUTING.md` - Contribution guidelines
