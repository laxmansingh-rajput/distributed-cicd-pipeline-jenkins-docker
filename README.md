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

## Getting Started

### Prerequisites

- AWS Account with EC2 access
- Docker Hub account
- GitHub repository
- Domain name (optional, for NGINX configuration)

### Setup Steps

1. **Create AWS EC2 Instances**
   - Launch two EC2 instances (one for Master, one for Agent)
   - Configure security groups to allow necessary ports:
     - Jenkins Master: 8080 (Jenkins UI), 22 (SSH)
     - Jenkins Agent: 22 (SSH), 80 (HTTP), 443 (HTTPS)

2. **Install Required Software**
   - Follow the [Installation Guide](./docs/Installation.md) to install:
      - Java (JRE 21)
     - Jenkins (on Master)
     - Docker (on Agent)
     - Docker Compose (on Agent)
     - NGINX (on Agent)

3. **Connect Master and Agent**
   - Follow the [SSH Connection Guide](./docs/sshConnection.md) to establish SSH connectivity between Master and Agent nodes

4. **Configure Jenkins Pipeline**
   - Set up GitHub Webhook
   - Create Jenkins pipeline job

5. **Configure the credentials and Env**
    - Configure Docker Hub credentials
    - Configure github reposetory credentials
    - Add the Enviornment variables as the secret file 
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
├── backend/
├── docker-compose.yml
├── Jenkinsfile
└── README.md
```

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



