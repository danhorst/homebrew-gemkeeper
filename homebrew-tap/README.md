# Homebrew Tap for Gemkeeper

This tap contains the Homebrew formula for [Gemkeeper](https://github.com/danhorst/gemkeeper).

## Installation

```bash
brew tap danhorst/gemkeeper
brew install gemkeeper
```

## Usage

### As a service (recommended)

```bash
# Start the service
brew services start gemkeeper

# Stop the service
brew services stop gemkeeper

# Check status
brew services info gemkeeper
```

### Manual usage

```bash
# Start in background
gemkeeper server start

# Start in foreground
gemkeeper server start --foreground

# Stop
gemkeeper server stop

# Check status
gemkeeper server status
```

## Configuration

Create a config file at one of these locations:
- `/opt/homebrew/etc/gemkeeper.yml` (Apple Silicon)
- `/usr/local/etc/gemkeeper.yml` (Intel)
- `~/.config/gemkeeper/config.yml`
- `~/.gemkeeper.yml`

Example configuration:

```yaml
port: 9292
repos_path: /opt/homebrew/var/gemkeeper/repos
gems_path: /opt/homebrew/var/gemkeeper/gems

gems:
  - repo: git@github.com:company/internal-gem.git
    version: latest
  - repo: git@github.com:company/another-gem.git
    version: v1.2.3
```

## Updating

```bash
brew update
brew upgrade gemkeeper
```
