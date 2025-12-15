# 🚀 CI/CD Pipeline with GitHub Actions, Minikube & Monitoring

This project demonstrates an **end-to-end CI/CD pipeline** using **GitHub Actions** to build, analyze, containerize, deploy, and monitor a **Spring Boot application** on a **local Minikube Kubernetes cluster**, with **Prometheus and Grafana** for monitoring.

The entire setup runs on a **MacBook Air (M2)** using a **self-hosted GitHub Actions runner**.

---

🧱  Architecture Diagram (Your Actual Setup)


<img width="335" height="739" alt="Screenshot 2025-12-15 at 5 25 44 PM" src="https://github.com/user-attachments/assets/d3a4da95-043a-40d6-a281-761b5606d102" />

---

## 🧱 Tech Stack

| Layer | Technology |
|-----|-----------|
| SCM | GitHub |
| CI/CD | GitHub Actions (Self-hosted runner) |
| Build Tool | Maven |
| Code Quality | SonarQube |
| Language | Java 11 |
| Containerization | Docker |
| Orchestration | Kubernetes (Minikube) |
| Monitoring | Prometheus |
| Visualization | Grafana |

---

## 📁 Project Structure

cicd-maven-k8s-monitoring/
├── app/
│   ├── src/main/java/com/example/demo/
│   │   └── DemoApplication.java
│   ├── src/main/resources/
│   │   └── application.properties
│   ├── pom.xml
│   └── Dockerfile
│
├── k8s/
│   ├── app/
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── prometheus/
│   │   ├── config.yaml
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── grafana/
│       ├── deployment.yaml
│       └── service.yaml
│
├── .github/workflows/
│   └── ci.yml
│
└── README.md

---

## 🔁 CI/CD Pipeline Flow

Pipeline runs on every push to `main` branch:

1. Checkout source code
2. Build Spring Boot app using Maven
3. Run SonarQube code quality analysis
4. Build Docker image inside Minikube Docker
5. Deploy application to Kubernetes
6. Deploy Prometheus and Grafana

---

## 🧠 Why Self-Hosted Runner?

GitHub-hosted runners **cannot access local resources** like Minikube running on a developer’s laptop.

Therefore, a **self-hosted runner** is used on the same Mac where Minikube is running.

---

## 🔐 GitHub Secrets Required

Add these in **Repo → Settings → Secrets → Actions**:

| Secret | Description |
|------|------------|
| `SONAR_HOST_URL` | SonarQube URL (e.g. http://localhost:9000) |
| `SONAR_TOKEN` | SonarQube authentication token |

---

## 🐳 Docker Image

- Base image: `eclipse-temurin:11-jre`
- Image name: `maven-app:latest`
- Image is built **inside Minikube**
- No Docker registry is used

---

## ☸️ Kubernetes Deployment

- Deployment: `demo-app`
- Service: `demo-service` (NodePort)
- `imagePullPolicy: Never` is used to avoid image pull errors

---

## 📊 Monitoring
### Prometheus

- Scrapes metrics from: kubelet
- Target: `demo-service:8080`

---

### Grafana
- Connected to Prometheus as a data source
- Used for JVM, HTTP, and application metrics

---

## 🌐 Access Services Locally

Run the following commands:


```bash
minikube service demo-service
minikube service prometheus-service
minikube service grafana-service
```
