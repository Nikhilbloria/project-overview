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
