# Contributing to CC Marketplace

Thank you for your interest in contributing to this Claude Code plugin marketplace! This document provides guidelines for contributing new plugins, improving existing ones, and enhancing documentation.

## Ways to Contribute

- **Add new example plugins**: Showcase different plugin capabilities
- **Improve existing plugins**: Enhance functionality or documentation
- **Fix bugs**: Report or fix issues in plugins
- **Improve documentation**: Make guides clearer and more comprehensive
- **Share feedback**: Suggest improvements or new examples

## Adding a New Plugin

### 1. Plugin Requirements

Your plugin should:
- Demonstrate a specific Claude Code feature or use case
- Include clear, comprehensive documentation
- Follow the established directory structure
- Be well-tested and functional
- Include a descriptive README.md

### 2. Directory Structure

Create your plugin following this structure:

```
plugins/your-plugin/
├── .claude-plugin/
│   └── plugin.json       # Required: Plugin metadata
├── commands/             # Optional: Slash commands
│   └── command.md
├── agents/               # Optional: Subagents
│   └── agent.md
├── skills/               # Optional: Agent skills
│   └── skill-name/
│       └── SKILL.md
├── hooks/                # Optional: Event hooks
│   └── hooks.json
├── mcp/                  # Optional: MCP configurations
│   └── config.json
└── README.md            # Required: Documentation
```

### 3. Plugin Metadata

Create a `plugin.json` file:

```json
{
  "name": "your-plugin",
  "description": "Clear description of what the plugin does",
  "version": "1.0.0",
  "author": {
    "name": "Your Name",
    "email": "your.email@example.com"
  }
}
```

### 4. Documentation Requirements

Your plugin's README.md should include:

- **Overview**: What the plugin does
- **Features**: Key capabilities
- **Installation**: How to install
- **Usage**: How to use (with examples)
- **Structure**: File organization explanation
- **Configuration**: Any required setup
- **Examples**: Practical usage examples
- **License**: Reference to MIT license

### 5. Register Your Plugin

Add your plugin to `.claude-plugin/marketplace.json`:

```json
{
  "name": "your-plugin",
  "description": "Brief description",
  "version": "1.0.0",
  "author": {
    "name": "Your Name",
    "email": "your.email@example.com"
  },
  "source": "./plugins/your-plugin",
  "category": "development"
}
```

**Categories:**
- `development` - Development tools
- `testing` - Testing utilities
- `documentation` - Documentation generators
- `productivity` - Workflow enhancements
- `devops` - Deployment and infrastructure

## Plugin Best Practices

### Commands (Slash Commands)

- Use clear, descriptive filenames (e.g., `review-code.md`)
- Write specific instructions for Claude
- Include examples in the command prompt
- Test the command thoroughly

**Example command structure:**

```markdown
You are a [specialized role] assistant.

When this command is invoked:
1. [Step 1]
2. [Step 2]
3. [Step 3]

Guidelines:
- [Guideline 1]
- [Guideline 2]

Provide output in this format:
[Expected format description]
```

### Skills (Agent Skills)

- Define clear capabilities
- Specify when the skill should activate
- Provide structured output formats
- Include practical examples

**Example skill structure:**

```markdown
# Skill Name

## Overview
[What the skill does]

## Capabilities
- [Capability 1]
- [Capability 2]

## Usage
Activate when:
- [Trigger 1]
- [Trigger 2]

## Approach
1. [Step 1]
2. [Step 2]

## Output Format
[Expected format]
```

### MCP Servers

- Use descriptive server names
- Document required dependencies
- Provide setup instructions
- Include environment variable templates
- Never commit secrets

**Example MCP configuration:**

```json
{
  "mcp": {
    "servers": {
      "server-name": {
        "command": "npx",
        "args": ["-y", "package-name", "arg"],
        "description": "Clear description of server purpose",
        "env": {
          "VAR_NAME": "${VAR_NAME}"
        }
      }
    }
  }
}
```

## Code Style

### File Naming

- Use kebab-case: `my-plugin`, `my-command.md`
- Be descriptive: `code-reviewer.md` not `cr.md`
- Match command names to filenames

### JSON Formatting

- Use 2-space indentation
- Include all required fields
- Keep descriptions concise but clear

### Markdown

- Use headers for structure
- Include code examples with syntax highlighting
- Add links to relevant documentation
- Keep line length reasonable (80-120 characters)

## Testing Your Plugin

Before submitting:

1. **Test installation**
   ```bash
   cp -r plugins/your-plugin ~/.claude/plugins/
   ```

2. **Verify functionality**
   - Run all commands
   - Test skills trigger appropriately
   - Ensure MCP servers connect

3. **Check documentation**
   - All instructions are clear
   - Examples work as shown
   - Links are valid

4. **Review metadata**
   - Version is correct
   - Author info is accurate
   - Description is clear

## Submission Process

### For GitHub Contributors

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b add-my-plugin
   ```

3. **Add your plugin**
   - Create plugin directory
   - Add all necessary files
   - Update marketplace.json

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add my-plugin: [brief description]"
   ```

5. **Push and create PR**
   ```bash
   git push origin add-my-plugin
   ```
   Then create a Pull Request on GitHub

### Commit Message Format

```
Add [plugin-name]: [brief description]

- Feature 1
- Feature 2
- Feature 3
```

Or for updates:

```
Update [plugin-name]: [what changed]

- Improvement 1
- Fix 1
```

## Review Process

Your contribution will be reviewed for:

1. **Functionality**: Does it work as described?
2. **Documentation**: Is it clear and complete?
3. **Code quality**: Follows best practices?
4. **Usefulness**: Provides value to users?
5. **Compatibility**: Works with Claude Code?

## Getting Help

If you need assistance:

1. Check existing plugin examples
2. Review Claude Code documentation
3. Open an issue with your question
4. Join community discussions

## Plugin Ideas

Looking for inspiration? Consider creating plugins for:

- **Language-specific tools**: Python formatters, Go analyzers
- **Framework helpers**: React component generators, Django utilities
- **DevOps automation**: Docker helpers, Kubernetes tools
- **Testing utilities**: Test generators, coverage analyzers
- **Documentation**: API doc generators, README creators
- **Code quality**: Linters, security scanners
- **Project management**: Task trackers, changelog generators

## Recognition

Contributors will be:
- Listed in plugin authorship
- Mentioned in release notes
- Credited in the main README

## Questions?

If you have questions about contributing:

- Open an issue for discussion
- Review existing plugins for examples
- Check Claude Code documentation

Thank you for helping make this marketplace better!
