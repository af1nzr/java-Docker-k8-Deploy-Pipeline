# ☕ Java Microservices on Kubernetes with GitHub Actions 🚀

This repository contains a **Java microservices application** (`ProductCatalogue`, `Shopfront`, `StockManager`) with a complete **CI/CD pipeline** using **GitHub Actions**, **Docker**, and **Kubernetes**.

---

## ✅ Features

- **Multiple Java services**
  - Product Catalogue
  - Shopfront
  - Stock Manager
- **Maven build automation**
- **Dockerized microservices**
- **GitHub Actions pipeline**
  - Builds Java apps with Maven
  - Creates & pushes Docker images to **GitHub Container Registry (GHCR)**
  - Deploys to a Kubernetes cluster
- **Kubernetes manifests**
  - Deployments & Services for each microservice

---


- Each service runs in its **own Docker container**
- Kubernetes manages **deployments** & **services**
- Images are stored in **ghcr.io/af1nzr/**

---

## 📂 Project Structure

<pre>
  ├── productcatalogue/
│ ├── src/...
│ ├── pom.xml
│ └── Dockerfile
├── shopfront/
│ ├── src/...
│ ├── pom.xml
│ └── Dockerfile
├── stockmanager/
│ ├── src/...
│ ├── pom.xml
│ └── Dockerfile
├── kubernetes/
│ ├── productcatalogue-service.yaml
│ ├── shopfront-service.yaml
│ └── stockmanager-service.yaml
└── .github/workflows/java-kubernetes-pipeline.yml
</pre>


---

## 🚀 CI/CD Pipeline

The pipeline is defined in **`.github/workflows/java-kubernetes-pipeline.yml`**.

### 1️⃣ On every `push` or `pull_request` to `main`:

✅ **Checkout code**  
✅ **Build Java microservices with Maven**  
✅ **Build Docker images**  
✅ **Push images to GHCR**  
✅ **Deploy to Kubernetes cluster**  
✅ **Verify deployments**

---

## 🔧 Setup & Requirements

### 1. Prerequisites

- **Docker**
- **Kubernetes (Minikube / Cloud Cluster)**
- **kubectl**
- **GitHub Personal Access Token (for GHCR)**

### 2. GitHub Secrets

You must set the following **GitHub Actions secrets**:

| Secret Name    | Description |
|----------------|-------------|
| `GHCR_PAT`     | Personal Access Token for GitHub Container Registry |
| `KUBE_CONFIG`  | Base64-encoded kubeconfig for your cluster |

---

## 🐳 Build & Run Locally

You can build and run services locally without Kubernetes:

```bash
# Build microservices
mvn clean install

# Build Docker image (example)
docker build -t productcatalogue:latest -f productcatalogue/Dockerfile .

# Run locally
docker run -p 8020:8020 productcatalogue:latest
```
☸️ Deploying to Kubernetes
```
1. Build & Push Images
docker build -t ghcr.io/af1nzr/productcatalogue:latest productcatalogue
docker push ghcr.io/af1nzr/productcatalogue:latest

2. Apply Kubernetes Manifests
kubectl apply -f kubernetes/productcatalogue-service.yaml
kubectl apply -f kubernetes/shopfront-service.yaml
kubectl apply -f kubernetes/stockmanager-service.yaml

3. Verify Deployments
kubectl get pods
kubectl get services
kubectl rollout status deployment/productcatalogue

```
🖥️ Deploying to Minikube (Local Dev)
If you’re using Minikube, enable Docker inside Minikube so it can see local images:
```
eval $(minikube docker-env)

# Build & deploy

docker build -t productcatalogue:latest productcatalogue
kubectl apply -f kubernetes/productcatalogue-service.yaml
```
Then access:
```
minikube service productcatalogue
```
⚡ GitHub Actions Self-Hosted Runner (Optional)

If you want GitHub Actions to deploy directly to your local Minikube:
Install a self-hosted runner on your machine
Change the workflow to runs-on: self-hosted
It will then use your local Minikube kubeconfig

🛠️ Troubleshooting

Error: kubeconfig not found?
→ Make sure KUBE_CONFIG secret is set & base64 encoded.

ImagePullBackOff?

→ Ensure Kubernetes has a GHCR pull secret:
```
kubectl create secret docker-registry ghcr-secret \
  --docker-server=ghcr.io \
  --docker-username=USERNAME \
  --docker-password=TOKEN \
  --docker-email=you@example.com
```
Minikube only works locally
→ Use self-hosted runner or manual deployment.

📜 License
This project is licensed under the MIT License.

Author
Af1n
GitHub

Don’t forget to star this repo if you like it!
```

---

## ✅ Why this README is great:
✔ Explains **what the project does**  
✔ Shows **architecture**  
✔ Includes **CI/CD pipeline details**  
✔ Gives **local & Kubernetes deployment instructions**  
✔ Explains **GitHub Actions secrets**  
✔ Includes **troubleshooting** & **Minikube notes**  

---

Do you want me to **also include GitHub Actions workflow YAML as a section inside README** (so users see pipeline definition), or keep it clean?
```
