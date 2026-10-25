# Class Notes: Kubernetes Ingress: Layer 7 Routing & Host-Based Traffic 🚦

**Date:** 01-08-2026  
**Topic:** Why Ingress? Ingress Resources vs Controllers, Path-Based and Host-Based Routing

---

## 📌 1. Why Ingress over multiple LoadBalancer Services?

```
 WITHOUT INGRESS (Expensive & Unmanageable):
   Frontend Service (Type: LoadBalancer) -----> Cloud Public IP 1 ($$$ per IP)
   Backend API Service (Type: LoadBalancer)  -----> Cloud Public IP 2 ($$$ per IP)
   Analytics Service (Type: LoadBalancer)   -----> Cloud Public IP 3 ($$$ per IP)

 WITH INGRESS (Cost-Effective & Unified):
   Single Cloud Public IP / Load Balancer
                 |
                 v
   [ INGRESS CONTROLLER (Layer 7 Reverse Proxy) ]
                 |
                 |---> app.company.com/api    ----> routes to Backend Service (ClusterIP)
                 |---> app.company.com/        ----> routes to Frontend Service (ClusterIP)
                 |---> docs.company.com/       ----> routes to Docs Service (ClusterIP)
```

---

## 📜 2. Ingress Resource Manifest

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: main-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
    - host: app.devopsinsiders.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend-svc
                port:
                  number: 80
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: backend-svc
                port:
                  number: 5000
```

---

## 💡 Key Takeaways
1. Ingress operates at **Layer 7 (HTTP/HTTPS)**, enabling path-based routing, host-based routing, and SSL termination.
2. An Ingress resource does nothing on its own; it requires an **Ingress Controller** to execute routing.
