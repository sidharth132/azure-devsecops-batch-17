# Class Notes: Axion Systems: Multi-Service Ingress & Subdomain Routing 🌐

**Date:** 22-08-2026  
**Topic:** Subdomain Routing with Ingress, AGIC Architecture, and Multi-Microservice Exposure

---

## 📌 1. Architecture: Unified Ingress with Subdomains

Routing multiple microservices through a single Application Gateway using DNS subdomains:

```
 [ Internet ]
      |
      v
 [ Azure Application Gateway (AGIC) ] (Public IP: 135.237.18.16)
      ├── Host: axion.vistara.website        ----> routes to axion-ui service (Port 80)
      ├── Host: api.vistara.website          ----> routes to axion-query service (Port 5000)
      └── Host: ingest.vistara.website       ----> routes to axion-ingestion service (Port 5001)
```

---

## 📜 2. Host-Based Ingress Rule

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: axion-ingress
  annotations:
    appgw.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  ingressClassName: azure-application-gateway
  rules:
    - host: axion.vistara.website
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: axion-ui
                port:
                  number: 80
    - host: api.vistara.website
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: axion-query
                port:
                  number: 5000
```

---

## 💡 Key Takeaways
1. Host-based routing enables clean microservice API scoping without polluting URL path namespaces.
2. AGIC configures backend pools dynamically by watching pod IP endpoints.
