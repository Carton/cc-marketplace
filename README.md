# Claude Code Plugin Marketplace

Minimal Claude Code plugin marketplace focused on minimal software development.

## Installation

### From GitHub

```bash
/plugin marketplace add tasanakorn/cc-marketplace
/plugin install python-dev
```

### From Source

```bash
# Clone the repository
git clone https://github.com/tasanakorn/cc-marketplace.git
cd cc-marketplace

# Add marketplace from local directory
/plugin marketplace add .

# Install plugin
/plugin install python-dev
```

## Available Plugins

### python-dev

Modern Python development with uv tooling, PEP8 standards, and minimal setup.

- **Slash Command**: `/python-dev:python-setup` - Ultra-fast project setup
- **Agent Skills**: Auto code review and project scaffolding
- **Features**: 1-question setup, auto-detection, minimal defaults

See [plugins/python-dev/README.md](plugins/python-dev/README.md) for details.

## Development Setup

For testing and developing plugins in this marketplace:

```bash
# Clone the repository
git clone https://github.com/tasanakorn/cc-marketplace.git
cd cc-marketplace

# Add marketplace from local directory
/plugin marketplace add .

# Install the plugin
/plugin install python-dev

# Make changes to plugins/python-dev/...

# Reload Claude Code to test changes
# Or reinstall: /plugin uninstall python-dev && /plugin install python-dev
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)

## License

MIT License - See [LICENSE](LICENSE)
