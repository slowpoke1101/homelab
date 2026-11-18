# SSH Key Authentication Runbook

## Purpose

Configure SSH key authentication for administrative access from trusted devices (IZZY, LONS, etc.) and disable password-based SSH authentication.

---

# Generate SSH Key Pair

## Linux (Fedora, Debian, Ubuntu)

Generate an Ed25519 key:

```bash
ssh-keygen -t ed25519 -C "izzy"
```

Accept the default location:

```text
~/.ssh/id_ed25519
```

Files created:

```text
~/.ssh/id_ed25519      # Private Key
~/.ssh/id_ed25519.pub  # Public Key
```

View public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Example output:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... izzy
```

---

# SSH Directory and File Permissions

## Create SSH Directory

```bash
mkdir -p ~/.ssh
```

## Set Directory Permissions

```bash
chmod 700 ~/.ssh
```

Expected:

```text
drwx------
```

---

## Create authorized_keys

```bash
touch ~/.ssh/authorized_keys
```

## Set File Permissions

```bash
chmod 600 ~/.ssh/authorized_keys
```

Expected:

```text
-rw-------
```

---

# Add Public Keys

Edit:

```bash
nano ~/.ssh/authorized_keys
```

Paste one public key per line:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... izzy
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAJ... lons
```

Save and exit.

---

# Verify Permissions

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

Expected:

```text
drwx------ 2 user user
-rw------- 1 user user
```

---

# Test SSH Key Authentication

Connect from the client:

```bash
ssh user@server
```

Verify key usage:

```bash
ssh -v user@server
```

Look for:

```text
Offering public key:
```

and

```text
Authenticated using public key
```

---

# SSH Server Configuration

Edit:

```bash
sudo nano /etc/ssh/sshd_config
```

Recommended settings:

```text
PubkeyAuthentication yes

PasswordAuthentication no

KbdInteractiveAuthentication no

ChallengeResponseAuthentication no

PermitRootLogin prohibit-password

UsePAM yes
```

Notes:

- PubkeyAuthentication yes = allow keys
- PasswordAuthentication no = disable passwords
- KbdInteractiveAuthentication no = disable keyboard-interactive logins
- PermitRootLogin prohibit-password = root only via SSH key
- UsePAM yes = normal and recommended

---

# Check Included Configurations

Many modern systems load additional configuration files.

Check:

```bash
grep Include /etc/ssh/sshd_config
```

Common result:

```text
Include /etc/ssh/sshd_config.d/*.conf
```

Inspect overrides:

```bash
sudo ls -l /etc/ssh/sshd_config.d/
```

Search for authentication settings:

```bash
sudo grep -R authentication /etc/ssh/
```

---

# Validate Effective SSH Configuration

View the actual configuration sshd is using:

```bash
sudo sshd -T | grep -E "passwordauthentication|kbdinteractiveauthentication|pubkeyauthentication|usepam"
```

Expected:

```text
passwordauthentication no
kbdinteractiveauthentication no
pubkeyauthentication yes
usepam yes
```

---

# Validate Configuration Syntax

Always validate before restarting:

```bash
sudo sshd -t
```

No output indicates success.

---

# Restart SSH Service

## Fedora / RHEL / Rocky

```bash
sudo systemctl restart sshd
```

## Debian / Ubuntu

```bash
sudo systemctl restart ssh
```

or

```bash
sudo systemctl restart sshd
```

---

# Verify Service Status

```bash
systemctl status sshd
```

or

```bash
systemctl status ssh
```

---

# Test Password Authentication Is Disabled

Force password authentication:

```bash
ssh \
-o PreferredAuthentications=password \
-o PubkeyAuthentication=no \
user@server
```

Expected:

```text
Permission denied (publickey).
```

---

# Recovery Safety Procedure

Before disabling passwords:

1. Keep existing SSH session open.
2. Open a second terminal.
3. Test SSH key login.
4. Verify access works.
5. Restart SSH.
6. Test again.
7. Only then close the original session.

This prevents accidental lockout.

---

# Common Locations

## Normal User

```text
/home/<user>/.ssh/
```

## Root User

```text
/root/.ssh/
```

Files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
~/.ssh/authorized_keys
```

---

# Quick 
