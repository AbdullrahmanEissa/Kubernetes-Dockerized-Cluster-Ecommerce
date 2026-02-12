# 🚀 Dockerized MERN Stack E-Commerce App on Kubernetes

A fully containerized MERN stack e-commerce application deployed on Kubernetes with proper service discovery, configuration management, and ingress routing.

This project demonstrates real-world Kubernetes concepts including:

* Deployments for lifecycle management
* Services for internal networking and service discovery
* ConfigMaps for externalized configuration
* Ingress for external traffic routing
* Resource limits for container stability

<img width="1920" height="918" alt="Screenshot from 2026-02-12 11-40-53" src="https://github.com/user-attachments/assets/0648f5ab-03b8-47ef-907b-e0a03471ed26" />
<img width="1920" height="918" alt="Screenshot from 2026-02-12 11-40-59" src="https://github.com/user-attachments/assets/5a1eed35-1df0-48b8-aeb3-5fe2d276a72e" />
<img width="1920" height="918" alt="Screenshot from 2026-02-12 11-41-04" src="https://github.com/user-attachments/assets/77595eaf-6e6a-48ca-94e6-0865f218b664" />

---

## 🧱 Architecture Overview

```
Browser
   ↓
Ingress (ecommerce-app.local)
   ↓
Frontend Service
   ↓
Frontend Pod (Vite)
   ↓
Backend Service
   ↓
Backend Pod (Node.js / Express)
   ↓
MongoDB Service       Redis Service
   ↓                      ↓
MongoDB Pod          Redis Pod
```

Kubernetes handles:

* Internal DNS resolution
* Service-to-service communication
* Pod lifecycle and restarts
* Load balancing via Services

---

## 📦 Tech Stack

* **Frontend**: React + Vite
* **Backend**: Node.js + Express
* **Database**: MongoDB
* **Cache**: Redis
* **Containerization**: Docker
* **Orchestration**: Kubernetes (Minikube)
* **Ingress Controller**: NGINX Ingress

---

## 📁 Project Structure

```
.
├── Dockerfile-front
├── Dockerfile-end
├── docker-compose.yml
├── kubernetes/
│   ├── frontend/
│   │   ├── deployment.yml
│   │   └── service.yml
│   ├── backend/
│   │   ├── deployment.yml
│   │   ├── service.yml
│   │   └── configmap.yml
│   ├── mongodb/
│   │   ├── deployment.yml
│   │   └── service.yml
│   ├── redis/
│   │   ├── deployment.yml
│   │   └── service.yml
│   └── ingress.yml
```

---

## 🐳 Local Docker Setup (Optional)

Build and run with Docker Compose:

```bash
docker-compose up --build
```

Frontend: [http://localhost:5173](http://localhost:5173)
Backend: [http://localhost:5000](http://localhost:5000)

---

## ☸️ Kubernetes Deployment (Minikube)

### 1️⃣ Start Minikube

```bash
minikube start
minikube addons enable ingress
```

### 2️⃣ Use Minikube Docker Daemon

```bash
eval $(minikube docker-env)
```

Build images inside Minikube:

```bash
docker build -t dockerized-mernstack-ecommerce-app-frontend -f Dockerfile-front .
docker build -t dockerized-mernstack-ecommerce-app-backend -f Dockerfile-end .
```

---

### 3️⃣ Apply Kubernetes Resources

```bash
kubectl apply -f kubernetes/
```

Verify:

```bash
kubectl get pods
kubectl get services
kubectl get ingress
```

---

### 4️⃣ Configure Local DNS

Get Minikube IP:

```bash
minikube ip
```

Add to `/etc/hosts`:

```
<minikube-ip> ecommerce-app.local
```

---

### 5️⃣ Access the App

Open in browser:

```
http://ecommerce-app.local
```

---

## 🔧 Configuration Management

Environment variables are externalized using a **ConfigMap**:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: backend-config
data:
  PORT: "5000"
  MONGO_URI: "mongodb://mongodb:27017/ecomdb"
  UPSTASH_REDIS_URL: "redis://redis:6379"
  CLIENT_URL: "http://frontend:5173"
  NODE_ENV: "development"
```

This ensures:

* No hardcoded secrets inside containers
* Reusable Docker images
* Clean separation between code and configuration

---

## 🌐 Service Discovery

Kubernetes internal DNS enables service-to-service communication:

```
mongodb.default.svc.cluster.local
redis.default.svc.cluster.local
```

Pods communicate using service names:

```
mongodb:27017
redis:6379
backend:5000
frontend:5173
```

No static IP dependency required.

---

## ⚙️ Resource Management

Example resource limits:

```yaml
resources:
  limits:
    memory: "512Mi"
    cpu: "500m"
```

Prevents:

* OOM crashes
* Node starvation
* Uncontrolled memory usage

---

## 📈 What This Project Demonstrates

* Real Kubernetes deployment workflow
* Debugging CrashLoopBackOff and OOMKilled errors
* Migrating from docker-compose to Kubernetes
* Service abstraction and stable networking
* Ingress routing with host-based rules

This is not just a containerized app — it is a structured Kubernetes deployment.

---

## 🔮 Future Improvements

* Replace Vite dev server with production NGINX build
* Move sensitive values to Kubernetes Secrets
* Add PersistentVolume for MongoDB
* Implement Horizontal Pod Autoscaler
* Add CI/CD pipeline (GitHub Actions)
* Convert to Helm chart

---

## 🧠 Learning Outcomes

By completing this deployment:

* Understood Deployments vs Services
* Implemented ConfigMaps correctly
* Verified internal DNS resolution
* Debugged real Kubernetes issues
* Exposed application using Ingress

---

## 👨‍💻 Author

Built and deployed as a hands-on Kubernetes learning project focused on real-world architecture and operational understanding.

<h1 align="center">E-Commerce Store 🛒</h1>

![Demo App](/frontend/public/screenshot-for-readme.png)

[Video Tutorial on Youtube](https://youtu.be/sX57TLIPNx8)

About This Course:

-   🚀 Project Setup
-   🗄️ MongoDB & Redis Integration
-   💳 Stripe Payment Setup
-   🔐 Robust Authentication System
-   🔑 JWT with Refresh/Access Tokens
-   📝 User Signup & Login
-   🛒 E-Commerce Core
-   📦 Product & Category Management
-   🛍️ Shopping Cart Functionality
-   💰 Checkout with Stripe
-   🏷️ Coupon Code System
-   👑 Admin Dashboard
-   📊 Sales Analytics
-   🎨 Design with Tailwind
-   🛒 Cart & Checkout Process
-   🔒 Security
-   🛡️ Data Protection
-   🚀Caching with Redis
-   ⌛ And a lot more...

### Setup .env file

```bash
PORT=5000
MONGO_URI=your_mongo_uri

UPSTASH_REDIS_URL=your_redis_url

ACCESS_TOKEN_SECRET=your_access_token_secret
REFRESH_TOKEN_SECRET=your_refresh_token_secret

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

STRIPE_SECRET_KEY=your_stripe_secret_key
CLIENT_URL=http://localhost:5173
NODE_ENV=development
```

### Run this app locally

```shell
npm run build
```

### Start the app

```shell
npm run start
```
