
# 🛠️ TWSThreeTierApp - 3-Tier Web Application with EKS, Docker, and Kubernetes

This project demonstrates a 3-tier web application architecture deployed on **Amazon EKS** using **Docker**, **Kubernetes**, **MongoDB**, and **ECR**. It includes a **React frontend**, **Node.js backend**, and a **MongoDB database**, all containerized and orchestrated with Kubernetes.


## 🧱 Architecture Diagram

![Architecture](https://github.com/gopu2311/TWSThreeTierApp/blob/main/Screenshot%202025-07-09%20144242.png)

## 📦 Project Structure

```
TWSThreeTierApp/
├── backend/               # Node.js backend (Express + MongoDB)
│   ├── models/
│   ├── routes/
│   ├── db.js
│   ├── index.js
│   ├── Dockerfile
│   └── ...
├── frontend/              # React frontend
│   ├── public/
│   ├── src/
│   ├── Dockerfile
│   └── ...
├── Kubernetes-Manifests-file/
│   ├── Backend/
│   ├── Database/
│   ├── Frontend/
│   └── ingress.yaml
```

## 🧰 Tech Stack

- **Frontend**: React.js
- **Backend**: Node.js (Express)
- **Database**: MongoDB
- **Containerization**: Docker
- **Image Registry**: Amazon ECR
- **Orchestration**: Kubernetes
- **Deployment**: AWS EKS with `eksctl`
- **Ingress**: NGINX + AWS ALB

## 🚀 Getting Started

### 1️⃣ Prerequisites

- AWS CLI configured
- eksctl installed
- kubectl installed
- Docker installed

### 2️⃣ Clone the Repository

```bash
git clone https://github.com/gopu2311/TWSThreeTierApp.git
cd TWSThreeTierApp
```

### 3️⃣ Create EKS Cluster

```bash
eksctl create cluster   --name three-tier-cluster   --region us-west-2   --node-type t2.medium   --nodes-min 2   --nodes-max 2
```

### 4️⃣ Update kubeconfig

```bash
aws eks update-kubeconfig   --region us-west-2   --name three-tier-cluster
```

### 5️⃣ Verify EKS Nodes

```bash
kubectl get nodes
```

## 🐳 Docker + Amazon ECR Instructions

### Authenticate Docker to ECR

```bash
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin <your_account_id>.dkr.ecr.us-west-2.amazonaws.com
```

### Build, Tag, and Push Images

```bash
# Backend
cd backend
docker build -t backend .
docker tag backend <your_account_id>.dkr.ecr.us-west-2.amazonaws.com/backend
docker push <your_account_id>.dkr.ecr.us-west-2.amazonaws.com/backend

# Frontend
cd ../frontend
docker build -t frontend .
docker tag frontend <your_account_id>.dkr.ecr.us-west-2.amazonaws.com/frontend
docker push <your_account_id>.dkr.ecr.us-west-2.amazonaws.com/frontend
```

## ☸️ Kubernetes Deployment

Apply the Kubernetes manifests:

```bash
kubectl apply -f Kubernetes-Manifests-file/Database/
kubectl apply -f Kubernetes-Manifests-file/Backend/
kubectl apply -f Kubernetes-Manifests-file/Frontend/
kubectl apply -f Kubernetes-Manifests-file/ingress.yaml
```

## 🌐 Accessing the App

- After deploying, use `kubectl get ingress` to get the ALB URL or IP address.
- The application will be accessible via the ALB configured through the NGINX Ingress Controller.

## 👨‍💻 Author

- **Gopendra Rajput** – [GitHub](https://github.com/gopu2311)

## 📜 License

This project is licensed under the MIT License.

---

> 🚨 Replace `<your_account_id>` with your actual AWS account ID before pushing to ECR.
