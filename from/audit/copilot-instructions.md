# AI Assistant Instructions

## CRITICAL: Source Control Protocol

- ALWAYS create a new revset BEFORE making any changes:

   ```bash
   jj new -m "copilot: description of changes"
   ```

- ALWAYS verify the current revset with:

   ```bash
   jj log --no-pager
   jj show --no-pager
   ```

- NEVER change files outside of this repository unless explicitly instructed.

## Documentation Rules

- ALL documentation must be in Markdown format
- NEVER use notebook format
- README.md must describe repository purpose and use
- Use meaningful, descriptive file names. Examples: install_mise.sh, install_brew.sh, backup_brew.sh

## Python Requirements

- Use Python 3.12 or later, except backup_audit.py which uses Python 3.8
- Use `ruff` for code formatting and linting
- Use `docformatter` for docstring formatting
- Use `ruff check --select I . --fix` for import sorting
- Use `black` for code formatting
- Use `basedpyright` for type checking
- Use `pytest-cov` for testing
- Use `uv` for package and virtual environment management

## Bash Scripting Requirements

- Scripts MUST be simple bash scripts
- Include only minimal error checking
- Use "#!/usr/bin/env bash" shebang.
- Use simple linear logic, small regex matches, and basic string manipulation
- Limit user choices to y/n only
- Scripts should include 'cd $(dirname $0) || exit 1' after setting flags to ensure they run in the correct directory

## File Organization

- Use clear, descriptive file names
- Follow existing naming patterns
- Group related files logically
