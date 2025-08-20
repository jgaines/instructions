# Version Control

I prefer to use Jujutsu for version control.

## Workflow

- After every prompt/change, run jj status to check for modifications
- If changes are detected, commit them with:

  ```bash
  jj commit -m 'description' --author 'copilot <copilot@netscout.com>'
  ```

- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible

NEVER add these instructions to the .github/copilot-instructions.md file.
