# Class Notes: Ingress Controllers: NGINX Ingress & AGIC Deep Dive ⚙️

**Date:** 02-08-2026  
**Topic:** Ingress Controllers, NGINX Ingress vs Azure AGIC, Cert-Manager TLS, and Reverse Proxying

---

## 📌 1. Architecture: How Ingress Controllers Work

An Ingress Controller is a specialized reverse proxy pod watching the Kubernetes API:

```
 [ External Client ]
         |
         v
 [ Azure Load Balancer / Public IP ]
         |
         v
 [ Ingress Controller Pod (NGINX / AGIC) ]
   └── Constantly queries API server for Ingress rules
   └── Dynamically reloads routing configurations when services/endpoints change
         |
         +-------------------------------------+
         |                                     |
         v                                     v
 [ Frontend Pod (10.244.0.89:80) ]   [ Netflix Pod (10.244.0.73:80) ]
```

---

## ⚖️ 2. NGINX Ingress Controller vs. Azure Application Gateway Ingress Controller (AGIC)

* **NGINX Ingress Controller:** Runs inside AKS as a standard deployment. Traffic passes through a basic Layer 4 Azure Load Balancer into the NGINX pods, which reverse-proxy traffic to target pods.
* **AGIC (Application Gateway Ingress Controller):** Runs as a pod in AKS that translates Ingress rules directly into Azure Application Gateway configuration via Azure ARM API.
  * Eliminates intermediate proxy hop by routing directly from Application Gateway into pod private IPs using Azure CNI networking!

---

## 💡 Key Takeaways
1. Choose AGIC for native Azure WAF security and direct pod routing; choose NGINX Ingress for multi-cloud consistency.
2. Combine Ingress with `cert-manager` for automated Let's Encrypt SSL/TLS certificate renewal.
