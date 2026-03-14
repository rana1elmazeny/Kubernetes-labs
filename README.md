# Kubernetes Lab 3

## Multi-Tenancy with Namespaces and Internal Routing

This lab demonstrates how to implement **environment isolation and internal communication** between applications in Kubernetes using **Namespaces and ClusterIP Services**.

The goal is to simulate two different environments (**Development** and **Staging**) within the **same Kubernetes cluster** while ensuring that applications in each environment communicate only with their own services.

---

# Architecture Overview

In this lab we deploy a **two-tier application** consisting of:

* **Frontend**: Nginx container
* **Backend**: Python HTTP server

Each environment contains its own isolated stack.

Cluster Structure:

```
Kubernetes Cluster
│
├── dev namespace
│   ├── frontend-pod
│   ├── backend-pod
│   └── backend-service (ClusterIP)
│
└── staging namespace
    ├── frontend-pod
    ├── backend-pod
    └── backend-service (ClusterIP)
```

Each frontend communicates with the backend **via Kubernetes Service DNS**.

Example:

```
frontend → backend-service:8080
```

---

# Project Structure

```
k8s-lab3/
.
├── backend.py
├── dev-environment.yaml
├── Dockerfile.backend
├── Dockerfile.frontend
├── index.html
├── nginx.conf
├── README.md
├── Screenshots
└── staging-environment.yaml

```

---

# Docker Images Build

Backend Image

```
docker build -f Dockerfile.backend -t backend-app:latest .
```

Frontend Image

```
docker build -f Dockerfile.frontend -t frontend-app:latest .
```

If using **Minikube**, load the images into the cluster:

```
minikube image load backend-app:latest
minikube image load frontend-app:latest
```

---

# Deploy Environments

Apply the Development environment configuration:

```
kubectl apply -f dev-environment.yaml
```

Apply the Staging environment configuration:

```
kubectl apply -f staging-environment.yaml
```

---

# Verify Running Pods and Services

Check the **Development environment**

```
kubectl get pods,svc -n dev
```

Screenshot:

*(Add screenshot here)*

---

Check the **Staging environment**

```
kubectl get pods,svc -n staging
```

Screenshot:

*(Add screenshot here)*

---

# Access the Application

Use port-forwarding to access the frontend service from the browser.

```
kubectl port-forward pod/frontend-pod 8082:80 -n dev
```

Open the browser:

```
http://localhost:8082
```

The web interface allows testing backend connectivity through the following endpoints:

* `/`
* `/health`
* `/info`

Screenshot:
1- Ensure that the status of pods and services in dev and staging namespace are running.
![Dev Namespace](screenshots/dev.png)

![Dev Namespace](screenshots/staging.png)
 
 
2- Execute port-forward to can access the app from the browser
![Dev Namespace](screenshots/port-forward.png)

3- open your browser at localhost:8082 then take the screenshot
![Dev Namespace](screenshots/FRONTEND.png)

![Dev Namespace](screenshots/FRONTEND2.png)

---

# Environment Isolation Test

Each namespace runs its **own isolated version** of the application stack.

The frontend in each namespace communicates only with its **local backend-service**, ensuring proper **environment separation** even though both environments share the same Kubernetes cluster.

The application interface also displays:

* Pod Name
* Namespace

This helps verify that requests are handled by the correct environment.

---

# Technologies Used

* Kubernetes
* Docker
* Nginx
* Python HTTP Server
* Minikube
* kubectl

---

# Author

**Rana Elmazeny**

DevOps Trainee

