# Class Notes: Continuous Integration vs. Continuous Delivery & Deployment 🚀

**Date:** 04-01-2026  
**Topic:** CI vs CD, DevSecOps Pipeline Stages, Artifact Promotion, and Release Gates

---

## 📌 1. CI vs. CD Explained

```
 [ Code Commit ]
        |
        v
 +-----------------------------------------------------------+
 | CONTINUOUS INTEGRATION (CI)                                |
 |  - Code Checkout                                          |
 |  - Linting & SAST Scanning                                |
 |  - Build / Compile Application                            |
 |  - Unit Testing & Code Coverage                           |
 |  - Package & Publish Artifact                             |
 +-----------------------------------------------------------+
        |
        v
 +-----------------------------------------------------------+
 | CONTINUOUS DELIVERY (CD)                                  |
 |  - Automated deployment to DEV and QA                     |
 |  - Manual Approval Gate / Release Approval                |
 +-----------------------------------------------------------+
        |
        v  (Approved by Release Manager)
 +-----------------------------------------------------------+
 | CONTINUOUS DEPLOYMENT (Full CD)                          |
 |  - Automatic deployment to Production with zero manual    |
 |    intervention upon passing all automated tests          |
 +-----------------------------------------------------------+
```

---

## ⚙️ 2. DevSecOps Pipeline Components

* **Source Control:** GitHub / Azure Repos triggering webhook on PR merge.
* **Security Scanning (SAST):** Trivy, tfsec, checkov, and SonarQube analyzing code before build.
* **Artifact Generation:** Produces immutable versioned packages.
* **Deployment Execution:** Deploys build artifact to target environment (e.g., SCP to NGINX root `/var/www/html` or Helm to Kubernetes).

---

## 💡 Key Takeaways
1. Continuous Delivery requires automated testing through staging, followed by business approval for production releases.
2. Continuous Deployment deploys every passing build directly to production automatically.
