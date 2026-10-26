# Class Notes: Integrating GitFlow with Multi-Stage Azure Pipelines 🏗️

**Date:** 27-09-2026  
**Topic:** Multi-Stage Pipelines, Environment Gates, Pull Request Validation, and Release Orchestration

---

## 📌 1. Complete Multi-Stage Pipeline Architecture

```
 STAGE 1: Build & Security Scanning (Runs on PR & Merge)
   ├── Code Linting
   ├── Trivy Container Vulnerability Scan
   └── Docker Build & Push to ACR
            |
            v
 STAGE 2: Deploy to DEV Environment (Runs on develop branch)
   └── Helm upgrade --install in dev namespace on AKS
            |
            v
 STAGE 3: Deploy to QA / Staging (Runs on release branch)
   └── Automated integration tests & smoke tests
            |
            v (Manual Business Approval Gate)
 STAGE 4: Deploy to PRODUCTION (Runs on main branch with Tag)
   └── Zero-downtime rolling update in prod namespace on AKS
```

---

## 💡 Key Takeaways
1. Use pipeline stage dependencies (`dependsOn`) and conditions (`condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))`) to control progression.
2. Enforce manual approvals on production environments within Azure DevOps.
