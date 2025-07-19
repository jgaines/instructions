## File Management

When creating files:
- Use meaningful, descriptive names for permanent scripts
- Prefix temporary test scripts with `tmp-`
- Create temporary files in `~/bin/steff` when possible
- Always document at the top of each file whether it's permanent or temporary
- Never use notebook format anywhere in this repository

When cleaning up:
- Remove all temporary scripts after completing tasks unless explicitly requested to keep them
- Track temporary files in a list for later cleanup
- Move useful test scripts to `~/bin/tests` if they have lasting value
- Keep `~/bin` uncluttered - only permanent, useful scripts belong there

## Code Quality Standards

For shell scripts:
- Include appropriate shebangs (`#!/usr/bin/env bash`, `#!/usr/bin/env zsh`, etc.)
- Make scripts executable with proper permissions
- Add comprehensive error handling for scripts that could fail
- Follow shell scripting best practices for the target shell
- Use consistent formatting and style across all scripts
- Test scripts thoroughly before marking them as permanent

For documentation:
- Add clear comments explaining what each script does
- Include usage examples in script headers
- Update README.md when adding permanent scripts to `~/bin`
- Follow the existing documentation style and format

## Repository Structure

This repository follows these organization principles:
- `~/bin` - Only permanent, useful scripts
- `~/bin/tests` - Test scripts that need to be preserved
- `~/bin/steff` - Temporary files and test scripts
- Follow established naming conventions in each directory
- Don't leave temporary files laying around

## Version Control

This repository uses Jujutsu (jj) in colocated mode on top of git:
- Before making any changes, create a new revset:
    ```shell
    jj new
    jj desc -m "description"  --author "copilot <copilot@jgaines.com>"
    ```

## Security Considerations

When working with scripts:
- Never include passwords, tokens, or sensitive data in scripts
- Reference external credential files from secure locations (e.g., `~/.ssh/`)
- Add file permission checks for credential files (should be 600)
- Include input sanitization for user-provided arguments
- Add error handling for missing credential files

## Automation Guidelines

When generating code for automation:
- Prefer bash for automation scripts (cleaner, more predictable)
- Include comprehensive error checking and logging
- Make scripts idempotent where possible
- Add progress indicators for long-running operations
- Use absolute paths and proper quoting in scripts
