# Mac Setup Playbook

This Ansible playbook automatically sets up a complete macOS development environment with applications, system preferences, development tools, and security configurations.

Built on the excellent work of [@geerlingguy](https://github.com/geerlingguy) and [@elliotweiser](https://github.com/elliotweiser).

## 🚀 Quick Start

### Prerequisites

1. Install Xcode Command Line Tools:
   ```bash
   xcode-select --install
   ```

2. Install Ansible:
   ```bash
   pip3 install ansible
   ```

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/felixgeelhaar/mac-setup-playbook.git
   cd mac-setup-playbook
   ```

2. Install required Ansible dependencies:
   ```bash
   ansible-galaxy install -r requirements.yml
   ```

3. Run the playbook:
   ```bash
   ansible-playbook main.yml -i inventory
   ```

## 📦 What Gets Installed

### Applications via Homebrew
- **Development**: VS Code, Docker, Postman, Git tools
- **Browsers**: Chrome, Firefox, Arc
- **Communication**: Discord, Slack, Zoom, Telegram  
- **Productivity**: Obsidian, Notion, Rectangle, Alfred/Raycast
- **Creative**: Affinity Suite (Designer, Photo, Publisher)
- **Security**: 1Password, Little Snitch
- **Utilities**: CleanMyMac, The Unarchiver, AppCleaner

### Development Tools & Languages
- **Languages**: Node.js (via NVM), Python, Go, Rust, PHP, Lua
- **Databases**: PostgreSQL, Redis, SQLite
- **DevOps**: Docker, Kubernetes, Terraform, Helm
- **CLI Tools**: Modern shell tools (bat, eza, fd, ripgrep, fzf)

### Mac App Store Apps
- Xcode
- 1Password for Safari
- Pages, Keynote, Numbers

### System Preferences
- Finder optimizations (show hidden files, extensions, path bar)
- Trackpad settings (tap to click, three-finger drag)
- Keyboard settings (fast key repeat, disable auto-correct)
- Dock configuration (auto-hide, optimal sizing)
- Security settings (require password, immediate lock)
- Performance optimizations (disable animations)

### Development Environment Setup
- **Node.js**: Latest LTS via NVM + essential packages (TypeScript, ESLint, Prettier)
- **Python**: Virtual environments + development packages (Black, Pytest, Jupyter)
- **Go**: Workspace setup + essential tools (goimports, golint)
- **Rust**: Components (clippy, rustfmt) + cargo tools
- **PHP**: Composer + quality tools (PHPUnit, PHP-CS-Fixer)
- **Git**: Optimized global configuration
- **Docker**: Development compose file with common services

### Security & Keys
- **SSH**: Ed25519 key generation + agent configuration
- **GPG**: Key generation + agent setup for signing commits
- **GitHub CLI**: Ready for authentication

## 🛠 Customization

### Modifying Software Lists
Edit the respective files in the `vars/` directory:
- `homebrew.yml`: CLI tools and GUI applications
- `mas.yml`: Mac App Store applications  
- `dock.yml`: Dock configuration and app positioning

### System Preferences
Modify `tasks/macos-preferences.yml` to adjust system settings.

### Development Environments
Customize `tasks/dev-environments.yml` for language-specific configurations.

## 🔧 Advanced Usage

### Run Specific Parts
```bash
# Only install Homebrew packages
ansible-playbook main.yml -i inventory --tags homebrew

# Only configure system preferences  
ansible-playbook main.yml -i inventory --tags macos-preferences
```

### Dry Run
```bash
ansible-playbook main.yml -i inventory --check
```

### Verbose Output
```bash
ansible-playbook main.yml -i inventory -v
```

## 📋 Post-Installation Steps

After running the playbook, complete these manual steps:

1. **GitHub Integration**:
   ```bash
   gh auth login
   ```

2. **Add SSH/GPG keys to GitHub/GitLab** (keys are displayed during setup)

3. **Configure Git identity**:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

4. **Sign in to applications**:
   - 1Password, Slack, Discord, etc.

5. **Verify development environments**:
   ```bash
   node --version
   python3 --version
   go version
   rustc --version
   ```

## 🧪 Testing

The playbook includes validation tasks that verify:
- All tools are properly installed
- Development environments are configured
- SSH/GPG keys are generated
- Applications are available

## 🔍 Troubleshooting

### Common Issues

**Homebrew installation fails**: Ensure Xcode Command Line Tools are installed first.

**Permission errors**: Some tasks require sudo privileges, ensure your user can use `sudo`.

**App Store login required**: For MAS apps, you may need to sign in to the App Store first.

**NVM/Node issues**: Restart your terminal or source your shell profile after installation.

### Getting Help

- Check the Ansible output for specific error messages
- Ensure all prerequisites are installed
- For Homebrew issues, run `brew doctor`
- For role-specific issues, check the upstream documentation

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Jeff Geerling](https://github.com/geerlingguy) - Mac automation roles
- [Elliot Weiser](https://github.com/elliotweiser) - macOS Command Line Tools role
