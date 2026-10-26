# Class Notes: Kubernetes Deployments: Rolling Updates & Zero Downtime 🚀

**Date:** 25-07-2026  
**Topic:** Deployment Controller, RollingUpdate vs Recreate, Revision History, Rollbacks, and HTTP Error Codes

---

## 📌 1. Architecture: How Deployments Manage ReplicaSets

A Deployment manages the declarative rollout of pods by creating and managing underlying **ReplicaSets**:

```
 [ Deployment: frontend-deploy ]
        |
        +----(Version 1)----> [ ReplicaSet v1 ] ----> 3 Pods (Image: v1.0)
        |
   (Update Image to v2.0)
        |
        +----(Version 2)----> [ ReplicaSet v2 ] ----> Scales UP (0 -> 1 -> 2 -> 3)
                              [ ReplicaSet v1 ] ----> Scales DOWN (3 -> 2 -> 1 -> 0)
```

---

## 🔄 2. Deployment Update Strategies

### 1. `RollingUpdate` (Default - Zero Downtime)
* Gradually replaces old pods with new pods one by one.
* **`maxSurge`:** How many pods can be created above the desired replica count during rollout (e.g., 25%).
* **`maxUnavailable`:** How many pods can be unavailable during update (e.g., 25%).
* **Result:** Users experience continuous service with zero dropped connections.

### 2. `Recreate`
* All old pods are killed simultaneously before any new pods are started.
* **Result:** Temporary downtime; suitable only for testing environments or when two versions cannot share the same database schema concurrently.

---

## ⏪ 3. Revision History & Rollbacks

```bash
# Check rollout status
kubectl rollout status deployment/frontend-deploy

# View revision history
kubectl rollout history deployment/frontend-deploy

# Roll back to the previous revision immediately
kubectl rollout undo deployment/frontend-deploy

# Roll back to a specific past revision
kubectl rollout undo deployment/frontend-deploy --to-revision=2
```

---

## 🛑 4. Common HTTP Error Codes Explained

* **200 OK:** Successful request.
* **400 Bad Request:** Client syntax error.
* **401 Unauthorized:** Missing or invalid authentication token.
* **403 Forbidden:** Authenticated user lacks permission (RBAC failure).
* **404 Not Found:** Endpoint or route does not exist.
* **500 Internal Server Error:** Unhandled exception in application code.
* **502 Bad Gateway:** Reverse proxy (Ingress/NGINX) cannot reach backend pod.
* **503 Service Unavailable:** Backend pods overloaded or failing readiness probes.

---

## 💡 Key Takeaways
1. Deployments guarantee zero-downtime upgrades via `RollingUpdate` and instant rollback recovery via `kubectl rollout undo`.
2. Always monitor rollout status and configure readiness probes to prevent broken pods from serving traffic.
