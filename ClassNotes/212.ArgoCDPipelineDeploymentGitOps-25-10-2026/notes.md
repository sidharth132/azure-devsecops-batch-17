# Class Notes: The Complete End-to-End GitOps CI/CD Production Pipeline 🚀

**Date:** 25-10-2026  
**Topic:** Full Production Loop: Application CI, Manifest Git Update, ArgoCD Sync, and Live Verification

---

## 📌 1. The Complete GitOps Production Architecture

The ultimate enterprise DevOps workflow uniting developer code, CI automation, and GitOps delivery:

```
 1. [ Developer commits code ] ----> [ App Repo: axion-ui ]
                                             |
                                             v
 2. [ Azure CI Pipeline ] -------------------+
    ├── Runs unit tests & SonarQube scan
    ├── Builds Docker image: axionacr.azurecr.io/axion-ui:v2.5
    └── Pushes image to Azure Container Registry (ACR)
            |
            v (Clones Config Repo)
 3. [ Updates Config Repo: k8s-manifests ]
    └── Updates values.yaml: image.tag = "v2.5"
    └── Automatically commits & pushes to Git!
            |
            v (Webhook / 3-minute poll)
 4. [ ArgoCD Controller inside AKS ]
    ├── Detects new commit in k8s-manifests repo
    ├── State becomes: OutOfSync
    └── Executes Automated Sync!
            |
            v
 5. [ AKS Cluster ]
    └── Performs zero-downtime rolling update with new v2.5 image!
    └── State becomes: Synced & Healthy ✅
```

---

## 💡 Key Takeaways
1. Separate application source code repositories from infrastructure manifest repositories for secure GitOps.
2. CI pipelines only build images and update configuration repositories; ArgoCD exclusively manages cluster deployments.
