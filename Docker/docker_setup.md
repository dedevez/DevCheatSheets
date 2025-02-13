# Docker Setup Guide

## Step 1: Update System Packages
### Ubuntu/Debian:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

### CentOS/RHEL:
```bash
sudo yum update -y
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### macOS:
Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Windows:
Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop)

---

## Step 2: Install Docker
### Ubuntu/Debian:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
sudo add-apt-repository "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

### CentOS/RHEL:
```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### macOS & Windows:
Docker Desktop includes Docker and Docker Compose automatically.

---

## Step 3: Start and Enable Docker
### Linux:
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## Step 4: Verify Docker Installation
```bash
docker --version
sudo docker run hello-world
```

---

## Step 5: Add a Non-Root User to the Docker Group
```bash
sudo usermod -aG docker $USER
newgrp docker  # Apply changes without logout
```
**OR**, for another user:
```bash
sudo usermod -aG docker username
```

Log out and back in, then verify:
```bash
docker run hello-world
```

---

## Step 6: Install Docker Compose (Optional)
### Linux:
```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### macOS & Windows:
Docker Compose is included with Docker Desktop.

---

## Step 7: Configure Docker to Start on Boot (Optional)
```bash
sudo systemctl enable docker
```

---

## Step 8: Enable Docker Remote API (Optional)
Edit Docker service file:
```bash
sudo nano /lib/systemd/system/docker.service
```
Modify the `ExecStart` line:
```ini
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375
```
Restart Docker service:
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---

## Troubleshooting
- **Permission Denied on Docker Commands:** Ensure your user is in the `docker` group (`groups $USER`).
- **Docker Daemon Not Running:** Restart Docker: `sudo systemctl restart docker`.
- **Pull Access Denied for Private Repos:** Ensure you’re logged into DockerHub: `docker login`.
- **Windows WSL Issue:** Enable WSL 2 and restart Docker Desktop.

---

**Docker is now set up on your system! 🚀**
