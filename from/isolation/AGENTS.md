# Agent Guidelines for isolate Project

## Project Overview
This is a bash-based repository containing the `isolate` script for running opencode in an isolated environment using bubblewrap.

## Build/Test Commands
- **No build system**: This is a simple bash script project
- **Testing**: Run `./isolate --help` to test basic functionality
- **Dependencies check**: Verify `bwrap` and `opencode` are installed
- **Script validation**: Use `shellcheck isolate` for bash linting

## Code Style Guidelines

### Bash Scripting
- Use `#!/bin/bash` shebang
- Enable strict mode: `set -euo pipefail`
- Support debug tracing with `TRACE` environment variable
- Use double quotes for variable expansions: `"$VARIABLE"`
- Prefer `$(command)` over backticks for command substitution
- Use `[[ ]]` for conditionals instead of `[ ]`
- Check command existence with `command -v` not `which`

### Error Handling
- Always check if required commands exist before use
- Provide helpful error messages with installation instructions
- Exit with appropriate error codes (0 for success, 1+ for errors)
- Use descriptive error messages

### Security Best Practices
- Resolve symlinks with `readlink -f` for security
- Use absolute paths for critical operations
- Validate file paths and existence before operations

## Version Control
This project uses Jujutsu for version control.

### Workflow
- After every prompt/change, run `jj status` to check for modifications
- If changes are detected, commit them with: `jj commit -m "description" --author "opencode <opencode@jgaines.com>"`
- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible