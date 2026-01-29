#  Automated Programs Installation for Developers

## Why does this project exist?

As a software developer, you've probably faced this situation: **every time you need to format your operating system, switch computers, or set up a new development environment, you have to spend hours (or days?) installing and configuring all the tools you use daily**.

This manual process is:
- **Extremely slow** - It can take 4-8 hours of installation and configuration
- **Tedious and repetitive** - Always the same steps over and over again
- **Error-prone** - It's easy to forget to configure something important
- **Hard to document** - Do you remember exactly what you configured last time?

**This project was born to solve exactly that problem!** 

I applied my **DevOps** knowledge to create an automated solution using **Ansible** that installs and configures my entire development environment in minutes instead of hours.

##  What does it do exactly?

This project automates the complete installation of:

- **Development tools** (IDEs, editors, terminals)
- **Programming languages** (Python, Node.js, Java, etc.)
- **Package managers** (npm, pip, yarn, etc.)
- **Build tools** (Docker, git, make, etc.)
- **Common applications** (browsers, communication, multimedia)
- **Custom configurations** (dotfiles, aliases, shortcuts)

All of this with a single command and no manual intervention.

##  Project Structure

```
automated-programs-installation/
├──  playbooks/                           # Ansible playbooks
│   ├── install_common_host_apps.yaml       # Common system applications
│   ├── install_developer_tools.yaml        # IDEs and development tools
│   ├── install_programming_languages.yaml  # Languages and their ecosystems
│   └── install_software_development_packages.yaml # Specific development packages
│
├──  roles/                               # Reusable Ansible roles
│   ├── common/                             # Base system configurations
│   ├── developer_tools/                    # IDEs and editors installation
│   ├── programming_languages/              # Language installation
│   └── software_development_packages/      # Packages and libraries
│
├──  group_vars/                          # Variables by host groups
├──  host_vars/                           # Host-specific variables
├──  .hosts                               # Ansible hosts inventory
├──  ansible.cfg                          # Ansible configuration
```

##  Quick Start

### Prerequisites

```bash
# On Ubuntu/Debian
sudo apt update
sudo apt install ansible git

# On Fedora
sudo dnf install ansible git

# On Arch Linux
sudo pacman -S ansible git
```

### Installation and Usage

1. **Clone this repository:**
```bash
git clone <REPO_URL>
cd automated-programs-installation
```

2. **Run the complete installation:**
```bash
# This installs everything (common apps, programming languages, dev packages, and dev tools)
ansible-playbook playbooks/install_common_host_apps.yaml
```

##  Available Playbooks

### `install_common_host_apps.yaml`
Installs essential operating system applications:
- Web browsers (Firefox, Chrome)
- Communication tools (Discord, Telegram, Slack)
- Multimedia players (VLC, Spotify)
- System utilities

**Note:** This playbook automatically imports and runs all other playbooks, installing the complete development environment.

### `install_developer_tools.yaml`
Sets up the development environment:
- IDEs (Visual Studio Code, IntelliJ IDEA, PyCharm)
- Editors (Neovim, Sublime Text)
- Advanced terminals (Terminator, Alacritty)
- Version control tools (Git with configuration)

### `install_programming_languages.yaml`
Installs languages and their ecosystems:
- Node.js (with npm, yarn, nvm)
- Go

### `install_software_development_packages.yaml`
Installs specific development packages:
- Docker, Docker Compose and Podman
- Database tools (PostgreSQL, MySQL clients)


## Modular Installation

### Understanding the Playbook Chain

The playbooks are designed with a chain of dependencies:

1. `install_common_host_apps.yaml` → imports `install_programming_languages.yaml`
2. `install_programming_languages.yaml` → imports `install_software_development_packages.yaml`  
3. `install_software_development_packages.yaml` → imports `install_developer_tools.yaml`

This means:
- Running `install_common_host_apps.yaml` installs **everything**
- Running `install_programming_languages.yaml` installs programming languages + dev packages + dev tools
- Running `install_software_development_packages.yaml` installs dev packages + dev tools
- Running `install_developer_tools.yaml` installs only dev tools

### Running Individual Components

To run only specific components without their dependencies:

1. **For common apps only:**
   - Edit `playbooks/install_common_host_apps.yaml`
   - Remove the line: `- import_playbook: install_programming_languages.yaml`
   - Run: `ansible-playbook playbooks/install_common_host_apps.yaml`

2. **For programming languages only:**
   - Edit `playbooks/install_programming_languages.yaml`
   - Remove the line: `- import_playbook: install_software_development_packages.yaml`
   - Run: `ansible-playbook playbooks/install_programming_languages.yaml`

3. **For dev packages only:**
   - Edit `playbooks/install_software_development_packages.yaml`
   - Remove the line: `- import_playbook: install_developer_tools.yaml`
   - Run: `ansible-playbook playbooks/install_software_development_packages.yaml`

4. **For dev tools only:**
   - Run directly: `ansible-playbook playbooks/install_developer_tools.yaml`

This modular approach gives you complete control over what gets installed.

## Customization

### Configuration Variables

You can customize what gets installed by editing files in `group_vars/` and `host_vars/`:

```yaml
# group_vars/all.yaml
install_vscode: true
install_intellij: false
python_version: "3.11"
node_version: "18.17.0"
```

### Adding New Programs

1. Edit the corresponding role in `roles/`
2. Add the installation tasks
3. Define necessary variables
4. Run the playbook

## Troubleshooting

### Common Issues

**Permission errors:**
```bash
# Make sure your user is in sudoers
sudo usermod -aG sudo $USER
```

**Ansible can't find inventory:**
```bash
# Verify you're in the correct directory
pwd # Should show: .../automated-programs-installation
```

**Connection failures:**
```bash
# Check hosts configuration
ansible-inventory --list
```

## Contributing

Contributions are welcome! If you have additional tools you'd like to automate:

1. Fork the project
2. Create a feature branch (`git checkout -b feature/new-tool`)
3. Add your changes
4. Commit (`git commit -m 'Add: New development tool'`)
5. Push (`git push origin feature/new-tool`)
6. Open a Pull Request


## License

This project is under the MIT License. See `LICENSE` for more details.

## Author

Created with ❤️ by a developer tired of wasting time setting up development environments.

---

**Does this project save you time?** ⭐ Give the repo a star and share it with other developers!

**Found a bug?**  Open an issue and we'll fix it together.

**Have an idea to improve it?** 💡 Suggestions are always welcome!
