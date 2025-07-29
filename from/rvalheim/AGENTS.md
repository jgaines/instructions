# Valheim Manager Rust Conversion Project

## Overview
This document provides guidance for converting the existing Python Valheim Manager application to Rust using relm4 for the GUI. The original Python application is a comprehensive profile management system for Valheim that manages configurations, plugins, characters, and worlds as unified profiles.

## Version Control
This project uses Jujutsu for version control.

### Workflow
- After every prompt/change, run `jj status` to check for modifications
- If changes are detected, commit them with: `jj commit -m "description" --author "warp <warp@jgaines.com>"`
- Use descriptive commit messages that explain what was changed
- Follow conventional commit format when possible

## Original Python Application Analysis
The existing application in `steff/valheim_manager/` is a full-featured Valheim profile manager with:
- Profile management (create, edit, copy, delete, run)
- Mod integration with ThunderStore and Nexus Mods
- Character and world save management
- Configuration file management
- Cache management for downloaded mods
- GUI built with PySimpleGUI

### Key Components:
- **app.py**: Main application entry point with GUI event loop
- **profile.py**: Profile management and operations
- **cache.py**: Download cache and mod management
- **config.py**: Configuration management
- **model.py**: Database models (SQLite with Pony ORM)
- **edit_profile.py**: Profile editing interface
- Platform-specific modules for Linux/Windows

## Rust Conversion Strategy

### 1. Technology Stack
- **GUI Framework**: relm4 (modern GTK4-based Rust GUI framework)
- **Database**: SQLite with rusqlite or diesel
- **HTTP Client**: reqwest for API calls
- **File Operations**: std::fs and tokio for async file operations
- **Archive Handling**: zip, tar, 7zip crates
- **Configuration**: serde with toml/yaml
- **Logging**: tracing/log

### 2. Architecture Design

#### Core Components:
1. **Main Application** (`src/main.rs`, `src/app.rs`)
   - relm4 application setup
   - Main window and event handling
   - Application state management

2. **Profile Management** (`src/profile/`)
   - Profile struct and operations
   - Profile switching logic
   - Symlink management

3. **Mod Management** (`src/mods/`)
   - ThunderStore API integration
   - Nexus Mods integration
   - Download and cache management
   - Dependency resolution

4. **Database Layer** (`src/database/`)
   - SQLite integration
   - Models for packages, profiles, cache
   - Migration system

5. **Configuration** (`src/config/`)
   - Application settings
   - Path management
   - Cross-platform compatibility

6. **GUI Components** (`src/ui/`)
   - Main window
   - Profile editor
   - Settings dialog
   - Progress dialogs

### 3. Implementation Phases

#### Phase 1: Foundation
- Set up Rust project structure
- Implement basic configuration system
- Create database models and migrations
- Basic profile management (create, list, delete)

#### Phase 2: Core Functionality
- Profile switching with symlinks
- Basic GUI with relm4
- Profile creation and copying
- File system operations

#### Phase 3: Mod Integration
- ThunderStore API client
- Download management
- Cache system
- Package installation/removal

#### Phase 4: Advanced Features
- Nexus Mods integration
- Profile editor GUI
- Configuration file editing
- Progress tracking for long operations

#### Phase 5: Polish
- Error handling and recovery
- Cross-platform testing
- Performance optimization
- Documentation

### 4. Key Rust Patterns to Use

#### relm4 Application Structure:
```rust
// Main application model
#[derive(Debug)]
struct App {
    profiles: Vec<Profile>,
    selected_profile: Option<usize>,
    config: Config,
}

// Application messages
#[derive(Debug)]
enum AppMsg {
    LoadProfiles,
    SelectProfile(usize),
    RunProfile(String),
    EditProfile(String),
    // ... other messages
}
```

#### Async Operations:
```rust
// Use tokio for file operations and HTTP requests
async fn download_mod(&self, url: &str) -> Result<PathBuf, Error> {
    // Implementation
}
```

#### Error Handling:
```rust
// Use thiserror for custom error types
#[derive(Debug, thiserror::Error)]
enum ValheimManagerError {
    #[error("Profile not found: {0}")]
    ProfileNotFound(String),
    #[error("Database error: {0}")]
    Database(#[from] rusqlite::Error),
    // ... other errors
}
```

### 5. Data Migration Strategy
- Create migration scripts to convert existing SQLite database
- Preserve user profiles and cached mod data
- Configuration file conversion from Python format

### 6. Testing Strategy
- Unit tests for core logic
- Integration tests for database operations
- GUI testing with relm4 test utilities
- Cross-platform testing (Linux/Windows)

### 7. Development Guidelines

#### Code Organization:
```
src/
├── main.rs              # Application entry point
├── app.rs               # Main application logic
├── config/              # Configuration management
├── database/            # Database models and operations
├── profile/             # Profile management
├── mods/                # Mod management and APIs
├── ui/                  # GUI components
├── error.rs             # Error types
└── utils.rs             # Utility functions
```

#### Dependencies (Cargo.toml):
```toml
[dependencies]
relm4 = "0.8"
gtk4 = "0.8"
tokio = { version = "1.0", features = ["full"] }
rusqlite = { version = "0.30", features = ["bundled"] }
reqwest = { version = "0.11", features = ["json"] }
serde = { version = "1.0", features = ["derive"] }
toml = "0.8"
thiserror = "1.0"
tracing = "0.1"
zip = "0.6"
```

### 8. Cross-Platform Considerations
- Use `std::path::PathBuf` for all path operations
- Platform-specific symlink handling
- Steam path detection (Linux: ~/.steam, Windows: registry/common paths)
- Different file system permissions handling

### 9. Performance Considerations
- Lazy loading of mod lists
- Efficient file copying with hard links where possible
- Background processing for long operations
- Memory-efficient handling of large archive files

### 10. Migration from Python
- Keep the same directory structure for profiles
- Maintain compatibility with existing profile format
- Preserve all user data during conversion
- Provide migration tool/script

## Next Steps
1. Set up basic Rust project structure
2. Implement configuration and database layers
3. Create basic relm4 GUI skeleton
4. Port core profile management functionality
5. Add mod management features incrementally

## References
- [relm4 Documentation](https://relm4.org/)
- [GTK4 Rust Bindings](https://gtk-rs.org/)
- [Original Python Source](steff/valheim_manager/)
- [Product Requirements Document](steff/valheim_manager/docs/PRD.md)