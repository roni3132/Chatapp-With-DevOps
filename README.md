# 🗨️ ChatApp – Kubernetes Deployment

This project demonstrates a complete deployment of a **Chat Application** (Frontend + Backend + MongoDB) on **Kubernetes (Minikube)**.

---

## 📁 Project Structure

k8s/
│
├── namespace.yml
├── mongo-pv.yml
├── mongo-pvc.yml
├── mongodb-deployment.yml
├── backend-deployment.yml
├── backend-service.yml
├── frontend-deployment.yml
├── frontend-service.yml
└── README.md

---

## 🚀 Components

| Component | Description | Type | Port |
|------------|--------------|------|------|
| **Frontend** | React-based chat UI | `ClusterIP` | `80` |
| **Backend** | Node.js / Express API server | `ClusterIP` | `5001` |
| **MongoDB** | NoSQL database | `ClusterIP` | `27017` |

---

## ⚙️ Prerequisites

Ensure you have the following installed:

- 🐳 Docker  
- ☸️ Minikube  
- 🔧 kubectl  

---

## 🏗️ Deployment Steps

### 1. Start Minikube
``` bash
minikube start
cd k8s 
```
### 2. Apply Namespace
```bash
kubectl apply -f namespace.yml
```
### 3. Apply All Resources
```bash
kubectl apply -f .
```

This creates:
Deployments for backend, frontend, MongoDB
Services for internal communication
Persistent Volume and Claim for MongoDB

### 🔍 Verify Deployment
```bash
kubectl get all -n chatapp
```

NAME                                      READY   STATUS    RESTARTS   AGE
pod/chatapp-backend-xxxxxx                1/1     Running   0          10s
pod/chatapp-frontend-xxxxxx               1/1     Running   0          10s
pod/mongodb-deployment-xxxxxx             1/1     Running   0          10s

NAME               TYPE        CLUSTER-IP       PORT(S)
service/backend    ClusterIP   10.110.50.164    5001/TCP
service/frontend   ClusterIP   10.101.48.8      80/TCP
service/mongo      ClusterIP   10.111.24.158    27017/TCP


### 🌐 Accessing the App
Since the services are ClusterIP, expose them locally via port-forwarding.
## 1️⃣ Backend
```bash
kubectl port-forward service/backend -n chatapp 5001:5001 &
```
## 2️⃣ Frontend
```bash
sudo -E kubectl port-forward service/frontend -n chatapp 80:80
```
Now open:
Frontend: http://localhost:81
Backend API: http://localhost:5001

### 💾 MongoDB Access
You can access MongoDB inside the cluster:

```bash 
kubectl exec -it deployment/mongodb-deployment -n chatapp -- bash
mongosh mongodb://localhost:27017
```

### 🧹 Cleanup
Delete everything using:
```bash 
kubectl delete ns chatapp
```