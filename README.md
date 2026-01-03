# Distributed CI/CD Pipeline with Jenkins and Docker

This project implements an end-to-end CI/CD pipeline for a distributed 3-tier full-stack web application using Jenkins and Docker. The pipeline is automatically triggered via a GitHub Webhook whenever code is pushed to the repository.

Upon receiving the webhook event, the Jenkins Master initiates the pipeline execution on a Jenkins Agent. The agent clones the latest code, builds and versions Docker images, securely injects environment variables, pushes images to Docker Hub, and deploys the frontend and backend services using Docker Compose.

The deployment is hosted on AWS EC2 and follows a production-like setup. NGINX is configured as a reverse proxy to route incoming traffic to the appropriate services based on domain names, enabling multiple applications to run on a single EC2 instance.

---

## Architecture Overview

The CI/CD pipeline follows a webhook-driven, multi-node Jenkins architecture:

1. **GitHub Webhook** triggers the pipeline on every code push
2. **Jenkins Master** receives the webhook and orchestrates the pipeline
3. **Jenkins Agent** clones the updated repository
4. **Docker images** are built and versioned on the agent
5. **Images are pushed** to Docker Hub
6. **Docker Compose** deploys the containers on AWS EC2
7. **NGINX** acts as a reverse proxy to route traffic based on domain names to specific application ports


![Architecture Diagram](./assets/architecture.png)


---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **Cloud** | AWS EC2 |
| **CI/CD** | Jenkins (Master-Agent architecture) |
| **Containerization** | Docker |
| **Container Registry** | Docker Hub |
| **Web Server / Reverse Proxy** | NGINX |
| **Version Control** | Git & GitHub |
| **Application** | Full-Stack Application (Frontend + Backend) |

---

## Prerequisites

- AWS Account with EC2 access
- Docker Hub account
- GitHub repository
- Domain name (optional, for NGINX configuration)

---

## Getting Started

### 1. Create AWS EC2 Instances

Launch two EC2 instances (one for Master, one for Agent) and configure security groups to allow necessary ports:

- **Jenkins Master:** 8080 (Jenkins UI), 22 (SSH)
- **Jenkins Agent:** 22 (SSH), 80 (HTTP), 443 (HTTPS)

### 2. Install Required Software

Follow the [Installation Guide](./Docs/Installation.md) to install:

- Java (JRE 21)
- Jenkins (on Master)
- Docker (on Agent)
- Docker Compose (on Agent)
- NGINX (on Agent)

### 3. Connect Master and Agent

Follow the [SSH Connection Guide](./Docs/sshConnection.md) to establish SSH connectivity between Master and Agent nodes.

### 4. Prepare Docker Files

- Add [frontend Dockerfile](./Frontend/dockerfile) in the `frontend/` directory for the frontend application
- Add [backend Dockerfile](./Backend/dockerFile) in the `backend/` directory for the backend application
- Add [docker-compose.yaml](./dockercompose.yaml) file at the root of the repository

### 5. Configure Credentials and Environment Variables

- Configure Docker Hub credentials in Jenkins
- Configure GitHub repository credentials in Jenkins
- Add the environment variables as a secret file in Jenkins

### 6. Configure Jenkins Pipeline

- Create a Jenkins pipeline job using the [Jenkinsfile](./jenkins/jenkinsFile)
- Set up GitHub Webhook in your repository settings

### 7. Configure NGINX

Add NGINX configuration files:

```bash
# Add frontend configuration
sudo cp Nginx/frontend.conf /etc/nginx/sites-available/

# Add backend configuration
sudo cp Nginx/backend.conf /etc/nginx/sites-available/

# Create soft links to sites-enabled
sudo ln -s /etc/nginx/sites-available/frontend.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/backend.conf /etc/nginx/sites-enabled/

# Test NGINX configuration
sudo nginx -t

# Restart NGINX
sudo systemctl restart nginx
```

---

## Project Structure

```
distributed-cicd-pipeline-jenkins-docker/
├── assets/
│   └── architecture.png
├── docs/
│   ├── Installation.md
│   └── sshConnection.md
├── frontend/
│   ├── Dockerfile          # Frontend container configuration
│   ├── .dockerignore       # Files to exclude from Docker build
│   └── ...                 # Frontend application files
├── backend/
│   ├── Dockerfile          # Backend container configuration
│   ├── .dockerignore       # Files to exclude from Docker build
│   └── ...                 # Backend application files
├── Nginx/
│   ├── frontend.conf       # NGINX frontend configuration
│   └── backend.conf        # NGINX backend configuration
├── Jenkins/
│   └── jenkinsFile         # Jenkins pipeline configuration
├── docker-compose.yml      # Multi-container orchestration file
└── README.md
```

---

## Jenkins Pipeline Stages

The Jenkins pipeline consists of the following stages:

1. **Checkout** – Clones the latest code from GitHub
2. **Build** – Builds Docker images for frontend and backend
3. **Tag & Version** – Tags images with build number / version
4. **Push** – Pushes Docker images to Docker Hub
5. **Deploy** – Deploys containers using Docker Compose

---

## Workflow

1. Developer pushes code to GitHub repository
2. GitHub webhook notifies Jenkins Master
3. Jenkins Master delegates the job to Jenkins Agent
4. Agent pulls the latest code from GitHub
5. Docker images are built from Dockerfiles in `frontend/` and `backend/` directories
6. Images are tagged with version numbers and pushed to Docker Hub
7. Docker Compose pulls the images and deploys containers on EC2
8. NGINX routes traffic to the appropriate services based on domain configuration

---

## Ports & Access

| Service | Port | Access |
|---------|------|--------|
| Jenkins UI | 8080 | Public (Web Interface) |
| Frontend | 80 / 443 | Public (via NGINX) |
| Backend | Internal | Private (via NGINX Proxy) |
| SSH | 22 | Restricted (Key-based) |

---

## Features

✅ Automated CI/CD pipeline triggered by GitHub Webhooks  
✅ Distributed build execution using Jenkins Master-Agent architecture  
✅ Docker containerization for consistent deployments  
✅ Automated image versioning and registry management  
✅ Multi-service deployment with Docker Compose  
✅ NGINX reverse proxy for domain-based routing  
✅ Production-ready deployment on AWS EC2  

---

## Troubleshooting

**Jenkins Agent Connection Issues**
- Verify SSH connectivity between Master and Agent
- Check agent logs in Jenkins UI
- Ensure Java is installed on the agent

**Docker Build Failures**
- Ensure Dockerfiles are properly configured in respective directories
- Verify Docker daemon is running on the agent
- Check Docker Hub credentials in Jenkins
- Check the path of the frontend and backend dockerfile in the docker-compose.yaml

**NGINX Errors**
- Run `sudo nginx -t` to validate configuration syntax
- Check NGINX error logs: `sudo tail -f /var/log/nginx/error.log`
- Ensure domain DNS is properly configured

**Port Conflicts**
- Verify security group rules in AWS EC2

---
