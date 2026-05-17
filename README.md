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
# ShellScape DevOps Monitoring Setup using Prometheus & Grafana

---

# Step 1: Started Kubernetes Cluster using Minikube

Minikube was used to create a local Kubernetes cluster.

## Commands Used

```bash
minikube start

kubectl get nodes
```

## Result

- Kubernetes cluster started successfully
- Node status verified using `kubectl`

---

# Step 2: Installed Helm

Helm was used as the Kubernetes package manager for installing Prometheus and Grafana easily.

## Verify Helm

```bash
helm version
```

## Install Helm (Windows)

```bash
winget install Helm.Helm
```

## Result

- Helm installed successfully
- Helm repository support enabled

---

# Step 3: Added Prometheus Helm Repository

The Prometheus Community Helm repository was added to download monitoring packages.

## Commands Used

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update
```

## Result

- Prometheus Helm charts downloaded successfully

---

# Step 4: Created Monitoring Namespace

A separate namespace was created for monitoring components.

## Command Used

```bash
kubectl create namespace monitoring
```

## Result

- Monitoring resources isolated inside Kubernetes

---

# Step 5: Installed Prometheus and Grafana

`kube-prometheus-stack` was installed using Helm.

## Command Used

```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

## Components Installed

- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- Kubernetes Metrics Exporters

## Result

- Monitoring stack deployed successfully
- Pods created automatically

---

# Step 6: Verified Monitoring Pods

Kubernetes pods were checked to ensure successful deployment.

## Command Used

```bash
kubectl get pods -n monitoring
```

## Example Output

```bash
monitoring-grafana                         Running
monitoring-kube-prometheus-prometheus     Running
monitoring-alertmanager                   Running
```

## Result

- All monitoring pods running successfully

---

# Step 7: Opened Grafana Dashboard

Grafana dashboard was exposed locally using port forwarding.

## Command Used

```bash
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
```

## Access URL

```text
http://localhost:3000
```

## Login Credentials

```text
Username: admin
Password: prom-operator
```

## Result

- Grafana dashboard opened successfully
- Kubernetes dashboards available

---

# Step 8: Opened Prometheus Dashboard

Prometheus UI was exposed locally.

## Command Used

```bash
kubectl port-forward -n monitoring svc/monitoring-kube-prometheus-prometheus 9090:9090
```

## Access URL

```text
http://localhost:9090
```

## Result

- Prometheus metrics dashboard accessible

---

# Step 9: Deployed ShellScape Application on Kubernetes

ShellScape application was deployed using Kubernetes Deployment and Service YAML files.

## Commands Used

```bash
kubectl apply -f deployment.yaml

kubectl apply -f service.yaml
```

## Verify Deployment

```bash
kubectl get pods

kubectl get svc
```

## Result

- ShellScape pods deployed successfully
- Application exposed using Kubernetes service

---

# Step 10: Monitored ShellScape using Grafana

Grafana dashboards were used to monitor application performance.

## Metrics Observed

- CPU Usage
- Memory Usage
- Pod Health
- Pod Restart Count
- Network Usage
- Cluster Health
- Node Performance

## Dashboards Used

- Kubernetes / Compute Resources / Pod
- Node Exporter Dashboard
- Kubernetes Cluster Monitoring

## Result

- Real-time visualization of ShellScape infrastructure
- Production-style monitoring implemented

---

# Step 11: Verified Metrics in Prometheus

Prometheus queries were used to monitor Kubernetes metrics.

## Example Queries

### CPU Usage

```promql
rate(container_cpu_usage_seconds_total[1m])
```

### Memory Usage

```promql
container_memory_usage_bytes
```

### Pod Status

```promql
kube_pod_status_phase
```

### Network Usage

```promql
container_network_receive_bytes_total
```

## Result

- Real-time metrics collected successfully
- Kubernetes components monitored continuously

---

# Learning Outcomes

- Learned Kubernetes monitoring architecture
- Understood Prometheus metrics collection
- Implemented Grafana dashboards
- Monitored Kubernetes pods and nodes
- Practiced production-level DevOps monitoring
- Integrated monitoring into CI/CD workflow
- Learned container orchestration monitoring

---

# Final Result

- Successfully integrated Prometheus and Grafana with ShellScape
- Implemented monitoring for Kubernetes infrastructure
- Visualized real-time metrics using Grafana dashboards
- Demonstrated DevOps, Cloud, Monitoring, and Kubernetes concepts together
- Built a production-style monitoring pipeline for containerized applications

---


# 🚀 ShellScape Deployment on AWS EC2 using Terraform, Docker & Jenkins CI/CD
---
ShellScape was deployed on AWS EC2 using Docker containers and Jenkins CI/CD automation.  
Terraform was used to automate infrastructure provisioning including EC2 instance creation, security group setup, and SSH key configuration.

The deployment pipeline was integrated with GitHub so that whenever code is pushed to the repository, Jenkins automatically:

- Pulls the latest code
- Builds a new Docker image
- Stops the old container
- Removes the old container
- Deploys the updated application automatically on AWS EC2

---

# 🏗️ Architecture Workflow

```text
Developer
   ↓
GitHub Repository
   ↓
GitHub Webhook
   ↓
Jenkins CI/CD Pipeline
   ↓
Docker Image Build
   ↓
Docker Container Deployment
   ↓
AWS EC2 Hosting
```

---

# 📁 Step 1: Created Terraform Project Folder

A dedicated Terraform project folder was created for infrastructure management.

## Project Structure

```text
shellscape-aws-terraform
│
├── main.tf
├── shellscape-key
├── shellscape-key.pub
```

This folder was used to manage:

- AWS infrastructure
- EC2 deployment
- Security configuration
- Deployment automation

---

# 🔐 Step 2: Created SSH Key Pair

An SSH key pair was generated for secure access to the EC2 instance.

## Command Used

```bash
ssh-keygen -t rsa -b 4096 -f shellscape-key
```

## Files Generated

```text
shellscape-key
shellscape-key.pub
```

## Purpose

| File | Purpose |
|---|---|
| shellscape-key | Private key used for SSH login |
| shellscape-key.pub | Public key uploaded to AWS |

This enabled secure remote access to the EC2 server.

---

# ☁️ Step 3: Created AWS Infrastructure using Terraform

Terraform was used as Infrastructure as Code (IaC) to automatically provision AWS resources.

## Resources Created

- AWS EC2 Instance
- AWS Security Group
- AWS Key Pair
- Public IP Configuration

## Terraform Features Used

- AWS region selection
- EC2 instance configuration
- Security group rules
- Docker/Jenkins installation using `user_data`

## Terraform Commands

```bash
terraform init
terraform plan
terraform apply
```

Terraform automatically created the EC2 instance without manually using the AWS Console.

---

# 🔥 Step 4: Configured AWS Security Group

A custom AWS security group was configured to allow required network traffic.

## Opened Ports

| Port | Purpose |
|---|---|
| 22 | SSH Remote Access |
| 80 | ShellScape Website |
| 8080 | Jenkins Dashboard |
| 50000 | Jenkins Agent Communication |

## Security Group Purpose

- Allow website access
- Allow Jenkins dashboard access
- Enable secure SSH connection
- Support Jenkins CI/CD communication

---

# 🖥️ Step 5: Connected to AWS EC2 Instance

After the EC2 instance was created, the generated public IP was used for SSH connection.

## SSH Command

```bash
ssh -i "shellscape-key" ec2-user@EC2_PUBLIC_IP
```

The instance was running Amazon Linux 2, so:

```text
ec2-user
```

was used as the default login user.

---

# 🐳 Step 6: Installed Docker on EC2

Docker was installed to containerize Jenkins and ShellScape.

## Commands Used

```bash
sudo yum update -y
sudo amazon-linux-extras install docker -y
sudo systemctl start docker
sudo systemctl enable docker
```

## Verify Docker Installation

```bash
sudo docker --version
```

## Purpose of Docker

- Containerization
- Isolated environment
- Simplified deployment
- Portable application execution

---

# ⚙️ Step 7: Ran Jenkins Inside Docker Container

Jenkins was deployed as a Docker container on EC2.

## Jenkins Docker Command

```bash
sudo docker run -d \
--name jenkins \
-p 8080:8080 \
-p 50000:50000 \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkins/jenkins:lts
```

## This Command Performed

- Downloaded Jenkins image
- Created Jenkins container
- Mapped Jenkins ports
- Persisted Jenkins data
- Connected Docker socket for CI/CD automation

## Jenkins Access URL

```text
http://EC2_PUBLIC_IP:8080
```

---

# 🛠️ Step 8: Configured Jenkins

Initial Jenkins setup was completed using the admin password.

## Command Used

```bash
sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## Jenkins Setup Completed

- Installed suggested plugins
- Created admin user
- Configured Jenkins dashboard

## Required Plugins Installed

- Git
- GitHub
- Pipeline
- Docker Pipeline

## Plugin Purpose

| Plugin | Purpose |
|---|---|
| Git | Source code management |
| GitHub | GitHub integration |
| Pipeline | CI/CD workflow support |
| Docker Pipeline | Docker automation |

---

# 🔓 Step 9: Enabled Docker Access for Jenkins

Jenkins initially faced Docker permission issues.

## Entered Jenkins Container

```bash
sudo docker exec -it -u root jenkins bash
```

## Commands Executed Inside Container

```bash
usermod -aG docker jenkins
chmod 666 /var/run/docker.sock
```

## Restarted Jenkins

```bash
sudo docker restart jenkins
```

## Result

Jenkins was now able to:

- Build Docker images
- Stop containers
- Remove containers
- Deploy updated containers automatically

---

# 🔄 Step 10: Created Jenkins CI/CD Pipeline

A Jenkins pipeline named:

```text
ShellScape-CICD
```

was created.

## Pipeline Configuration

```text
Pipeline script from SCM
↓
Git
↓
Repository URL
↓
Branch: main
↓
Script Path: Jenkinsfile
```

This allowed Jenkins to directly read the Jenkinsfile from GitHub.

---

# 📜 Step 11: Added Jenkinsfile

A Jenkinsfile was added inside the ShellScape repository.
## Jenkinsfile Used for CI/CD Pipeline

A `Jenkinsfile` was created in the root directory of the ShellScape project repository.

## Project Structure

```text
sheelscape
│
├── Jenkinsfile
├── Dockerfile
├── index.html
├── styles.css
├── js/
└── assets/
```

## Jenkinsfile Code

```groovy
pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                echo 'Cloning latest code from GitHub...'
                git branch: 'main', url: 'https://github.com/Nikhilbloria/sheelscape.git'
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
                sh 'docker run -d -p 80:80 --name shellscape-container shellscape:latest'
            }
        }
    }
}
```

## Pipeline Stages

- Clone latest GitHub code
- Build Docker image
- Stop old container
- Remove old container
- Deploy updated container

## Deployment Flow

```text
GitHub Code
↓
Docker Build
↓
Old Container Stop
↓
New Container Deployment
```

## Application Access

```text
http://EC2_PUBLIC_IP
```

---

# 🔗 Step 12: Added GitHub Webhook

GitHub webhook integration was configured for automatic CI/CD triggering.

## Webhook URL

```text
http://EC2_PUBLIC_IP:8080/github-webhook/
```

## Event Selected

```text
Just the push event
```

## Webhook Workflow

```text
GitHub Push
↓
Webhook Trigger
↓
Jenkins Pipeline Execution
```

This removed the need to manually click **Build Now**.

---

# 🚀 Step 13: Enabled Jenkins GitHub Trigger

Inside Jenkins pipeline configuration:

```text
Build Triggers
↓
GitHub hook trigger for GITScm polling
```

was enabled.

This allowed Jenkins to automatically start deployments whenever GitHub receives new commits.

---

# 🔄 Final Automated CI/CD Workflow

```text
Developer Code Changes
↓
Git Push to GitHub
↓
GitHub Webhook Trigger
↓
Jenkins Pulls Latest Code
↓
Docker Image Rebuilt
↓
Old Container Removed
↓
New Container Started
↓
Updated ShellScape Live on AWS EC2
```

---

# ✅ Final Result

ShellScape was successfully deployed on AWS EC2 using:

- Terraform
- AWS EC2
- Docker
- Jenkins
- GitHub
- CI/CD Automation

The deployment process became fully automated and production-style.

---

# 🌐 Elastic IP Configuration & Auto-Restart Setup

After deploying ShellScape on AWS EC2 using Docker and Jenkins CI/CD, an Elastic IP was configured to create a permanent public IP address for the server.

## Why Elastic IP Was Used

Normally, when an AWS EC2 instance is stopped and started again, AWS changes the public IP address automatically.

This creates problems such as:

- Jenkins URL changes
- Website URL changes
- GitHub webhook stops working
- Manual IP updates are required

To solve this issue, an Elastic IP was assigned to the EC2 instance.

---

# ☁️ Elastic IP Configuration

An Elastic IP was allocated from the AWS EC2 dashboard and associated with the ShellScape EC2 instance.

## Steps Performed

```text
AWS Console
↓
EC2
↓
Elastic IPs
↓
Allocate Elastic IP Address
↓
Associate Elastic IP with EC2 Instance
```

## Assigned Elastic IP

```text
54.204.135.197
```

---

# 🌐 Permanent URLs Created

## ShellScape Website

```text
http://54.204.135.197
```

## Jenkins Dashboard

```text
http://54.204.135.197:8080
```

These URLs now remain permanent even after restarting the EC2 instance.

---

# 🔄 Updated GitHub Webhook

The GitHub webhook was updated using the Elastic IP address.

## Webhook URL

```text
http://54.204.135.197:8080/github-webhook/
```

This ensured Jenkins CI/CD automation continued working after EC2 restarts.

---

# 🐳 Enabled Automatic Docker Container Restart

Docker restart policies were configured so containers automatically start whenever EC2 starts again.

## Restart Policy Used

```text
unless-stopped
```

## Jenkins Container Status

```bash
sudo docker inspect -f '{{ .HostConfig.RestartPolicy.Name }}' jenkins
```

Output:

```text
unless-stopped
```

## ShellScape Container Restart Policy

```bash
sudo docker update --restart unless-stopped shellscape-container
```

---

# 🚀 Final Automated Startup Workflow

Now whenever the EC2 instance is started:

```text
EC2 Starts
↓
Docker Service Starts
↓
Jenkins Container Starts Automatically
↓
ShellScape Container Starts Automatically
↓
Website Becomes Live
```

No manual Docker commands are required anymore.

---

# ✅ Final Result

Successfully configured:

- AWS Elastic IP
- Permanent Public IP
- Stable Jenkins URL
- Stable Website URL
- Persistent GitHub Webhook
- Automatic Docker Container Restart
- Production-style AWS deployment workflow
  

# 📚 Learning Outcomes

Through this project, I learned:

- Infrastructure as Code using Terraform
- AWS EC2 cloud deployment
- Docker containerization
- Jenkins CI/CD automation
- GitHub webhook integration
- Automated Docker deployment
- Security group configuration
- SSH key-based authentication
- Real-world DevOps deployment workflow
- Automated application update process

---
