# Agent Guidelines for isolate Project

## Project Overview

This is a bash-based repository containing the `isolate` script for running
AI tools (opencode, claude, gemini) in an isolated environment using bubblewrap.
The script provides secure sandboxing with multi-tool support, Docker integration,
and customizable mount options.

## Build/Test Commands

- **No build system**: This is a simple bash script project
- **Testing**: Run `./isolate --help` to test basic functionality
- **Dependencies check**: Verify `bwrap` and at least one AI tool (opencode, claude, gemini) are installed
- **Script validation**: Use `shellcheck isolate` for bash linting
- **Version check**: Run `./isolate --iso-version` to check script version

## Code Style Guidelines

### Bash Scripting

- Use `#!/bin/bash` shebang
- Enable strict mode: `set -euo pipefail`
- Support debug tracing with `TRACE` environment variable
- Use double quotes for variable expansions: `"$VARIABLE"`
- Prefer `$(command)` over backticks for command substitution
- Use `[[ ]]` for conditionals instead of `[ ]`
- Check command existence with `command -v` not `which`
- Implement proper signal handling for clean termination
- Support version reporting with `--iso-version` flag

### Multi-tool Support

- Support opencode (default), claude, and gemini tools
- Validate tool-specific requirements and configurations
- Handle tool-specific read-write directories and files
- Allow tool selection as first argument

### Error Handling

- Always check if required commands exist before use
- Provide helpful error messages with installation instructions
- Exit with appropriate error codes (0 for success, 1+ for errors)
- Use descriptive error messages

### Security Best Practices

- Resolve symlinks with `readlink -f` for security
- Use absolute paths for critical operations
- Validate file paths and existence before operations
- Implement tmpfs overlays to hide sensitive directories
- Support optional Docker socket access with explicit `--docker` flag
- Use bubblewrap for proper sandboxing and isolation

### Command Line Interface

- Support `--rw`, `--ro`, and `--hide` mount options
- Use `--` delimiter to separate mount options from tool arguments
- Implement `--docker` flag for Docker socket access
- Support `--iso-dry-run` for testing mount configurations
- Provide helpful error messages with installation instructions

## Version Control

This project uses Jujutsu for version control.

### Workflow

- After every prompt/change, run `jj status` to check for modifications
- If changes are detected, commit them with: `jj commit -m "description" --author "agent <agent@jgaines.com>"` where agent is the agent's name (i.e. opencode, claude, gemini)
- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible

## Project Structure

- `isolate` - Main bash script providing sandboxed AI tool execution
- `README.md` - Comprehensive documentation with usage examples
- `GEMINI.md` - Gemini-specific agent guidelines
- `AGENTS.md` - General agent guidelines (this file)
- `LICENSE` - MIT license file

## Features Supported

- Multi-tool support (opencode, claude, gemini)
- Secure bubblewrap sandboxing
- Custom read-only and read-write mounts
- Directory hiding with tmpfs overlays
- Docker socket integration
- Debug tracing with TRACE environment variable
- Signal handling for clean termination
