# Jenkins Master–Agent Setup

This document describes the installation and configuration steps for Jenkins Master and Jenkins Agent nodes.

---

## Master Node Setup

### Install Java

```bash
sudo apt update
sudo apt install -y fontconfig openjdk-21-jre
java -version
```

### Install Jenkins

```bash
# Add Jenkins repository key
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

# Add Jenkins repository
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt update
sudo apt install -y jenkins

# Start and enable Jenkins service
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### Access Jenkins

Once installed, access Jenkins at:
```
http://<master-ip>:8080
```

Retrieve the initial admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## Agent Node Setup

### Install Java

```bash
sudo apt update
sudo apt install -y fontconfig openjdk-21-jre
java -version
```

### Install Docker

```bash
sudo apt install -y docker.io
sudo usermod -aG docker $USER
```

**Note:** Log out and log back in for the group changes to take effect.

### Install Nginx

```bash
sudo apt install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Install Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.38.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### Verify Installations

```bash
# Verify Docker
docker --version

# Verify Docker Compose
docker compose version

# Verify Nginx
nginx -v
```

---

## Next Steps

After completing the installation:

1. Configure SSH access between Master and Agent
2. Add the Agent node in Jenkins (Manage Jenkins → Nodes)
3. Configure build jobs to run on the Agent