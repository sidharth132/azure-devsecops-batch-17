# Class Notes: GitFlow Branching Strategy in Enterprise Pipelines 🌿

**Date:** 30-11-2025  
**Topic:** GitFlow vs Trunk-Based Development, Branch Lifecycle, and Environment Mapping

---

## 📌 1. Overview: GitFlow Branching Architecture

GitFlow defines a strict branching model designed around scheduled releases and parallel feature development:

```
 main (Protected / Production) ------------------------------------------------------------[Tag v1.0]--->
                                     ^                                                          ^
                                     | (Merge PR)                                               |
 release/v1.0 -----------------------+----------------------------------------------------------+
                  ^
                  | (Cut release)
 develop (Integration Branch) ------------------------------------------------------------------------->
        ^                        |
        | (Merge PR)             | (Branch out)
        |                        v
 feature/dev-changes ----------> [Feature Development]
```

---

## 🌳 2. Branch Hierarchy & Roles

* **`main` (or `master`):** Stores official release history. Highly protected; no direct commits permitted. Deploys to Production.
* **`develop`:** Integration branch for completed features. Serves as the base for all feature branches and automated Dev deployments.
* **`feature/*`:** Branch off `develop`. Used by individual developers for isolated changes. Merged back via Pull Requests (PRs).
* **`release/*`:** Branch off `develop` when features are frozen for an upcoming release. Undergoes QA and bug fixing before merging to `main` and `develop`.
* **`hotfix/*`:** Branch off `main` to address critical production issues immediately, then merged back into both `main` and `develop`.

---

## 🔄 3. Trunk-Based vs. GitFlow Strategy

| Aspect | GitFlow | Trunk-Based Development |
| :--- | :--- | :--- |
| **Branch Lifespan** | Long-lived branches (`develop`, `release`) | Short-lived feature branches (< 1–2 days) |
| **Release Cadence** | Scheduled releases (sprints / milestones) | Continuous, frequent releases (multiple per day) |
| **Complexity** | High branch management overhead | Simple; relies on automated tests & feature flags |
| **Best For** | Enterprise scheduled releases & legacy apps | Microservices and modern CI/CD teams |

---

## 💡 Key Takeaways
1. Protect `main` and `develop` branches using Azure Repos branch policies requiring minimum reviewers and passing CI builds.
2. Delete feature branches immediately after merging to keep the repository clean.
