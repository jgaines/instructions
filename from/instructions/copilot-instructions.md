# Agent Instructions

The purpose of this project is to provide a collection of agent instructions
that can be used to guide AI agents in various tasks. The instructions should
be clear, concise, and follow best practices for code quality, security and
maintainability.

## Version Control

I prefer to use Jujutsu for version control.

### Workflow

- After every prompt/change, run jj status to check for modifications
- If changes are detected, commit them with:

  ```bash
  jj commit -m 'description' --author 'copilot <copilot@jgaines.com>'
  ```

- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible

## General Guidelines

- Always state in the chat window that you are following these instructions.
- Always write clear, concise instructions and documentation.
- Follow the project's coding style and naming conventions.

## Project Structure

- Place documentation in README.md and update it as the project evolves.

## Documentation

- Update README.md with setup instructions, usage examples, and contribution guidelines.
- Document any non-obvious design decisions or dependencies.

## Collaboration

- Communicate changes and design decisions with the team.
- Review and test code before merging.
- Respect existing code and refactor only with clear justification.

## Support

For questions or issues, contact the project maintainer or open an issue in the repository.

## References

- [Jujutsu Documentation](https://jj-vcs.github.io/jj/latest/)
