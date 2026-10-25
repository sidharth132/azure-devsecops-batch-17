# Class Notes: Complete Kubernetes CI/CD Pipeline with SonarQube & Security ☸️

**Date:** 03-10-2026  
**Topic:** End-to-End DevSecOps Pipeline: SonarQube, Image Building, Manifest Replacement, and Cluster Deployment

---

## 📌 1. The DevSecOps Pipeline Flow

```
 [ Azure Repos ]
        |
        v
 [ Azure DevOps Pipeline ]
   ├── Stage 1: Static Code Analysis (SonarQube Quality Gate)
   ├── Stage 2: Security Linting (Trivy / tfsec)
   ├── Stage 3: Container Build & Push to ACR
   └── Stage 4: Continuous Deployment to AKS
         ├── Substitute image tag in deployment.yaml
         └── kubectl apply -f deployment.yaml -f service.yaml
```

---

## ⚙️ 2. Dynamic Image Tag Injection

Never commit hardcoded container image tags into Git manifests. In the CD stage:

```bash
# Replaces IMAGE_TAG placeholder with actual CI Build ID
sed -i 's|IMAGE_TAG|'$(Build.BuildId)'|g' k8s/deployment.yaml

# Apply updated manifest to AKS
kubectl apply -f k8s/deployment.yaml -n production
```

---

## 💡 Key Takeaways
1. Break pipelines into clear quality gates: Fail builds if SonarQube detects high-severity vulnerabilities.
2. Dynamically inject immutable build identifiers during release stages.
