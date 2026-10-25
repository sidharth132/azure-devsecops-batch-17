# Class Notes: End-to-End Kubernetes Pipeline: Code to Cluster Deployment 🚢

**Date:** 17-10-2026  
**Topic:** Continuous Delivery with `kubectl apply`, Kubeconfig Authentication, and Service Verification

---

## 📌 1. Pipeline Architecture

Taking `axion-ui` from source code to running pods on AKS:

```
 [ Git Repo: axion-ui ]
        |
        v
 [ Azure CI Agent ]
   ├── 1. Docker Build & Push to ACR
   │
   └── 2. Authenticate to AKS (Kubeconfig / Service Connection)
   │
   └── 3. Deploy to Cluster:
         kubectl apply -f k8s/deployment.yaml
         kubectl apply -f k8s/service.yaml
   │
   └── 4. Rollout Verification:
         kubectl rollout status deployment/axion-ui -n default
```

---

## 💡 Key Takeaways
1. Always follow `kubectl apply` with `kubectl rollout status` to guarantee the pipeline fails if new pods cannot start.
2. Clean separation of build tasks and release tasks ensures reproducible deployments.
