# distributed-cicd-pipeline-jenkins-docker
This project implements an end-to-end CI/CD pipeline for a distributed 3-tier full-stack web application using Jenkins and Docker. The pipeline automates source code cloning, Docker image building and versioning, secure environment variable injection, publishing images to Docker Hub, and containerized deployment of frontend and backend services.

The deployment is hosted on AWS EC2 and follows a production-like setup. A multi-node Jenkins architecture (Master–Agent) is used to efficiently handle build and deployment tasks. NGINX is configured as a reverse proxy to route incoming traffic to the appropriate services based on domain names, enabling multiple applications to run on a single EC2 instance.

## Architecture Overview

The CI/CD pipeline follows a multi-node Jenkins architecture:

- Jenkins Master handles pipeline orchestration
- Jenkins Agent performs build and Docker image creation
- Docker images are pushed to Docker Hub
- The application is deployed on an AWS EC2 instance
- NGINX acts as a reverse proxy to route traffic based on domain names to specific application ports
![Architecture Diagram](./assets/architecture.png)
