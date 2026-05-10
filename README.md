ShellScape DevOps Deployment Project
Project Overview

ShellScape is a browser-based terminal simulation project that was containerized and deployed using modern DevOps and Cloud technologies. The objective of this project was to implement a complete CI/CD and deployment workflow using industry-standard tools.

Technologies Used
Technology	Purpose
Git	Version control
GitHub	Source code hosting
Docker	Containerization
Jenkins	CI/CD automation
Terraform	Infrastructure provisioning
Kubernetes	Container orchestration
Amazon EC2	Cloud hosting
Amazon EBS	Persistent storage
Project Workflow
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
EBS Persistent Storage
Implementation Steps
1. Version Control using Git & GitHub
Initialized local Git repository
Added project files to Git
Committed source code changes
Pushed ShellScape project to GitHub repository

Commands used:

git init
git add .
git commit -m "Initial commit"
git push origin main
2. Docker Containerization

The ShellScape project was containerized using Docker and served through Nginx.

Dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80
Docker Commands
docker build -t shellscape .

docker run -d -p 3000:80 --name shellscape-container shellscape
Result
Application successfully containerized
ShellScape accessible through Docker container
Improved portability and deployment consistency
3. Docker Hub Integration

The Docker image was pushed to Docker Hub for centralized image storage and deployment.

Commands Used
docker login

docker tag shellscape <dockerhub-username>/shellscape

docker push <dockerhub-username>/shellscape
Result
Docker image successfully uploaded to Docker Hub
Image available for Kubernetes and CI/CD deployment
