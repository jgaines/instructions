# Copilot Instructions for rvalheim

## Project Overview
This is a Rust application for managing Valheim profiles that provides both CLI and GUI interfaces with comprehensive configuration management. A Valheim profile consists of configuration files, plugins/mods, characters, and worlds combined into a single manageable unit. This is a complete rewrite in Rust of the original Python-based valheim_manager.

## Version Control
This repository uses Jujutsu (jj):
- Before making any changes, create a new working copy:
    ```shell
    jj new
    jj desc -m "description"  --author "copilot <copilot@jgaines.com>"
    ```
