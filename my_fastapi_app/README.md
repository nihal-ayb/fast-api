# Simple FastAPI Application

A lightweight FastAPI starter API with **Docker** containerisation, a **Jenkins CI/CD pipeline**, and **Kubernetes** deployment manifests.

## 📁 Project Structure

```
my_fastapi_app/
├── manifests/              # Kubernetes manifests
│   ├── deployment.yaml     # Deployment (rolling update, 2 replicas)
│   └── service.yaml        # ClusterIP Service (80 -> 8000)
├── main.py                 # FastAPI application entry point
├── requirements.txt        # Python dependencies
├── Dockerfile              # Python/Uvicorn container image
├── .dockerignore           # Files excluded from Docker build context
├── Jenkinsfile             # Declarative CI/CD pipeline
├── .gitignore              # Files excluded from Git
└── README.md               # This file
```

## 🚀 Run locally

```bash
pip install -r requirements.txt
uvicorn main:app --reload
# Docs: http://localhost:8000/docs
```

## 🐳 Docker

```bash
docker build -t my-fastapi-app:latest .
docker run -d -p 8000:8000 --name my-fastapi-app my-fastapi-app:latest
```

## ⚙️ Jenkins CI/CD Pipeline

| # | Stage | Description |
|---|-------|-------------|
| 1 | **Checkout Source Code** | Pull source code from SCM |
| 2 | **SonarQube Analysis** | Static analysis via `sonar-scanner` |
| 3 | **Build & Push Docker Image** | Push `:<BUILD_NUMBER>` and `:latest` to Docker Hub |
| 4 | **Update Manifest Repo** | Set new image tag in `my-fastapi-app/manifests/deployment.yaml` in the manifest repo |

Jenkins prerequisites: `docker-cred` (Docker Hub), `github-cred` (GitHub PAT with repo write), `sonarqube-env` (SonarQube server), `sonar-scanner` (scanner tool).

## ☸️ Kubernetes

```bash
kubectl apply -f manifests/
kubectl rollout status deployment/my-fastapi-app-deployment -n production
```
