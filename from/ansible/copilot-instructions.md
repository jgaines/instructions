# GitHub Copilot Instructions for eng-tools/ansible

## General Guidelines
- Always state in the chat window all instruction files you are following (eg:
  repository specific instructions, user-only instructions, workspace instructions).
- Never change files outside of this repository unless explicitly instructed.
- Always write clear, concise instructions and documentation.
- Follow the project's coding style and naming conventions.

## Source Control (Git)

- **Commit Identity**: Always commit with your system default as the committer and set
  Copilot as the author, with co-author attribution using command line arguments (never
  change global config):
  ```bash
  git commit --author="GitHub Copilot <copilot@netscout.com>" -m "commit message

  Issue: <branch_id>
  Co-authored-by: $(git config user.name) <$(git config user.email)>"
  ```
- **Commit Strategy**: Use multiple logical commits when making complex changes
  - Separate commits for different components (scripts, configuration, documentation)
  - Use descriptive commit messages following conventional commits format
  - Example:
    ```
    feat(keyauth): add SSH key cache update script
    fix(systemd): correct service file permissions
    docs(readme): update deployment instructions
    ```
- **Commit Message Format**:
  - Use conventional commit format: `type(scope): description`
  - Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`
  - Keep first line under 50 characters
  - Add detailed description if needed after blank line

## Security Considerations

- Never include passwords, tokens, or sensitive data in code
- Reference external credential files from secure locations (e.g., `~/.ssh/`,
  `/etc/ssl/certs/`)
- Add file permission checks for credential files (should be 600 or 400)
- Include input sanitization for user-provided arguments
- Add comprehensive error handling for missing credential files
- Use systemd security hardening features in service files
- Validate SSL certificates and use secure communication protocols
- Follow principle of least privilege for service accounts

## Ansible-Specific Guidelines

- **YAML Style**:
  - Use 2-space indentation
  - Quote strings when they contain variables or special characters
  - Use fully qualified collection names (e.g., `ansible.builtin.copy`)
  - Use descriptive task names

- **Code Quality**:
  - All Ansible files must be linted with `ansible-lint`:
    ```bash
    ansible-lint .
    ansible-lint --fix .
    ```
  - Fix all linting issues before committing
  - Use `ansible-lint` configuration file for project-specific rules

- **Template Files**:
  - Use `.j2` extension for Jinja2 templates
  - Comment complex template logic
  - Test templates with different variable combinations

- **Role Structure**:
  - Follow Ansible Galaxy role structure
  - Use `tasks/main.yml` as entry point
  - Separate complex logic into included task files
  - Use appropriate tags for task organization
  - When tasks are moved into separate files, add tags to the `import_tasks`
    directive only, not to individual tasks in the imported file

- **Variables**:
  - Use descriptive variable names
  - Document variables in `defaults/main.yml`
  - Use `group_vars` and `host_vars` appropriately

- **Service Configuration**:
  - Use systemd security hardening features
  - Run services with minimal required privileges
  - Use appropriate service types (`oneshot` for batch operations)
  - Include proper service dependencies

## Documentation Rules

- ALL documentation must be in Markdown format
- NEVER use notebook format
- Use maximum 80 character line width for all documentation
- README.md must describe repository purpose and use
- Include deployment instructions and examples
- Document all variables and their purposes
- Provide troubleshooting guides
- Include security considerations section

## Python Requirements

- Use Python 3.12 or later
- Use `ruff` for code formatting and linting for Python code:
  ```bash
  ruff format .
  ruff check . --fix
  ```
- Use `docformatter` for docstring formatting:
  ```bash
  docformatter --in-place --black **/*.py
  ```
- Use `ruff check --select I . --fix` for import sorting
- Use `basedpyright` for type checking:
  ```bash
  basedpyright .
  ```
- Use `uv` for package and virtual environment management:
  ```bash
  uv venv
  uv pip install -r requirements.txt
  ```

## File Organization

- Use clear, descriptive file names
- Follow existing naming patterns in the repository
- Group related files logically:
  - Scripts in `files/` directory
  - Templates in `templates/` directory
  - Task files in `tasks/` directory
  - Variables in `vars/`, `defaults/`, `group_vars/`, `host_vars/`
- Use consistent file extensions:
  - `.yaml` for Ansible files
  - `.j2` for Jinja2 templates
  - `.py` for Python scripts
  - `.md` for documentation

## Testing Requirements

- Include test scripts for validation
- Use dry-run modes where applicable
- Test with mock servers for development
- Validate configuration before deployment
- Include both positive and negative test cases

## Deployment Guidelines

- Use Ansible tags for selective deployment
- Include validation steps in playbooks
- Support both check mode and live deployment
- Handle distribution differences (RedHat vs Debian/Ubuntu)
- Provide rollback procedures when applicable
