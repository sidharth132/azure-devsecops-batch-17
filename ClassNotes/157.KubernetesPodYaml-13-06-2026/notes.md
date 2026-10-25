# Class Notes: Writing Pod YAML Manifests & Multi-Container Pods 📦

**Date:** 13-06-2026  
**Topic:** The 4 Root Fields of Kubernetes Manifests, Spec Exploration, and Port Forwarding

---

## 📌 1. The 4 Mandatory Top-Level Fields

Every Kubernetes manifest requires four root keys:

```yaml
# 1. Version of Kubernetes API holding this resource
apiVersion: v1

# 2. Type of resource being created
kind: Pod

# 3. Metadata identifying the object
metadata:
  name: tinku-app-pod
  namespace: default
  labels:
    app: frontend
    env: dev

# 4. Desired state specification
spec:
  containers:
    - name: nginx-container
      image: nginx:alpine
      ports:
        - containerPort: 80
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

---

## 🛠️ 2. Port-Forwarding for Local Verification

To test pod endpoints locally without exposing public services:

```bash
# Forwards local port 9090 to pod containerPort 80
kubectl port-forward pod/tinku-app-pod 9090:80
```
* Open `http://localhost:9090` in your local browser to interact with the pod running inside AKS!

---

## 💡 Key Takeaways
1. Memorize the four root fields: `apiVersion`, `kind`, `metadata`, and `spec`.
2. Always declare `resources.requests` and `resources.limits` to ensure the Scheduler places pods safely.
