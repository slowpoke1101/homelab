# Sandbox VS Code VM

## Purpose

I wanted a safe place to use VS Code, GitHub, and AI tools without putting my real homelab at risk. This VM is named `sandbox-vscode` and it lives in the `Sandbox` VLAN so I can test ideas, write docs, and make GitHub changes without touching my production network.

This is not a production system. It is a disposable working environment for documentation, experiments, and low-risk changes.

## Scope

This runbook covers:

- Proxmox VM creation for a secure documentation workstation
- VLAN isolation for the sandbox environment
- Linux installation and hardening
- Visual Studio Code and GitHub tool installation
- Safe Git and GitHub workflow for a multi-machine homelab repo
- Snapshot and rollback procedures before experimenting with AI tools or documentation changes

This does not replace the primary workstation or any production change process. It is a controlled, disposable environment for risky or exploratory work.

## Prerequisites

- A Proxmox host available, such as Sora
- A working homelab VLAN plan with `Sandbox` on VLAN 50
- A Linux ISO, such as Mint XFCE or Ubuntu LTS
- A GitHub account with access to the repo
- A safe place to store real credentials or passphrases if I need them
- A plan to roll back the VM if something breaks

## Expected Impact

The `sandbox-vscode` VM will be separated from the production network, but it will still be able to reach the internet for package updates and GitHub access. I should not trust it with real secrets or live infrastructure credentials. It is just a sandbox for documentation and experiments.

## Procedure

### 1. Create the Proxmox VM

Create a new VM in Proxmox with the following settings:

- Name: `sandbox-vscode`
- Node: choose the Proxmox host that manages the sandbox environment
- OS: Linux
- BIOS: OVMF if using modern guest support
- Machine: `q35`
- SCSI controller: `VirtIO SCSI`
- CPU: 2 cores
- Type: `host`
- Memory: 4 to 6 GiB
- Disk: 40 GiB, VirtIO disk
- Network: VirtIO NIC
- Bridge: `vmbr0`
- VLAN: `50` for the sandbox network
- Firewall: enabled

If the environment uses a dedicated network plan, place the VM on VLAN 50 and keep it separated from the main LAN and management networks.

### 2. Install Mint XFCE or Ubuntu

Install the chosen Linux distribution using the VM console. Use defaults for the initial install but keep the system simple and consistent.

Recommended setup:

- User: `sandbox`
- Password: strong and unique
- Package manager: default system package repository
- Desktop environment: XFCE for light weight performance

### 3. Update the guest and install core tools

After the guest is installed, update the system and install the tools needed for editing and Git operations:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git gh curl wget gpg ca-certificates qemu-guest-agent spice-vdagent
```

Install Visual Studio Code:

```bash
sudo apt install wget gpg -y
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update && sudo apt install code -y
```

### 4. Configure Git and GitHub access

Set the identity for Git in the VM:

```bash
git config --global user.name "Marco"
git config --global user.email "your_email@example.com"
```

Generate an SSH key for GitHub if you want passwordless access:

```bash
ssh-keygen -t ed25519 -C "sandbox-vscode"
cat ~/.ssh/id_ed25519.pub
```

Add the public key to GitHub under Settings → SSH and GPG keys.

### 5. Clone the homelab repo

Clone the repository into the VM from GitHub:

```bash
mkdir -p ~/Documents
cd ~/Documents
git clone git@github.com:YOUR_USERNAME/YOUR_REPO.git
```

If the repo is privately hosted, use the SSH URL. If using HTTPS, authenticate through GitHub with a token or browser login when prompted.

### 6. Keep the repo in a safe Git workflow

Use the GitHub repo as the source of truth. Treat the sandbox VM as a secure working area for AI-assisted documentation and low-risk changes.

The key workflow is:

```bash
cd ~/Documents/YOUR_REPO
# edit files in VS Code

git status
git add .
git commit -m "Add sandbox documentation update"
git push
```

Then, on the main workstation or other machine, sync with:

```bash
git pull
```

### 7. Use the sandbox VM for AI-assisted edits, not for production secrets

This VM is for:

- writing docs and runbooks
- testing ideas before I apply them to the real environment
- using AI to clean up notes and draft changes
- preparing commits and GitHub updates

This VM is not for:

- storing real credentials
- keeping sensitive tokens or private keys in plain text
- doing live admin work on production systems
- editing important configs without review

If something is important, it stays on the trusted workstation or in a proper secret manager.

### 8. Create a snapshot before risky experiments

Before heavy AI use, large documentation changes, or testing risky network workflows, take a Proxmox snapshot of the VM.

This allows a quick rollback if the VM becomes unstable or if a tool starts acting unexpectedly.

### 9. Network isolation and firewall expectations

The important part is that this VM stays isolated. It should live in VLAN 50 and not have access to my production or management networks. If the Proxmox firewall is enabled, I should allow only the minimum traffic needed:

- outbound HTTPS to GitHub and package repos
- outbound DNS if needed
- outbound NTP if needed
- no access to production VLANs unless I intentionally allow it

This is safer than assuming the VM will just behave itself.

## Validation

After setup, confirm the following:

1. The VM reports an IP on the correct sandbox network.
2. The GitHub repo can be cloned and pushed from within the VM.
3. Visual Studio Code opens the repository normally.
4. Git status, add, commit, and push work correctly.
5. The VM can reach package repositories and GitHub without needing a direct link to privileged networks.
6. A snapshot is saved before any AI-assisted experimentation begins.

Useful checks:

```bash
ip addr
ip route
ping 1.1.1.1
git remote -v
git status
```

## Rollback And Recovery

If the VM becomes unstable, carries accidental changes, or is no longer trusted:

```bash
# In Proxmox, revert the VM snapshot from the web UI
```

If a rollback is not available, rebuild the VM from the sanitized template or recreate a fresh Mint instance and re-clone the GitHub repo.

For a repo-level recovery, use Git history and remotes to recover previous commits:

```bash
git log --oneline
git checkout <previous-commit>
git revert <bad-commit>
```

## Working Rules

- Keep the repo on GitHub as the source of truth.
- Use the sandbox VM for controlled documentation and experiments.
- Push before switching to a different machine.
- Pull before editing the same repo on a different host.
- Keep the VM in VLAN 50.
- Take a snapshot before I do anything risky.
- Do not put production secrets in the sandbox environment.

## Related Documentation

- [Runbooks and Projects overview](README.md)
- [Encrypted backup drive for cold storage](encrypted-backup-drive-cold-storage.md)
- [Architecture Overview](../Architecture%20Overview/README.md)
- [Security Architecture](../Architecture%20Overview/security-architecture.md)
