# AI Assistant Instructions

## Source Control

This repository uses Jujutsu (jj) in colocated mode on top of git:
- Before making changes confirm that the active revset is empty, `jj diff -s` should return nothing.
- If the active revest is not empty, create a new revset with `jj new`
- Add a description to the revset specifying the author as copilot: `jj desc -m "description" --author="copilot <copilot@netscout.com>"`

## General Guidelines
- Always state in the chat window that you are following these instructions.
- Never change files outside of this repository unless explicitly instructed.
- Always write clear, concise instructions and documentation.
- Follow the project's coding style and naming conventions.

## Security Considerations

- Never include passwords, tokens, or sensitive data in code
- Reference external credential files from secure locations (e.g., `~/.ssh/`)
- Add file permission checks for credential files (should be 600)
- Include input sanitization for user-provided arguments
- Add error handling for missing credential files

## Documentation Rules

- ALL documentation must be in Markdown format
- NEVER use notebook format
- README.md must describe repository purpose and use

## Python Requirements

- Use Python 3.12
- Use `ruff` for code formatting and linting
- Use `docformatter` for docstring formatting
- Use `ruff check --select I . --fix` for import sorting
- Use `basedpyright` for type checking
- Use `tox` for testing
- Use `uv` for package and virtual environment management

## File Organization

- Use clear, descriptive file names
- Follow existing naming patterns
- Group related files logically
