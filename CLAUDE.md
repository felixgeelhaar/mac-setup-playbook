# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a comprehensive Ansible playbook for complete Mac setup automation. It sets up a full development environment including applications, system preferences, development tools, security configurations, and validates the installation. Built using roles from @geerlingguy and @elliotweiser, extended with custom automation tasks.

## Common Commands

### Running the Playbook
```bash
# Install required Ansible roles and collections first
ansible-galaxy install -r requirements.yml

# Run the complete setup playbook
ansible-playbook main.yml -i inventory

# Dry run to see what would be changed
ansible-playbook main.yml -i inventory --check

# Run with verbose output
ansible-playbook main.yml -i inventory -v

# Run specific parts by tags (if implemented)
ansible-playbook main.yml -i inventory --tags homebrew
```

### Linting and Validation
```bash
# Lint Ansible playbook
ansible-lint

# Lint YAML files
yamllint .

# Check playbook syntax
ansible-playbook main.yml --syntax-check
```

## Architecture

### Core Structure
- `main.yml`: Main playbook orchestrating the entire Mac setup process
- `inventory`: Defines localhost as the target machine (127.0.0.1)
- `ansible.cfg`: Ansible configuration with local connection settings
- `requirements.yml`: External Ansible collections and roles dependencies

### Configuration Variables
All configuration is defined in `vars/` directory (all properly formatted with YAML document start markers):
- `homebrew.yml`: Homebrew packages and cask applications (organized by category)
- `dotfiles.yml`: Repository configuration for personal dotfiles
- `mas.yml`: Mac App Store applications with correct App Store IDs
- `dock.yml`: Dock configuration and app positioning

### Key Roles and Collections
- **geerlingguy.mac collection**: Contains homebrew, mas, and dock roles
- `elliotweiser.osx-command-line-tools`: Installs Xcode command line tools
- `geerlingguy.dotfiles`: Clones and manages dotfiles repository

### Custom Task Files
- `tasks/dotfiles.yml`: GNU Stow integration for dotfiles symlinking
- `tasks/macos-preferences.yml`: Complete macOS system preferences automation
- `tasks/dev-environments.yml`: Language-specific development environment setup
- `tasks/ssh-gpg-setup.yml`: SSH/GPG key generation and configuration
- `tasks/validation.yml`: Post-installation validation and verification

## Enhanced Features

### System Preferences Automation
- Finder optimizations (hidden files, extensions, path bar, status bar)
- Trackpad settings (tap to click, three-finger drag)
- Keyboard settings (fast repeat, disable auto-correct)
- Dock configuration (auto-hide, sizing, animations)
- Security settings (password requirements, screensaver)
- Performance optimizations (disable window animations)

### Development Environment Setup
- **Node.js**: NVM setup + global packages (TypeScript, ESLint, Prettier, etc.)
- **Python**: Virtual environments + dev tools (Black, Pytest, Jupyter)
- **Go**: Workspace setup + essential tools (goimports, golint)
- **Rust**: Components (clippy, rustfmt) + cargo tools
- **PHP**: Composer + quality tools (PHPUnit, PHP-CS-Fixer)
- **Docker**: Development compose file with PostgreSQL, Redis, MongoDB
- **Git**: Optimized global configuration

### Security and Authentication
- **SSH**: Ed25519 key generation with proper agent configuration
- **GPG**: Key generation with agent setup for commit signing
- **GitHub CLI**: Ready for authentication setup

### Expanded Software Collection
The Homebrew configuration now includes:
- Modern CLI tools (bat, eza, fd, fzf, starship, git-delta)
- Database tools (PostgreSQL, Redis, SQLite)
- DevOps tools (Kubernetes, Terraform, Helm)
- Security tools (age, sops, Little Snitch)
- Productivity apps (Rectangle, Alfred, Raycast, Notion)
- Creative tools (Affinity Suite)

## Configuration Patterns

### Adding New Software
- CLI tools: Add to appropriate category in `homebrew_installed_packages` in `vars/homebrew.yml`
- GUI applications: Add to appropriate category in `homebrew_cask_apps` in `vars/homebrew.yml`
- Mac App Store apps: Add to `mas_installed_apps` in `vars/mas.yml` with correct App Store ID

### System Preferences
Modify `tasks/macos-preferences.yml` using `community.general.osx_defaults` module for macOS defaults.

### Development Environments
Customize `tasks/dev-environments.yml` for language-specific configurations and global package installations.

### Security Configuration
Modify `tasks/ssh-gpg-setup.yml` for SSH/GPG key management and security tool setup.

### Dock Management
- Remove unwanted apps: Add to `dockitems_remove` in `vars/dock.yml`
- Add/position apps: Define in `dockitems_persist` with name, path, and position

### Dotfiles Integration
The playbook clones the dotfiles repository from `https://github.com/felixgeelhaar/dotfiles` and uses GNU Stow to create symlinks for configuration files.

## Validation and Testing

The playbook includes comprehensive validation tasks that check:
- All CLI tools are properly installed and accessible
- GUI applications are present in /Applications
- SSH/GPG keys are generated
- Development environments are configured
- Dotfiles repository is cloned

## Important Notes

### Modern Ansible Collection Usage
This playbook now uses the `geerlingguy.mac` collection instead of individual roles, which is the current recommended approach.

### YAML Standards
All YAML files include proper document start markers (`---`) for linting compliance.

### Idempotency
All tasks are designed to be idempotent - safe to run multiple times without causing issues.

## Lint Configuration
- `.ansible-lint`: Skips experimental and fqcn-builtins rules
- `.yamllint.yml`: Extends default rules with 180 character line length limit