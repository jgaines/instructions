# Agent Instructions

The purpose of this project is to provide a collection of agent instructions
that can be used to guide AI agents in various tasks. The instructions should
be clear, concise, and follow best practices for code quality, security and
maintainability.

## Source Control

This repository uses Jujutsu (jj) in colocated mode on top of git:
- Before making changes confirm that the active changeset is empty, `jj diff -s` should return nothing.
- If the active revest is not empty, create a new revset with `jj new`
- Add a description to the revset specifying the author as copilot: `jj new -m "description" --author="copilot <copilot@jgaines.com>"`


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
