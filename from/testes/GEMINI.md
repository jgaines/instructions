# Gemini Project Guidelines: Valheim Manager

This document outlines the development guidelines for the testes project.

## IMPORTANT: Version Control

This repository uses Jujutsu (jj) in colocated mode on top of git.
Jujutsu is *not* git and has a different workflow.
Follow these steps for each distinct task or set of related changes:

1. **Start a new task:** Run `jj new` to create a new, empty revset for your
   changes. This isolates your work from other tasks.
2. **Implement changes:** Make all necessary file modifications for the current
   task. Your changes are automatically included in the working-copy commit.
3. **Describe and author:** After completing the changes for the current task,
   run `jj describe -m "Your concise commit message" --author="gemini
   <gemini@jgaines.com>"` to add a description to the current revset.

## Project Overview

Testes is a test repo for the purposes of evaluating Gemini and shaking out
issues it has with properly following my Jujutsu workflow.  As a secondary goal,
it is also a testbed for Python GUI testing.

The key goals of this project are:

* Get Gemini to properly follow my Jujutsu workflow.
* Try out some Python GUI frameworks, starting with customtkinter.

## Development Environment

This project uses uv for dependency management. To
set up the development environment, follow these steps:

1. Install uv if you haven't already.
2. Install the project dependencies

## Contribution Guidelines

### Project Configuration

Whenever possible all tool configuration should be in the pyproject.toml
file. This includes configurations for linters, formatters, and testing tools.

### Code Style

This project uses [Ruff](https://beta.ruff.rs/docs/) for linting and code
formatting.

### Testing

This project uses tox and pytest-cov for testing.
