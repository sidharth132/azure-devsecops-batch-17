# Class Notes: Kubernetes Services: ClusterIP, NodePort & LoadBalancer 🌐

**Date:** 26-07-2026  
**Topic:** Service Types, Label Selectors, Endpoints, and Kube-Proxy Traffic Routing

---

## 📌 1. Why Do We Need Services?

**"Pods are ephemeral and their IP addresses change every time they recreate."**  
A **Service** provides a stable, permanent IP address and DNS name pointing to a dynamic pool of pods:

```
 [ Client Request to Service IP: 10.0.100.50 ]
                       |
                       v
       [ Kubernetes Service (frontend-svc) ]
         Selector: app = frontend
                       |
       +---------------+---------------+
       |                               |
       v                               v
 [ Pod 1 (10.244.0.12) ]     [ Pod 2 (10.244.1.8) ]
```

---

## ⚖️ 2. The 3 Primary Service Types

| Service Type | Scope | Description | Use Case |
| :--- | :--- | :--- | :--- |
| **`ClusterIP`** (Default) | Internal Only | Allocates a virtual cluster-internal IP accessible only within the cluster | Backend APIs, internal databases |
| **`NodePort`** | Cluster + Node IP | Opens a dedicated port (30000–32767) on EVERY worker node's physical IP | Direct node access, dev environments |
| **`LoadBalancer`** | Public / External | Integrates with cloud provider (Azure) to provision a real cloud Load Balancer with a Public IP | Public-facing applications |

---

## 🔗 3. Selectors & Endpoints

* A service connects to pods using `.spec.selector.app = frontend`.
* Kubernetes automatically creates an **Endpoints** object (`kubectl get endpoints`) tracking the live IPs of all matching pods.
* If a pod fails its readiness probe, Kubernetes temporarily removes its IP from the Endpoints list so it receives no user traffic.

---

## 💡 Key Takeaways
1. Use `ClusterIP` for internal microservices and `LoadBalancer` (or Ingress) for external traffic.
2. The selector on the Service must precisely match the labels on the Deployment's template.
