# Copilot Instructions for govalheim

## Project Overview
This is a Go application for managing Valheim profiles that provides both CLI and GUI interfaces with comprehensive configuration management. A Valheim profile consists of configuration files, plugins/mods, characters, and worlds combined into a single manageable unit. This is a complete rewrite in Go of the original Python-based valheim_manager. The CLI is built with Cobra framework, the GUI uses Fyne v2, and configuration is managed with Viper. The project follows a clean, minimal structure with the main entry point delegating to command handlers in the `cmd/` package.

## Version Control
This repository uses Jujutsu (jj):
- Before making any changes, create a new working copy:
    ```shell
    jj new
    jj desc -m "description"  --author "copilot <copilot@jgaines.com>"
    ```

## Architecture & Structure
- **Entry Point**: `main.go` - Simple delegation to `cmd.Execute()`
- **CLI Framework**: Uses Spf13 Cobra for command structure
- **GUI Framework**: Uses Fyne v2 for native cross-platform GUI
- **Configuration**: Uses Spf13 Viper for configuration management (YAML/JSON/TOML/env vars)
- **Commands**: All CLI commands are defined in `cmd/` package, starting with `root.go`
- **GUI Package**: `gui/` contains Fyne application code
- **Config Package**: `config/` contains Viper configuration management
- **Module**: `github.com/jgaines/govalheim` - Standard Go module structure

## Development Environment
- **Go Version**: 1.24.5 (managed via mise - see `mise.toml`)
- **Tool Management**: Uses mise for Go version and cobra-cli tool management
- **Editor Config**: VSCode settings point to mise-managed Go installation
- **Version Control**: Uses jj (Jujutsu) alongside git (git disabled in VSCode)

## Key Development Commands
```bash
# Install/sync tools via mise
mise install

# Build the application
go build -o govalheim

# Run CLI interface
go run main.go [command] [flags]

# Run GUI interface
go run main.go gui

# Configuration management
go run main.go config show          # Show current configuration
go run main.go config init          # Create default config file
go run main.go --config custom.yaml # Use custom config file

# Add new cobra commands
cobra-cli add [command-name]

# Cross-compile for different platforms (GUI apps)
GOOS=windows go build -o govalheim.exe
GOOS=darwin go build -o govalheim-mac
```

## Configuration Management
- **Config File**: `govalheim.yaml` (YAML), `govalheim.json` (JSON), or `govalheim.toml` (TOML)
- **Search Paths**: Current directory, home directory
- **Environment Variables**: Prefix with `GOVALHEIM_` (e.g., `GOVALHEIM_APP_DEBUG=true`)
- **CLI Flags**: `--config path/to/config.yaml` to specify custom config
- **Priority**: CLI flags > Environment variables > Config file > Defaults
- **App Metadata**: App name and version are hardcoded in `app/app.go`, not configurable

## Code Patterns & Conventions
- **Cobra Structure**: Commands are organized as separate files in `cmd/` package
- **GUI Structure**: Fyne components are organized in `gui/` package, use `config.Get()` for settings
- **Configuration**: Use `config.Get()` to access current configuration, all settings are strongly typed.  Do not use Fyne Preferences.
- **Error Handling**: CLI exits with status code 1 on errors (see `cmd/root.go:Execute()`)
- **Command Definition**: Follow the existing pattern in `root.go` with Use, Short, Long descriptions
- **Flag Management**: Use Cobra's persistent flags for global options, local flags for command-specific options
- **Fyne UI Patterns**: Use widget-based components, container layouts for structure, event-driven interactions

## File Organization
- `main.go`: Application entry point - keep minimal
- `cmd/root.go`: Root command definition, Execute() function, and config initialization
- `cmd/[command].go`: Individual command implementations (CLI commands)
- `cmd/gui.go`: GUI launcher command
- `cmd/config.go`: Configuration management commands
- `app/app.go`: Application metadata (name, version) - hardcoded constants
- `gui/app.go`: Main GUI application structure and layout
- `gui/[component].go`: Additional GUI components (when added)
- `config/config.go`: Viper configuration setup, structs, and management functions
- `govalheim.yaml`: Default configuration file (created by `config init`)
- `mise.toml`: Tool version management - update when changing Go or tool versions
- `.vscode/settings.json`: Points to mise-managed tools - maintain these paths

## Development Workflow
1. Use `mise install` to ensure correct Go and tool versions
2. Add new CLI commands via `cobra-cli add [name]` which creates new files in `cmd/`
3. Add new GUI components by creating files in `gui/` package
4. Extend configuration by adding fields to structs in `config/config.go`
5. Test CLI with `go run main.go [command]`
6. Test GUI with `go run main.go gui`
7. Test configuration with `go run main.go config show`
8. Build final binary with `go build`
9. Cross-compile for target platforms as needed

## Important Notes
- The project uses mise for reproducible tool management - always use the specified versions
- Git is disabled in VSCode settings, suggesting jj is the preferred VCS
- GUI and CLI are separate interfaces to the same application, both use the same configuration
- Fyne v2 provides native performance and cross-platform compatibility with widget-based UI
- Viper handles config files, environment variables, and CLI flags with a clear precedence order
- Configuration is strongly typed through Go structs with mapstructure tags
- The binary file `govalheim` is automatically ignored during config file search to prevent parsing errors
- No tests currently exist - follow Go testing conventions when adding them
