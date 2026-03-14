# Kubernetes Lab 2: Pod Troubleshooting & Static Pods 🛠️

## 📖 Project Overview

This lab focuses on two important Kubernetes operational skills:

* Running **Static Pods** directly on a node using the kubelet
* Practicing **Pod troubleshooting techniques**

In real production environments, engineers often need to diagnose misconfigured workloads and ensure that critical node-level services continue running even when the control plane is unavailable.

This lab simulates such a scenario by deploying a static logging agent and debugging a broken pod manifest.

---

# 🧱 Project Structure

```
.
├── fail-pod.yaml
├── logger-static.yaml
└── README.md
```

| File               | Description                                           |
| ------------------ | ----------------------------------------------------- |
| fail-pod.yaml      | Broken pod manifest used for troubleshooting practice |
| logger-static.yaml | Static pod manifest for the logging container         |
| README.md          | Documentation for the lab                             |

---

# ⚙️ Lab Tasks

## 1️⃣ Access the Control Plane Node

Connect to the Kubernetes node using:

```bash
minikube ssh
```

Identify the directory used by the kubelet to load **static pod manifests**:

```
/etc/kubernetes/manifests
```

All YAML files placed in this directory are automatically started as pods by the kubelet.

---

# 2️⃣ Create a Static Pod

Create a static pod manifest for a logging container.

Example:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: logger-static

spec:
  containers:
  - name: logger
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
```

Save the file in:

```
/etc/kubernetes/manifests/logger-static.yaml
```

The kubelet will automatically detect the manifest and start the pod.

Verify it from your cluster:

```bash
kubectl get pods -o wide
```

Static pods typically appear with the format:

```
<pod-name>-<node-name>
```

---

# 3️⃣ Deploy the Broken Pod

Apply the provided broken manifest:

```bash
kubectl apply -f fail-pod.yaml
```

Check the pod status:

```bash
kubectl get pods
```

The pod will likely enter a failing state such as:

```
CrashLoopBackOff
```

---

# 4️⃣ Troubleshoot the Pod

Use Kubernetes debugging commands to identify the issue.

Inspect the pod:

```bash
kubectl describe pod fail-pod
```

Check container logs:

```bash
kubectl logs fail-pod
```

If the container keeps restarting:

```bash
kubectl logs fail-pod --previous
```

Review the events section and logs to identify the misconfiguration and correct it.

---

# 5️⃣ Add Ephemeral Storage

Update the pod to include temporary storage using an **emptyDir volume**.

Example:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: fixed-pod

spec:
  containers:
  - name: test-container
    image: busybox
    command: ["sh", "-c", "echo hello > /data/file.txt && sleep 3600"]

    volumeMounts:
    - name: data-volume
      mountPath: /data

  volumes:
  - name: data-volume
    emptyDir: {}
```

Deploy the updated manifest:

```bash
kubectl apply -f fail-pod.yaml
```

---

# 🧪 Verify Ephemeral Storage

Access the running container:

```bash
kubectl exec -it fixed-pod -- sh
```

Check the created file:

```bash
cat /data/file.txt
```

Expected output:

```
hello
```

---
# 🧠 Concepts Covered

This lab introduces several operational Kubernetes concepts:

* Static Pods
* kubelet-managed workloads
* Pod lifecycle and restart behavior
* Debugging failing pods
* Container logs and events
* Ephemeral storage with emptyDir volumes

---

# 🔍 Useful Troubleshooting Commands

| Command                         | Purpose                              |
| ------------------------------- | ------------------------------------ |
| `kubectl get pods`              | List running pods                    |
| `kubectl describe pod <pod>`    | Inspect pod configuration and events |
| `kubectl logs <pod>`            | View container logs                  |
| `kubectl logs <pod> --previous` | View logs from a crashed container   |
| `kubectl exec -it <pod> -- sh`  | Access the container shell           |

---

# 🎯 Learning Outcome

After completing this lab, you should be able to:

* Deploy and manage **Static Pods**
* Understand how kubelet manages node-level workloads
* Diagnose pod failures using logs and events
* Fix misconfigured containers
* Implement ephemeral storage for temporary data

---

# 🛠 Technologies Used

* Kubernetes
* kubectl CLI
* Minikube
* BusyBox Container

---

# 👩‍💻 Author

**Rana Elmazeny**

DevOps Engineer in progress 🚀

---
