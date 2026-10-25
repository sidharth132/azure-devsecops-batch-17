# Class Notes: Enterprise GitFlow in Automated CI/CD Pipelines 🌿

**Date:** 19-09-2026  
**Topic:** Integrating GitFlow Branching with Azure YAML Pipelines & Quality Gates

---

## 📌 1. Architecture: Automated Branch-to-Environment Promotion

Mapping Git branches directly to automated CI/CD pipeline triggers:

```
 Branch: feature/*    ---> Trigger: PR Validation Pipeline (Linting, Unit Tests, SonarQube)
                                 |
                                 v (PR Merged)
 Branch: develop      ---> Trigger: Continuous Integration & Deploy to DEV Environment
                                 |
                                 v (Release Branch cut)
 Branch: release/*    ---> Trigger: Deploy to QA / Staging Environment (E2E & Load Tests)
                                 |
                                 v (Approved & Merged)
 Branch: main         ---> Trigger: Production Deployment Pipeline (with Manual Approval Gate)
```

---

## 🛡️ 2. Branch Protection Policies

* Enforce minimum of 2 peer code reviews before merge.
* Require linked work items (Azure Boards).
* Require passing CI build validation status checks.
* Prohibit direct push / force push to `main` and `develop`.

---

## 💡 Key Takeaways
1. Branch protection policies prevent untested or unreviewed code from entering production pipelines.
2. Keep CI/CD pipeline definitions versioned in the same repository as the application code.
