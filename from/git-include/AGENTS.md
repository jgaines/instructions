# Development Guidelines for git-include

## Project Overview
Simple Python 3 CLI tool for managing inverted .gitignore files using git subcommands.

## Commands
- **Test**: `python3 -m py_compile git-include` (syntax check)
- **Run**: `python3 git-include [spec]` or `./git-include [spec]`
- **No formal test suite**: Manual testing recommended

## Code Style & Conventions
- **Python 3.8** standard library only (no external dependencies)
- **4-space indentation**, no tabs
- **snake_case** for functions and variables
- **Descriptive function names** with docstrings
- **Error handling**: Try/except with user-friendly messages
- **Input validation**: Check file existence, git repo presence
- **Interactive prompts**: Use EOFError/KeyboardInterrupt handling
- **File operations**: Use context managers (`with open()`)
- **Path handling**: Mix of os.path and pathlib as needed

## Architecture
- Single-file script with clear function separation
- Main CLI logic in `main()` function
- Helper functions for file operations and user interaction
- No classes or complex data structures needed

## Version Control
This project uses Jujutsu for version control.

### Workflow
- After every prompt/change, run `jj status` to check for modifications
- If a file that you created does not show up in the status, run `jj include <file>` to include it
- If changes are detected, commit them with: `jj commit -m 'description' --author 'opencode <opencode@jgaines.com>'`
- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible
