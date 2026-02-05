# Homebrew Tap for Gemkeeper

[Gemkeeper][1] is an opinionated wrapper around [Gem in a Box][2] for managing private gem dependencies in offline development environments.
It automates cloning, building, and serving internal gems from a local Geminabox server, alongside proxied public gems from RubyGems.org, so you can develop Rails apps without VPN access.


## Installation

```bash
brew tap danhorst/gemkeeper
brew install gemkeeper
```


## Quick Start

1. Create a configuration file:

```bash
mkdir -p ~/.config/gemkeeper
cat > ~/.config/gemkeeper/config.yml << 'EOF'
port: 9292

gems:
  - repo: git@github.com:company/internal-gem.git
    version: latest
EOF
```

2. Start the server and sync your gems:

```bash
brew services start gemkeeper
gemkeeper sync
```

3. Point your Gemfile at the local server:

```ruby
source "http://localhost:9292" do
  gem "internal-gem"
end
```


## Running the Server


### As a Service (recommended)

```bash
brew services start gemkeeper
brew services stop gemkeeper
brew services info gemkeeper
```

### Manually

```bash
gemkeeper server start              # Daemonized
gemkeeper server start --foreground  # Foreground (useful for debugging)
gemkeeper server stop
gemkeeper server status
```

## Managing Gems

```bash
gemkeeper sync                # Sync all configured gems
gemkeeper sync internal-gem   # Sync a specific gem
gemkeeper list                # List cached gems
```

## Configuration

Gemkeeper searches for a config file in the following order:

1. `./gemkeeper.yml`
2. `~/.config/gemkeeper/config.yml`
3. `~/.gemkeeper.yml`
4. `/usr/local/etc/gemkeeper.yml` (Homebrew on Intel)
5. `/opt/homebrew/etc/gemkeeper.yml` (Homebrew on Apple Silicon)

### Options

```yaml
# Port for the Geminabox server (default: 9292)
port: 9292

# Where to clone gem repositories (default: ./cache/repos)
repos_path: /opt/homebrew/var/gemkeeper/repos

# Where to store built gems (default: ./cache/gems)
gems_path: /opt/homebrew/var/gemkeeper/gems

# PID file location (default: ./cache/gemkeeper.pid)
pid_file: /opt/homebrew/var/gemkeeper/gemkeeper.pid

# List of gems to manage
gems:
  - repo: git@github.com:company/internal-gem.git
    version: latest           # Latest commit on main/master

  - repo: git@github.com:company/another-gem.git
    version: v1.2.3           # Specific tag or branch

  - repo: git@github.com:company/ruby-gem-three.git
    name: gem-three           # Override the gem name
```

### Gem Definition Fields

| Field     | Required | Default    | Description                                                        |
|-----------|----------|------------|--------------------------------------------------------------------|
| `repo`    | yes      |            | Git repository URL                                                 |
| `version` | no       | `"latest"` | `"latest"` for trunk, or a specific tag/branch                     |
| `name`    | no       |            | Override gem name (automatically strips `ruby-` prefix if omitted) |


## Updating

```bash
brew update
brew upgrade gemkeeper
```


## More Information

See the [gemkeeper repository][1] for full documentation, development instructions, and issue tracking.

[1]: https://github.com/danhorst/gemkeeper
[2]: https://github.com/geminabox/geminabox
