# Docker Setup on a DigitalOcean Droplet

## Step 1: Update and Install Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

## Step 2: Add Docker’s Official GPG Key and Repository
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
sudo add-apt-repository "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

## Step 3: Install Docker
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

## Step 4: Start and Enable Docker Service
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## Step 5: Verify Docker Installation
```bash
docker --version
sudo docker run hello-world
```

## Step 6: Add a Non-Root User to the Docker Group
```bash
sudo usermod -aG docker $USER
newgrp docker  # Apply changes without logout
```
**OR**, if setting up for another user:
```bash
sudo usermod -aG docker username
```

## Step 7: Test Docker Without Sudo
Log out and log back in, then verify:
```bash
docker run hello-world
```

## Step 8: Configure Docker to Start on Boot (Optional)
```bash
sudo systemctl enable docker
```

## Step 9: Allow Docker to Use External Registries (Optional)
```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<EOF
{
  "insecure-registries": ["your-registry.com:5000"]
}
EOF
sudo systemctl restart docker
```

## Step 10: Enable Docker Remote API (Optional)
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

## Step 11: Install Docker Compose (Optional)
```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

## Troubleshooting
- **Permission Denied on Docker Commands:** Ensure your user is in the `docker` group (`groups $USER`).
- **Docker Daemon Not Running:** Try restarting the service: `sudo systemctl restart docker`.
- **Pull Access Denied for Private Repos:** Make sure you're logged into DockerHub: `docker login`.

---
**Docker is now set up on your DigitalOcean droplet! 🚀**
