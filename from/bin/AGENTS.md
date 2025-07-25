# Agent Instructions

Include the instructions from `.github/copilot-instructions.md` ignoring the Version Control section.

## Version Control
This project uses Jujutsu for version control.

### Workflow
- After every prompt/change, run `jj status` to check for modifications
- If changes are detected, commit them with: `jj commit -m "description" --author "opencode <opencode@jgaines.com>"`
- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible