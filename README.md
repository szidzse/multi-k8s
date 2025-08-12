# Multi-Service Kubernetes Application

This project is a multi-container application deployed on Google Kubernetes Engine (GKE) using GitHub Actions for continuous deployment.
It includes a React frontend, a Node.js/Express API server, a worker service, and backing services (PostgreSQL and Redis), all orchestrated with Kubernetes.

<img width="2072" height="713" alt="Screenshot From 2025-08-12 12-05-09" src="https://github.com/user-attachments/assets/9a667718-59de-4445-8361-1d44f3f2e7f5" />

## 📦 Tech Stack

- Frontend: React (served via Nginx)
- Backend API: Node.js + Express
- Worker Service: Node.js background job processor
- Database: PostgreSQL
- Cache: Redis
- Containerization: Docker
- Orchestration: Kubernetes (GKE)
- CI/CD: GitHub Actions
- Ingress: Nginx Ingress Controller

## ⚙️ How It Works

- Frontend (client)
  - React app built and served via Nginx.
  - Communicates with the API server via the Ingress controller.

- Backend (server)
  - Node.js + Express app exposing REST endpoints.
  - Uses PostgreSQL for persistence and Redis for caching.

Worker
  - Processes background jobs from Redis.

Kubernetes Setup
  - Each service runs in its own Deployment with a ClusterIP Service for internal communication.
  - PostgreSQL uses a PersistentVolumeClaim for data storage.
  - Ingress-Nginx routes external traffic to the correct service.

## 🚀 Deployment Process

Deployment is handled automatically via GitHub Actions when changes are pushed to the main branch.
CI/CD Pipeline Steps:

1. Run Tests – Build and run tests for the React frontend.
2. Authenticate with GCP – Use a service account key stored in GitHub Secrets.
3. Build Docker Images – For client, server, and worker.
4. Push Images – To Docker Hub with latest and commit SHA tags.
5. Apply Kubernetes Configs – Apply manifests from the k8s folder.
6. Update Deployments – Force deployments to use the new image tags.

## 🔑 Required GitHub Secrets

| Secret Name       | Description                         |
| ----------------- | ----------------------------------- |
| `DOCKER_USERNAME` | Docker Hub username                 |
| `DOCKER_PASSWORD` | Docker Hub password or access token |
| `GKE_SA_KEY`      | GCP Service Account JSON key        |

## 📡 Accessing the App

The application is accessible via the Ingress-Nginx Controller public IP.
To enable HTTPS, you need a domain name and TLS certificates (e.g., via cert-manager and Let's Encrypt).

##🌐 Re-deploying After Project Deletion in GCP

If you delete the GCP project (and thus the cluster), redeployment steps are:
  - Create a new GCP project.
  - Create a GKE cluster in the new project.
  - Update GitHub Actions secrets with the new service account key.
  - Push a new commit to main to trigger redeployment.
