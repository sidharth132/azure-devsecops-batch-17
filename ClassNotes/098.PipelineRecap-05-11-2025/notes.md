# Class Notes: Pipeline Recap, Git Areas & Azure DevOps Organization Structure 🔁

**Date:** 05-11-2025  
**Topic:** Git Internal Areas, Azure DevOps Organizations & Projects, and Pipeline Triggering

---

## 📌 1. Git Workflow & Local Storage Areas

Understanding how files move through Git before hitting the pipeline:

```
+-------------------+       git add       +-------------------+      git commit     +-------------------+       git push       +-------------------+
| Working Directory | ------------------> |   Staging Area    | ------------------> | Local Repository  | -------------------> |    Azure Repos    |
| (Modified files)  |                     | (Index / Tracked) |                     |   (.git folder)   |                      | (Remote Central)  |
+-------------------+                     +-------------------+                     +-------------------+                      +-------------------+
```

---

## 🏢 2. Azure DevOps Hierarchy

* **Azure DevOps Organization (`dev.azure.com/<org_name>`):** Top-level administrative boundary for your company.
* **Projects:** Logical partitions within the organization isolating teams, repositories, boards, and pipelines.
* **Repositories (Azure Repos):** Git repositories containing application source code and infrastructure configurations.
* **Pipelines (Azure Pipelines):** Automation engines responding to repo events (commits, PRs, tags).

---

## 💡 Key Takeaways
1. Pipeline automation triggers directly from branch pushes or pull requests in Azure Repos.
2. Always keep your local git state synchronized (`git pull --rebase`) before committing changes.
