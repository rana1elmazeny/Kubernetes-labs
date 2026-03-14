# Kubernetes Nginx Pod Lab 🚀

## 📖 Project Overview

This lab demonstrates the basic workflow of deploying and managing a simple web application in a Kubernetes cluster.

The goal of this project is to verify cluster access and understand the fundamental Kubernetes concepts including:

* Running Pods
* Imperative vs Declarative configuration
* Pod inspection and debugging
* Exposing applications using Kubernetes Services
* Verifying connectivity inside the cluster

The application used in this lab is a simple **Nginx web server** serving a custom HTML page.

---

# 🧱 Project Structure

```
.
├── index.html
├── pod.yaml
├── service.yaml
└── README.md
```

| File         | Description                                     |
| ------------ | ----------------------------------------------- |
| index.html   | Custom webpage served by the Nginx container    |
| pod.yaml     | Declarative configuration for the Nginx Pod     |
| service.yaml | Kubernetes Service to expose the Pod internally |
| README.md    | Project documentation                           |

---

# ⚙️ Lab Tasks

This lab includes the following steps:

### 1️⃣ Run a Pod using Imperative Command

Create a simple Nginx Pod using a direct kubectl command.

```bash
kubectl run nginx-pod --image=nginx:latest
```

Verify the Pod:

```bash
kubectl get pods
```

---

### 2️⃣ Deploy a Pod using Declarative YAML

Create a Pod using a YAML manifest (`pod.yaml`) that mounts a custom `index.html` file inside the Nginx container.

Apply the configuration:

```bash
kubectl apply -f pod.yaml
```

Verify the Pod:

```bash
kubectl get pods
```

---

### 3️⃣ Inspect the Pod

Check the Pod status and lifecycle phases.

```bash
kubectl get pods
kubectl describe pod nginx-lab
kubectl logs nginx-lab
```

Watch lifecycle changes:

```bash
kubectl get pods -w
```

---
Sunday, 15 March 2026 

### 4️⃣ Expose the Pod with a Service

Create a **ClusterIP Service** to allow internal communication within the cluster.

Apply the service configuration:

```bash
kubectl apply -f service.yaml
```

Verify the service:

```bash
kubectl get services
```

---

### 5️⃣ Test the Application

Enter the container:

```bash
kubectl exec -it nginx-lab -- /bin/bash
```

Check the HTML content:

```bash
cat /usr/share/nginx/html/index.html
```

Test the service endpoint:

```bash
curl nginx-service
```

Expected output:

```
Hello from My First Lab
```

---

# 🧠 Concepts Covered

This lab introduces several key Kubernetes concepts:

* Pods
* Containerized applications
* Imperative vs Declarative management
* Pod Lifecycle
* kubectl debugging commands
* Kubernetes Services (ClusterIP)
* Internal DNS communication

---

# 🔍 Useful Commands

| Command                          | Purpose                       |
| -------------------------------- | ----------------------------- |
| `kubectl get pods`               | List running pods             |
| `kubectl describe pod <pod>`     | View detailed pod information |
| `kubectl logs <pod>`             | Check container logs          |
| `kubectl exec -it <pod> -- bash` | Access container shell        |
| `kubectl get services`           | List services                 |

---

# 🎯 Learning Outcome

After completing this lab, you should be able to:

* Deploy containers to Kubernetes
* Understand the Pod lifecycle
* Expose applications using Kubernetes Services
* Debug running containers using kubectl
* Validate service connectivity within a cluster

---

# 🛠 Technologies Used

* Kubernetes
* Docker Containers
* Nginx
* kubectl CLI

---

# 👩‍💻 Author

**Rana Elmazeny**

DevOps Engineer in progress 🚀
---
