# GitHub Copilot Instructions for zigup

## Project Overview

`zigup` is a specialized tool that manages daily development builds of the Zig programming language and ZLS (Zig Language Server) within the mise version manager ecosystem. It fills a gap where mise cannot install bleeding-edge daily builds that are required for projects like ziglings.

## Version Control

This repository uses Jujutsu (jj) in colocated mode on top of git:
- Before making any changes, create a new revset:
    ```shell
    jj new
    jj desc -m "description"  --author "copilot <copilot@jgaines.com>"
    ```

## Architecture & Core Components

### Single-File Design
- **`zigup.py`**: The entire application is contained in one Python script with inline dependencies (PEP 723 format)
- Uses `# /// script` header for uv compatibility, allowing `uv run zigup.py` without separate installation
- Designed for distribution as a standalone script or via package managers

### Key Integration Points
- **mise dependency**: Requires mise to be installed; validates this before proceeding
- **Target directory**: `~/.local/share/mise/installs/zig/` - hardcoded mise structure
- **Version endpoints**: 
  - Zig: `https://ziglang.org/download/index.json`
  - ZLS: `https://releases.zigtools.org/v1/zls/select-version?zig_version={}&compatibility=only-runtime`

### Platform Support Pattern
- Uses `platform.machine().lower() + "-" + platform.system().lower()` for platform detection
- Linux fully implemented, macOS partially implemented (untested), Windows not implemented
- Archive handling varies by platform: `.tar.xz`, `.tar.gz` (with `.zip` commented out for Windows)

## Critical Workflows

### Version Management Logic
```python
# Uses proper semantic version parsing instead of string comparison
# Handles Zig's dev build format: 0.15.0-dev.{build_number}+{commit_hash}
new_version_available = is_newer_version(zig_version_current, zig_version_master)
```

**Critical Bug**: The original string comparison failed when dev build numbers had different digit lengths (e.g., `dev.929` vs `dev.1145`). The `parse_zig_version()` and `is_newer_version()` functions now properly handle Zig's versioning scheme.

### Symlink Strategy
- Extracts to versioned directory: `~/.local/share/mise/installs/zig/{version}/`
- Creates `dev-latest` symlink pointing to current version
- Automatically removes old version directories when upgrading

### Dual Tool Installation
- Installs Zig first, then queries ZLS API with Zig version to get compatible ZLS build
- ZLS is installed within the Zig installation directory structure
- Both tools become available through mise's shim system

## Project-Specific Conventions

### Error Handling Approach
- Uses subprocess return codes and exception catching rather than extensive validation
- Fails fast on missing dependencies (mise not installed)
- Graceful handling of network and extraction errors with user-friendly messages

### Verbosity Levels
- Default: minimal output showing version comparison and major operations
- `-v`: shows download URLs and symlink operations  
- `-vv`: dumps full JSON responses from version APIs

### Archive Extraction Pattern
```python
def extract_tar_strip_leading_dir(tar_file, extract_dir, mode, strip_components=1):
```
Custom extraction function strips leading directory components to flatten archive structure into mise's expected layout.

## Development Environment

### Package Management
- Uses **uv** as primary package manager (evidenced by `uv.lock`)
- Supports both `uv tool install` and `pipx install` workflows
- `pyproject.toml` configured with hatchling build backend
- Version sourced from `__version__` variable in main script

### VS Code Configuration
- `.vscode/settings.json` shows mise integration for multiple language tools
- Python interpreter set to workspace `.venv` directory
- Zig tools configured to use mise shims

### Dependencies
- Minimal runtime dependency: only `requests>=2.32.3`
- Development dependency: `ipython>=8.31.0` for debugging/exploration
- Uses PEP 723 inline script dependencies for standalone execution

## Key Files for Understanding

- `zigup.py`: Complete application logic and entry point
- `pyproject.toml`: Package configuration and dependency specification  
- `README.md`: Usage patterns and installation methods
- `.vscode/settings.json`: Shows integration with mise toolchain management
