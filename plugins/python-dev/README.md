# Python Dev Plugin

Modern Python development plugin with uv tooling, PEP8 standards, and minimal setup.

## Overview

This plugin helps you create modern Python projects with ultra-fast setup (1 question, smart defaults). It uses `uv` for package management, enforces PEP8 standards with type hints, and follows modern Python best practices.

## Features

**Slash Command:**
- `/python-dev:python-setup` - Ultra-fast Python project setup (1 question, smart defaults)

**Agent Skills:**
- `python-code-review` - Auto-reviews Python code for PEP8, type hints, best practices
- `python-project-setup` - Auto-scaffolds Python projects when requested

## Quick Start

```bash
# Create your project
mkdir my-app && cd my-app

# Setup Python project
/python-dev:python-setup

# Just press Enter for CLI with click+rich (default)
# Or select:
# [1] CLI with click+rich (default)
# [2] CLI with argparse
# [3] CLI with typer
# [4] TUI with textual
# [5] Generic Python Project

# That's it! Project ready to use.
```

## What You Get

```
my-app/                    # Your current directory
├── src/my_app/
│   ├── __init__.py
│   └── __main__.py
├── pyproject.toml         # Minimal: ruff + mypy
├── README.md
└── .gitignore
```

**Auto-detected:**
- ✅ Package name from folder name
- ✅ Python version from environment
- ✅ Current directory as project root

**Minimal by default:**
- ✅ No tests (add when ready)
- ✅ No extra files
- ✅ Only ruff + mypy for dev

## Commands

```bash
# Run your app
uv run python -m my_app

# Lint code
uv run ruff check .
uv run mypy src

# Add dependencies
uv add requests
uv add --dev pytest
```

## Installation

### From cc-marketplace

```bash
# Add the marketplace
/plugin marketplace add tasanakorn/cc-marketplace

# Install the plugin
/plugin install python-dev
```

### Manual Installation

```bash
# Copy to Claude Code plugins directory
cp -r plugins/python-dev ~/.claude/plugins/

# Restart Claude Code
```

## Usage

### Automatic Project Setup (Skill)

Simply describe what you want to Claude:

```
"I need to create a new CLI application called my-tool"
```

Claude will automatically:
1. Detect it's a Python project setup task
2. Use the python-project-setup skill
3. Ask clarifying questions
4. Generate the complete project structure
5. Provide setup instructions

### Manual Project Setup (Command)

For more control, use the command directly:

```bash
/python-setup
```

Claude will guide you through:
1. Project name and description
2. Project type selection
3. Library preferences
4. Structure generation
5. Setup completion

### Code Review (Skill)

Ask Claude to review your Python code:

```
"Can you review this Python code for best practices?"
```

Claude will automatically:
1. Check PEP8 compliance
2. Validate type hints
3. Identify anti-patterns
4. Suggest improvements
5. Provide detailed feedback

## Project Structure

The plugin creates Python projects following this structure:

```
project-name/
├── src/
│   └── package_name/
│       ├── __init__.py          # Package initialization
│       ├── __main__.py          # Entry point
│       ├── py.typed             # Type hints marker
│       └── (application modules)
├── tests/
│   ├── __init__.py
│   └── test_basic.py
├── pyproject.toml               # Modern Python configuration
├── README.md
├── .gitignore
├── .python-version              # Python version for uv
└── uv.lock                      # Dependency lock file
```

## Modern Python Standards

This plugin enforces and promotes:

### Use uv Instead of pip

```bash
# ❌ Old way
pip install package
python -m venv .venv
pip install -r requirements.txt

# ✅ Modern way with uv
uv add package
uv venv
uv sync
```

### PEP8 Compliance

- **Naming**: snake_case for functions/variables, PascalCase for classes
- **Line length**: 88 characters (Black standard)
- **Imports**: Organized (stdlib → third-party → local)
- **Formatting**: 4-space indentation, proper spacing

### Type Hints Everywhere

```python
# ❌ No type hints
def process(data):
    return data.upper()

# ✅ Proper type hints
def process(data: str) -> str:
    return data.upper()
```

### Modern Type Syntax (Python 3.9+)

```python
# ❌ Old typing imports
from typing import List, Dict
def get_items() -> List[Dict[str, int]]:
    pass

# ✅ Modern built-in syntax
def get_items() -> list[dict[str, int]]:
    pass
```

### src/ Layout

Prevents import issues and packaging problems:

```
src/package_name/    # Your package here
tests/               # Tests outside src/
```

## Example Workflows

### Creating a CLI Application

**User:** "Create a CLI tool called 'data-processor' for processing CSV files"

**Claude (using python-project-setup skill):**
1. Detects CLI application need
2. Recommends click + rich + inquirer
3. Creates project structure
4. Generates pyproject.toml with dependencies
5. Creates CLI entry point
6. Provides usage instructions

**Result:**
```python
# src/data_processor/__main__.py
import click
from rich.console import Console

console = Console()

@click.command()
@click.argument("file", type=click.Path(exists=True))
def main(file: str) -> None:
    """Process a CSV file."""
    console.print(f"[green]Processing {file}...[/green]")

if __name__ == "__main__":
    main()
```

### Reviewing Existing Code

**User:** "Review this code for issues"

```python
def get_users(filter=[], limit=None):
    users = database.query("SELECT * FROM users")
    if filter:
        users = [u for u in users if u in filter]
    return users[:limit]
```

**Claude (using python-code-review skill):**

```markdown
## Python Code Review Results

### Critical Issues 🔴
1. **Line 1**: Mutable default argument `filter=[]`
   - This can cause bugs when function is called multiple times
   - Fix: Use `filter: list | None = None`

2. **Line 1**: Missing type hints
   - Add proper type annotations

### Improved Version:
```python
def get_users(
    filter: list[str] | None = None,
    limit: int | None = None
) -> list[User]:
    """Get filtered users from database.
    
    Args:
        filter: Optional list of user IDs to filter by
        limit: Optional maximum number of users to return
        
    Returns:
        List of User objects matching criteria
    """
    if filter is None:
        filter = []
        
    users = database.query("SELECT * FROM users")
    if filter:
        users = [u for u in users if u.id in filter]
    
    return users[:limit] if limit else users
```

### Next Steps:
- Add docstring
- Use context manager for database query
- Consider SQL-level filtering for better performance
```

### Migrating Existing Project to uv

**User:** "Help me migrate my project from pip to uv"

**Claude (using python-project-setup skill):**
1. Detects existing project
2. Analyzes current structure
3. Reads requirements.txt
4. Creates pyproject.toml
5. Provides migration steps

```bash
# 1. Initialize uv
uv venv

# 2. Import existing requirements
uv pip install -r requirements.txt

# 3. Generate lock file
uv sync

# 4. Update your workflow
# Replace: pip install package
# With: uv add package
```

## pyproject.toml Template

The plugin generates comprehensive `pyproject.toml` files:

```toml
[project]
name = "my-project"
version = "0.1.0"
description = "Project description"
authors = [
    {name = "Your Name", email = "you@example.com"}
]
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "click>=8.1.0",
    "rich>=13.7.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0.0",
    "pytest-cov>=4.1.0",
    "ruff>=0.1.0",
    "mypy>=1.8.0",
]

[project.scripts]
my-project = "my_package.__main__:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
line-length = 88
target-version = "py311"
select = ["E", "F", "I", "N", "W", "UP"]

[tool.mypy]
python_version = "3.11"
strict = true
warn_return_any = true
warn_unused_configs = true

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py"]
```

## Development Commands Reference

Once your project is set up:

| Task | Command |
|------|---------|
| Add dependency | `uv add package-name` |
| Add dev dependency | `uv add --dev package-name` |
| Remove dependency | `uv remove package-name` |
| Update dependencies | `uv sync` |
| Run script | `uv run python script.py` |
| Run application | `uv run python -m package_name` |
| Run tests | `uv run pytest` |
| Test coverage | `uv run pytest --cov=package_name` |
| Lint code | `uv run ruff check .` |
| Format code | `uv run ruff format .` |
| Type check | `uv run mypy src` |

## Prerequisites

### Required

- **Claude Code** installed and running
- **Python 3.11+** recommended
- **uv** package manager

### Installing uv

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Verify installation
uv --version
```

## Best Practices Enforced

### ✅ DO

- Use `uv` for all dependency management
- Add type hints to all functions
- Follow PEP8 naming conventions
- Use `src/` layout for packages
- Include docstrings on public APIs
- Write tests from the start
- Use context managers for resources
- Prefer f-strings for formatting
- Keep functions small and focused

### ❌ DON'T

- Use `pip` or `python` directly (use `uv` instead)
- Use mutable default arguments
- Use bare `except:` clauses
- Use wildcard imports (`from module import *`)
- Leave functions without type hints
- Write functions longer than 20 lines
- Use global variables
- Forget to write docstrings

## Common Patterns

### CLI Application with Click

```python
import click
from rich.console import Console

console = Console()

@click.group()
@click.version_option()
def main() -> None:
    """My CLI application."""
    pass

@main.command()
@click.argument("name")
@click.option("--greeting", default="Hello", help="Greeting to use")
def greet(name: str, greeting: str) -> None:
    """Greet someone."""
    console.print(f"[bold]{greeting}, {name}![/bold]")

if __name__ == "__main__":
    main()
```

### TUI Application with Textual

```python
from textual.app import App, ComposeResult
from textual.widgets import Header, Footer, Button, Static

class MyTUI(App):
    """A simple TUI application."""
    
    TITLE = "My Application"
    
    def compose(self) -> ComposeResult:
        yield Header()
        yield Static("Welcome!")
        yield Button("Click me", id="btn")
        yield Footer()
    
    def on_button_pressed(self, event: Button.Pressed) -> None:
        """Handle button clicks."""
        self.notify("Button clicked!")

def main() -> None:
    """Run the TUI application."""
    app = MyTUI()
    app.run()

if __name__ == "__main__":
    main()
```

## Troubleshooting

### uv not found

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add to PATH (Linux/macOS)
export PATH="$HOME/.cargo/bin:$PATH"

# Restart terminal and verify
uv --version
```

### Type hints not working

```python
# Ensure you're using Python 3.9+ for modern syntax
python --version

# For older Python, use typing imports
from typing import List, Dict
```

### Skills not activating

- Make sure plugin is installed: `/plugin list`
- Use natural language to trigger skills
- For manual control, use `/python-setup` command

## Integration

This plugin works seamlessly with:
- New Python project creation
- Existing project modernization
- Code review workflows
- Migration from pip to uv
- Type hints adoption
- PEP8 compliance checking
- Dependency management tasks

## Plugin Structure

```
python-dev/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata
├── commands/
│   └── python-setup.md          # /python-setup command
├── skills/
│   ├── python-project-setup/
│   │   └── SKILL.md            # Project setup skill
│   └── python-code-review/
│       └── SKILL.md            # Code review skill
└── README.md                    # This file
```

## Contributing

Suggestions for improvements are welcome! Consider:
- Additional project templates
- More library recommendations
- Enhanced code review rules
- Additional skills for testing/deployment

## Resources

- [uv documentation](https://github.com/astral-sh/uv)
- [PEP8 Style Guide](https://peps.python.org/pep-0008/)
- [Python Type Hints](https://docs.python.org/3/library/typing.html)
- [Click Documentation](https://click.palletsprojects.com/)
- [Textual Documentation](https://textual.textualize.io/)
- [Rich Documentation](https://rich.readthedocs.io/)

## License

MIT - See repository LICENSE file

## Author

Tasanakorn Phaipool <tasanakorn@gmail.com>

## Support

For questions or issues, refer to the main marketplace README or open an issue in the repository.
