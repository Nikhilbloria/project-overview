# ShellScape DevOps Deployment Project

## Project Overview

ShellScape is a browser-based terminal simulation project that was containerized and prepared for deployment using modern DevOps and Cloud technologies.  
The objective of this project is to implement a complete CI/CD and deployment workflow using industry-standard tools and practices.

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Git | Version Control |
| GitHub | Source Code Hosting |
| Docker | Containerization |
| Docker Hub | Container Image Registry |
| Jenkins | CI/CD Automation |
| Terraform | Infrastructure Provisioning |
| Kubernetes | Container Orchestration |
| Amazon EC2 | Cloud Hosting |
| Amazon EBS | Persistent Storage |

---

# Project Workflow

```text
Developer
   ↓
GitHub Repository
   ↓
Jenkins CI/CD Pipeline
   ↓
Docker Image Build
   ↓
Docker Hub
   ↓
Kubernetes Deployment
   ↓
AWS EC2 Instance
   ↓
Amazon EBS Storage
```

---

# Implementation Steps

## 1. Version Control using Git & GitHub

The ShellScape project source code was managed using Git and hosted on GitHub.

### Steps Performed

- Initialized local Git repository
- Added project files to Git tracking
- Committed source code changes
- Pushed project to GitHub repository

### Commands Used

```bash
git init
git add .
git commit -m "Initial commit"
git push origin main
```

---

## 2. Docker Containerization

The ShellScape project was containerized using Docker and served using an Nginx web server.

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80
```

### Docker Commands

#### Build Docker Image

```bash
docker build -t shellscape .
```

#### Run Docker Container

```bash
docker run -d -p 3000:80 --name shellscape-container shellscape
```

### Result

- Application successfully containerized
- ShellScape accessible through Docker container
- Improved deployment consistency and portability

---

## 3. Docker Hub Integration

The Docker image was uploaded to Docker Hub for centralized image storage and future deployment through Kubernetes and CI/CD pipelines.

### Commands Used

#### Login to Docker Hub

```bash
docker login
```

#### Tag Docker Image

```bash
docker tag shellscape <dockerhub-username>/shellscape
```

#### Push Docker Image

```bash
docker push <dockerhub-username>/shellscape
```

### Result

- Docker image successfully uploaded to Docker Hub
- Image available for Kubernetes and CI/CD deployment

---

# Planned DevOps Integrations

The following technologies will be integrated in the next phases of the project.

## Jenkins CI/CD Pipeline

- Automate build and deployment process
- Trigger builds automatically on GitHub commits

## Terraform

- Provision AWS infrastructure using Infrastructure as Code (IaC)

## Kubernetes

- Deploy and manage Docker containers
- Enable scalability and orchestration

## AWS EC2 & EBS

- Host application on cloud infrastructure
- Attach persistent storage using EBS volumes

---

# Project Objectives

- Implement modern DevOps practices
- Automate deployment workflow
- Containerize web applications
- Integrate CI/CD pipeline
- Prepare scalable cloud deployment architecture

---

# Future Enhancements

- Complete Jenkins pipeline automation
- Kubernetes cluster deployment
- HTTPS and domain integration
- Monitoring and logging setup
- Load balancing and scaling

---

# Conclusion

The ShellScape DevOps Deployment Project demonstrates the implementation of containerization, cloud deployment preparation, and DevOps workflow integration using industry-standard tools including Docker, GitHub, Jenkins, Terraform, Kubernetes, and AWS services.
