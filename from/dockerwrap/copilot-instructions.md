# Copilot Instructions for dockerwrap

## Project Architecture

This is a Docker Compose wrapper toolkit that solves two key problems:

1. **DNS Resolution**: Replace hostnames in port mappings with IP addresses (`dc-preprocess.py`)
2. **Global Dockerignore**: Inject global `.dockerignore` patterns into builds (`docker-build-with-global.sh`)
3. **Combined Solution**: Merge both features into a unified `dc-wrap` script (specified in `dc-wrap-spec.md`)

## Key Files & Patterns

### `dc-preprocess.py` - DNS Resolution Wrapper
- Uses `uv --quiet run --script` shebang for dependency management
- Implements DNS caching with `@lru_cache` decorator
- Fork/exec pattern with stdin injection to pass modified YAML to `docker compose`
- Auto-discovers compose files: `compose.{yaml,yml}`, `docker-compose.{yaml,yml}`, plus `.override` variants
- Handles both short syntax (`hostname:8080:80`) and long syntax (`host_ip: hostname`) port mappings

### `docker-build-with-global.sh` - Global Dockerignore Injection
- Merges `~/.config/docker/global.dockerignore` with local `.dockerignore`
- Uses temporary files with backup/restore pattern for safety
- Applies `DOCKER_BUILDKIT=1` for enhanced build features

### Development Workflow
- **Python Environment**: Uses Python 3.13+ with `uv` for dependency management
- **Project Structure**: Single-file scripts with inline dependency declarations
- **Error Handling**: Graceful degradation (keep original values on failure)
- **Logging**: INFO level for operations, WARNING for failures

## Critical Patterns to Follow

### Script Structure
```python
#!/usr/bin/env -S uv --quiet run --script
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "pyyaml",
# ]
# ///
```

### DNS Resolution with Caching
```python
@lru_cache
def hostname_to_ip(hostname: str) -> str:
    # Always handle gaierror gracefully
    # Log resolutions for debugging
```

### File Safety Pattern
```bash
# Always backup before modifying
if [ -f "$ORIG_FILE" ]; then
    mv "$ORIG_FILE" "$ORIG_FILE.bak"
fi
# ... do work ...
# Always restore on cleanup
if [ -f "$ORIG_FILE.bak" ]; then
    mv "$ORIG_FILE.bak" "$ORIG_FILE"
fi
```

### Process Replacement Pattern
```python
# Fork/exec to replace current process
r, w = os.pipe()
if os.fork() == 0:
    # Child: setup stdin and exec
    os.dup2(r, sys.stdin.fileno())
    os.execlp("docker", *args)
else:
    # Parent: write data and wait
    os.write(w, data.encode())
    os.wait()
```

## Future Implementation Notes

The `dc-wrap-spec.md` outlines a unified script that:
- Uses **temporary files** instead of stdin injection for better reliability
- Detects build contexts from compose files to apply dockerignore processing
- Implements signal handling for proper cleanup
- Supports environment variables for feature toggles (`DC_WRAP_NO_DNS`, `DC_WRAP_NO_GLOBAL_IGNORE`)

## Testing Considerations

- Test with various compose file formats and Docker Compose v1/v2
- Verify cleanup on script interruption (SIGINT/SIGTERM)
- Test DNS resolution failures and dockerignore missing scenarios
- Validate temporary file cleanup and original file restoration

## Version Control

If found read all instructions in `.github/copilot-instructions-local.md` for version control practices.