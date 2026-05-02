#  MERN Orchestration & Scaling Project

## Project Overview

This project demonstrates a complete **end-to-end DevOps pipeline** for a MERN (MongoDB, Express, React, Node.js) application, covering:

* Version Control with Git
* Containerization using Docker
* CI/CD with Jenkins
* Deployment on AWS EKS (Kubernetes)
* Monitoring & Logging with CloudWatch
* Optional ChatOps Integration

---

## Tech Stack

* **Frontend:** React
* **Backend:** Node.js, Express
* **Database:** MongoDB
* **CI/CD:** Jenkins
* **Containerization:** Docker
* **Container Registry:** Amazon ECR
* **Orchestration:** Kubernetes (EKS)
* **Monitoring & Logging:** AWS CloudWatch
* **Cloud Provider:** AWS

---

## Project Workflow

---

## Step 1: Version Control with Git

### Fork & Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/StreamingApp.git
cd StreamingApp
```

### Add Upstream Repository

```bash
git remote add upstream https://github.com/UnpredictablePrashant/StreamingApp.git
git remote -v
```

### Sync Fork with Upstream

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Step 2: Prepare the MERN Application

### Containerization

Create Dockerfiles:

#### Backend Dockerfile

```Dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

#### Frontend Dockerfile

```Dockerfile
FROM node:18 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

---

### Push Images to Amazon ECR

#### Create Repositories

```bash
aws ecr create-repository --repository-name frontend
aws ecr create-repository --repository-name backend
```

#### Authenticate Docker to ECR

```bash
aws ecr get-login-password --region <region> \
| docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```

#### Build & Push Images

```bash
docker build -t frontend .
docker tag frontend:latest <ECR_URL>/frontend:latest
docker push <ECR_URL>/frontend:latest

docker build -t backend .
docker tag backend:latest <ECR_URL>/backend:latest
docker push <ECR_URL>/backend:latest
```

---

## Step 3: AWS Environment Setup

### Install AWS CLI

```bash
aws configure
```

Provide:

* Access Key
* Secret Key
* Region
* Output format

---

## Step 4: Continuous Integration (CI) with Jenkins

### Jenkins Setup

* URL: https://jenkinsacademics.herovired.com/
* Username: `herovired`
* Password: `herovired`

### Required Plugins

* Git
* Docker Pipeline
* AWS Credentials
* Pipeline

---

### Jenkins Pipeline (Jenkinsfile)

Jenkins is used to automate the build and integration process. A CI pipeline is created to fetch code from the GitHub repository, build container images, and push them to Amazon ECR.

The pipeline is configured to trigger automatically whenever changes are pushed to the repository. This ensures continuous integration and reduces manual effort in the development workflow.

## Step 5: Kubernetes Deployment (EKS)

Amazon Elastic Kubernetes Service (EKS) is used to orchestrate and manage the containerized application. A Kubernetes cluster is created to host the application.

Helm charts are used to define and manage Kubernetes resources such as deployments and services. This simplifies the deployment process and enables easy scaling and updates.

### Create EKS Cluster

```bash
eksctl create cluster --name mern-cluster --region <region>
```

---

### Deploy using Helm

```bash
helm create mern-chart
```

Update:

* Deployment.yaml
* Service.yaml
* Values.yaml

Deploy:

```bash
helm install mern-app ./mern-chart
```

---

## Step 6: Monitoring & Logging

### Monitoring with CloudWatch

* Enable Container Insights
* Set alarms for CPU, memory usage

### Logging

* Configure logs using:

```bash
kubectl logs <pod-name>
```

* Push logs to CloudWatch Logs

---

## Step 7: Documentation

Include:

* Architecture Diagram
* Deployment Steps
* Jenkins Pipeline
* Kubernetes Configs
* Screenshots

Upload to GitHub repository.

---

## Step 8: Final Validation

Verify:

* Frontend accessible via LoadBalancer
* Backend API working
* Pods running successfully

```bash
kubectl get pods
kubectl get svc
```

---

## Bonus: ChatOps Integration

### Create SNS Topic

```bash
aws sns create-topic --name deployment-alerts
```

### Integrate with Slack / Teams

* Use Webhooks
* Subscribe endpoint to SNS

---

## Sceeenshots:

<img width="940" height="508" alt="image" src="https://github.com/user-attachments/assets/633c45da-f52a-47d7-b741-240f9120f035" />

<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/2f72c47d-aadf-4eff-89fa-c75e9bf3961b" />

<img width="940" height="295" alt="image" src="https://github.com/user-attachments/assets/84be3858-93c3-450a-93cf-ec4d3c58bdf9" />

<img width="940" height="497" alt="image" src="https://github.com/user-attachments/assets/0e0c87b5-6e08-4d4e-b1df-625ca1a89939" />

<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/1b9734ba-133d-49ae-ae0b-304dc3a216d3" />

<img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/f6c43eb7-9c5f-48e8-b3ea-b1e60cbd782d" />

<img width="764" height="300" alt="image" src="https://github.com/user-attachments/assets/1d1cd7dd-9b25-4cee-94f0-a60950246e21" />

<img width="940" height="230" alt="image" src="https://github.com/user-attachments/assets/5d925b2d-d50e-469e-b8b1-7c3b5d9067bc" />

<img width="472" height="705" alt="image" src="https://github.com/user-attachments/assets/8fddfb77-e88d-4a58-85cf-f376e230c089" />


## Architecture Diagram (High-Level)

```
GitHub → Jenkins → Docker → Amazon ECR → EKS → CloudWatch
```

---

## Final Deliverables

* ✔ Dockerized MERN Application
* ✔ Jenkins CI/CD Pipeline
* ✔ ECR Repositories
* ✔ EKS Deployment with Helm
* ✔ Monitoring & Logging Setup
* ✔ GitHub Documentation

---

## Conclusion

This project demonstrates real-world DevOps practices including:

* Automation
* Scalability
* Container orchestration
* Cloud-native deployment

---

## Author

**Sweta Patil**
 
