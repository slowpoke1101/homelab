# Install Docker Engine

## Purpose

Install Docker Engine and the Docker Compose plugin on a Debian or Ubuntu VM.

## Prerequisites

- A supported Debian-based Linux system with sudo access.
- Network access to the Docker package repository.
- A non-root administrative user.

## Procedure

### 1. Install prerequisites
```
sudo apt update
sudo apt install ca-certificates curl gnupg
```

### 2. Add Docker's repository key
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 3. Add the Docker repository
```
cat /etc/os-release
# Use the VERSION_CODENAME value for <distribution-codename> below.
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/<ADD VERSION CODENAME HERE> stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Install and enable Docker
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
```

### 5. Optional: allow the user to run Docker without sudo
```
sudo usermod -aG docker <username>
# Log out and back in for the group change to take effect.
```

Membership in the `docker` group grants root-equivalent control over the host. Only add
trusted administrative users.

## Validation

```bash
docker version
docker compose version
sudo systemctl is-active docker
sudo docker run --rm hello-world
```

## Recovery

If the installation fails, inspect the package and service state before retrying:

```bash
apt policy docker-ce
sudo systemctl status docker
sudo journalctl -u docker --no-pager -n 50
```