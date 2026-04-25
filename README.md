# Fedora Workstation Setup

Ansible playbook to provision a [Fedora workstation](https://fedoraproject.org/workstation/). Runs against localhost.

## Fresh Fedora install

```bash
sudo dnf upgrade -y
sudo dnf install -y git ansible
ansible-galaxy collection install community.general
git clone https://github.com/antonpirker/fedora-workstation-setup.git
cd fedora-workstation-setup
ansible-playbook -i inventory playbook.yml --ask-become-pass
```

## Usage

```bash
ansible-playbook -i inventory playbook.yml --ask-become-pass
```

## Roles

### `base`
Installs base packages via `dnf`: `git`, `gh`, `curl`, `wget`, `htop`, `vim`, `direnv`.

### `docker`
Installs Docker via `dnf`, enables and starts the `docker` systemd service, and adds the current user to the `docker` group.

### `python`
Installs `uv` (the Python package manager) to `~/.local/bin`, installs `ruff` via `uv tool`, and installs the configured Python version (default: `3.14`) via `uv python install`. The Python version is set in `roles/python/defaults/main.yml`.

### `node`
Installs `fnm` (Fast Node Manager) and uses it to install and set the latest LTS Node.js as default. Adds `fnm` to `~/.bashrc`.

### `claude_code`
Installs Claude Code CLI via the official install script. On subsequent runs, if Claude is already installed it runs `claude update` instead of reinstalling.

### `wifi_suspend`
Fixes WiFi connectivity issues after suspend/resume by configuring the system to use the `s2idle` (shallow) sleep state instead of deep S3 sleep. Drops a config file at `/etc/systemd/sleep.conf.d/s2idle.conf`.

### `graphics`
Installs `gimp` and `inkscape` via `dnf`.

### `obsidian`
Installs Obsidian via Flatpak from Flathub (`md.obsidian.Obsidian`). Adds Flathub as a Flatpak remote if not already present.
