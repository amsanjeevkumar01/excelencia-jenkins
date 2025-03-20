# Jenkins CI/CD Pipeline for Spring Boot with SonarQube, Trivy & Kubernetes Deployment

This repository contains a Jenkins pipeline for automating the build, test, security scanning, and deployment of a Spring Boot application.

## 📌 Features
- ✅ **Automated Build & Unit Testing** using Maven
- ✅ **Code Quality Analysis** with SonarQube
- ✅ **Docker Image Build & Push** to a container registry
- ✅ **Security Scan** with Trivy
- ✅ **Kubernetes Deployment** using Jenkins Kubernetes Plugin

---

## 📋 Prerequisites

### **1. Jenkins Setup**
Ensure your Jenkins instance has the following **plugins installed**:
- 🟢 [Pipeline](https://plugins.jenkins.io/workflow-aggregator/)
- 🟢 [SonarQube Scanner](https://plugins.jenkins.io/sonarqube/)
- 🟢 [Kubernetes CLI](https://plugins.jenkins.io/kubernetes-cli/)

### **2. Docker Registry Access**
- Have a **Docker registry account** (Docker Hub, AWS ECR, Azure ACR, etc.).
- Create repository: `<your-docker-registry>/<your-app-name>`.

### **3. SonarQube Configuration**
- Set up **SonarQube Server** in Jenkins:
  1. Go to `Manage Jenkins` → `Configure System`.
  2. Find the **SonarQube Servers** section.
  3. Add your **SonarQube instance** and authentication token.

### **4. Kubernetes Cluster**
- Ensure you have a running Kubernetes cluster.
- Add **KubeConfig credentials** in Jenkins (`k8s-credentials`).

## 🏗️ Running the Pipeline
1. Go to your pipeline job in Jenkins.
2. Click `Build Now`.
3. Monitor the stages in Jenkins.

---

## 📜 Pipeline Breakdown
| Stage                  | Description |
|------------------------|-------------|
| **Checkout Code**      | Fetches the latest code from SCM. |
| **Build & Unit Tests** | Compiles the project and runs unit tests with Maven. |
| **SonarQube Analysis** | Runs static code analysis using SonarQube. |
| **Build Docker Image** | Builds a Docker image for the application. |
| **Trivy Security Scan** | Scans the Docker image for vulnerabilities. |
| **Push Docker Image**  | Pushes the built image to a Docker registry. |
| **Deploy to Kubernetes** | Updates the Kubernetes deployment |
