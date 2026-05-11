# ShellScape DevOps Deployment Project

## Project Overview

ShellScape is a browser-based terminal simulation project that was containerized and prepared for deployment using modern DevOps and Cloud technologies.  
The objective of this project is to implement a complete CI/CD and deployment workflow using industry-standard tools and practices.

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

## 4. Kubernetes Deployment

The ShellScape application was deployed using Kubernetes for container orchestration and container management.

Kubernetes was used to:
- Deploy Docker containers
- Manage application pods
- Expose application services
- Enable scalability and orchestration

---

### Kubernetes Deployment File

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: shellscape-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: shellscape

  template:
    metadata:
      labels:
        app: shellscape

    spec:
      containers:
      - name: shellscape
        image: <dockerhub-username>/shellscape

        ports:
        - containerPort: 80
```

---

### Kubernetes Service File

```yaml
apiVersion: v1
kind: Service

metadata:
  name: shellscape-service

spec:
  type: LoadBalancer

  selector:
    app: shellscape

  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

---

### Kubernetes Commands Used

#### Deploy Application

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

#### Check Running Pods

```bash
kubectl get pods
```

#### Check Services

```bash
kubectl get services
```

#### Port Forward Application

```bash
kubectl port-forward service/shellscape-service 3000:80
```

Application accessible at:

```text
http://localhost:3000
```

---

### Temporary Stop and Restart of Kubernetes Pods

#### Stop Application Temporarily

```bash
kubectl scale deployment shellscape-deployment --replicas=0
```

#### Start Application Again

```bash
kubectl scale deployment shellscape-deployment --replicas=2
```

---

### Result

- Successfully deployed ShellScape application on Kubernetes
- Multiple pods created for scalability
- Application exposed through Kubernetes service
- Kubernetes used for container orchestration and deployment management Project demonstrates the implementation of containerization, cloud deployment preparation, and DevOps workflow integration using industry-standard tools including Docker, GitHub, Jenkins, Terraform, Kubernetes, and AWS services.

---

# 🚀 Jenkins CI/CD Setup for ShellScape

## 📌 Step 1: Ran Jenkins using Docker

Jenkins was running inside a Docker container.

```powershell
docker ps
```

Jenkins was available at:

```text
http://localhost:9090
```

---

# 🔗 Step 2: Connected Jenkins with Docker

Docker access was given to Jenkins so Jenkins could run Docker commands during deployment.

Jenkins container was connected with Docker socket:

```powershell
-v //var/run/docker.sock:/var/run/docker.sock
```

This allowed Jenkins to execute:

```text
docker build
docker stop
docker rm
docker run
```

---

# 🐳 Step 3: Installed Docker inside Jenkins Container

Entered Jenkins container as root:

```powershell
docker exec -it -u root jenkins-container bash
```

Installed Docker commands inside Jenkins:

```bash
apt update
apt install docker.io -y
usermod -aG docker jenkins
exit
```

Restarted Jenkins:

```powershell
docker restart jenkins-container
```

---

# 🔌 Step 4: Installed Required Jenkins Plugins

Installed required plugins from Jenkins dashboard:

```text
Manage Jenkins → Plugins → Available Plugins
```

Plugins used:

```text
GitHub Integration
Pipeline
Docker Pipeline
Docker API
Pipeline: GitHub
```

---

# 📦 Step 5: Added Dockerfile to Project

Created a `Dockerfile` in the ShellScape project.

```dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80
```

This Dockerfile serves the ShellScape frontend using Nginx.

---

# ⚙️ Step 6: Added Jenkinsfile to Project

Created a `Jenkinsfile` in the root of the project.

```groovy
pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                echo 'Cloning latest code from GitHub...'
                git branch: 'main',
                url: 'https://github.com/Nikhilbloria/sheelscape.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t shellscape:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container...'
                sh 'docker stop shellscape-container || true'
            }
        }

        stage('Remove Old Container') {
            steps {
                echo 'Removing old container...'
                sh 'docker rm shellscape-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                echo 'Starting new container...'
                sh 'docker run -d -p 8082:80 --name shellscape-container shellscape:latest'
            }
        }
    }
}
```

---

# 📤 Step 7: Pushed Code to GitHub

Committed and pushed project files to GitHub.

```powershell
git add .
git commit -m "Add Jenkins CI/CD pipeline"
git push origin main
```

Repository used:

```text
https://github.com/Nikhilbloria/sheelscape.git
```

---

# 🏗️ Step 8: Created Jenkins Pipeline Job

In Jenkins dashboard:

```text
New Item → Pipeline
```

Job name:

```text
ShellScape-CICD
```

---

# 🔄 Step 9: Connected Jenkins Job with GitHub

In pipeline configuration:

```text
Definition: Pipeline script from SCM
SCM: Git
Repository URL: https://github.com/Nikhilbloria/sheelscape.git
Branch: */main
Script Path: Jenkinsfile
```

Then clicked:

```text
Save
```

---

# ▶️ Step 10: Ran Jenkins Pipeline

Clicked:

```text
Build Now
```

Jenkins performed these steps automatically:

```text
Cloned latest code from GitHub
Built Docker image
Stopped old container
Removed old container
Started new container
```

---

# ✅ Step 11: Verified Successful Deployment

Jenkins pipeline completed with:

```text
Finished: SUCCESS
```

The application was deployed using Docker and was available at:

```text
http://localhost:8082
```

---

# 🔥 Final CI/CD Workflow

```text
Code pushed to GitHub
        ↓
Jenkins pipeline runs
        ↓
Latest code is cloned
        ↓
Docker image is built
        ↓
Old container is stopped
        ↓
Old container is removed
        ↓
New container is started
        ↓
Updated ShellScape app is live
```
### Result

- Successfully implemented a complete CI/CD pipeline for the ShellScape project using Jenkins and Docker.
- Automated the workflow from GitHub code push to Docker container deployment.
- Jenkins automatically cloned the latest repository, built Docker images, and deployed updated containers.
- Improved deployment speed, reduced manual work, and minimized deployment errors.
- Integrated GitHub, Jenkins, and Docker to create an automated DevOps workflow.
- Project demonstrates practical implementation of CI/CD automation and containerized deployment using industry-standard DevOps tools.

 ---
 
# 🚀 Docker vs Jenkins vs Kubernetes

These three tools work together in modern DevOps workflows, but each has a different purpose.

| Tool | Main Purpose |
|------|---------------|
| Docker | Create and run containers |
| Jenkins | Automate CI/CD pipelines |
| Kubernetes | Manage and scale containers |

---

# 🐳 1. Docker

Docker is used to:

- Package applications and dependencies
- Create lightweight containers
- Run applications consistently on any system

## 📌 Example

```text
Your ShellScape App
        ↓
Docker Image
        ↓
Docker Container
```

## 🔧 Docker Responsibilities

- Build images
- Run containers
- Stop containers
- Provide isolated environments

## 💻 Example Docker Commands

```bash
docker build
docker run
docker stop
```

## 🧠 Simple Meaning

Docker is like:

> **Container Creator**

---

# 🤖 2. Jenkins

Jenkins is used to:

- Automate the software development workflow
- Perform Continuous Integration and Continuous Deployment (CI/CD)

## 📌 Example Workflow

```text
GitHub Push
      ↓
Automatic Build
      ↓
Automatic Deployment
```

## 🔧 Jenkins Responsibilities

- Pull code from GitHub
- Run tests
- Execute pipelines
- Automate Docker commands
- Handle CI/CD automation

## 📌 Example

Your Jenkins pipeline automatically executed:

```bash
docker build
docker stop
docker rm
docker run
```

## 🧠 Simple Meaning

Jenkins is like:

> **Automation Manager**

---

# ☸️ 3. Kubernetes

Kubernetes is used to:

- Manage many containers automatically
- Handle production-scale deployments

## 🔧 Kubernetes Responsibilities

- Auto scaling
- Load balancing
- Self-healing containers
- Container orchestration
- Multi-container management

## 📌 Example

Suppose:

```text
1000 users visit your website
```

## 🧠 Simple Meaning

Kubernetes is like:

> **Container Manager for Large Systems**

---

# 🔄 Real Relationship Between Them

```text
Docker creates containers
        ↓
Kubernetes manages containers
        ↓
Jenkins automates deployment
```

---

# 🍔 Real-Life Analogy

| Tool | Analogy |
|------|----------|
| Docker | Creates packaged food |
| Jenkins | Automates kitchen workflow |
| Kubernetes | Manages many kitchens/restaurants |

---

# 📝 In One Line

| Tool | One-Line Meaning |
|------|------------------|
| Docker | Containerization |
| Jenkins | Automation |
| Kubernetes | Orchestration |

---
