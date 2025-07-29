# Agent Guidelines for Instructions Collection

## Project Overview
This is a documentation and instruction collection repository for AI agent best practices, focusing on GitHub Copilot and other coding assistants.

## Build/Test Commands
- **No build system**: This is a documentation repository with bash scripts
- **Script validation**: Use `shellcheck gather` for bash linting
- **Test collection**: Run `./gather` to collect instruction files from other repos

## Code Style Guidelines

### Bash Scripting (for gather script)
- Use `#!/bin/bash` shebang and `set -e` for safety
- Use double quotes for variable expansions: `"$VARIABLE"`
- Prefer `$(command)` over backticks
- Use `[[ ]]` for conditionals instead of `[ ]`

### Documentation
- Use clear, concise language in instruction files
- Follow markdown best practices with proper heading hierarchy
- Include practical examples with code blocks
- Keep instructions under 20 lines for agent consumption

## Version Control
This project uses Jujutsu for version control following the Copilot instructions pattern.

### Workflow
- After changes, run `jj status` to check modifications
- If changes exist, commit with: `jj commit -m "description" --author "opencode <opencode@jgaines.com>"`
- Use descriptive commit messages explaining what was changed