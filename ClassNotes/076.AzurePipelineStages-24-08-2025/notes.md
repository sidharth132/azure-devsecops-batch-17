# Class Notes: Azure Pipelines Stages, Jobs & Agent Pools 🚀

**Date:** 24-08-2025  
**Topic:** Azure DevOps YAML Pipeline Hierarchy, Agent Pools, Pool Overrides, and Parallel Job Execution

---

## 📌 1. Azure Pipeline Hierarchy

Azure Pipelines YAML configurations are structured into four nested tiers:

```
 Pipeline (Top Level)
    └── Stages (e.g., Build, Test, Deploy to Dev, Deploy to Prod)
           └── Jobs (Run on an agent in a designated agent pool)
                  └── Steps / Tasks (Sequential commands: bash, pwsh, tasks)
```

---

## 🤖 2. Agent Pool Scopes & Overriding

Agent pools can be defined at both the **pipeline level** and the **job level**:

```yaml
# Pipeline-level default agent pool
pool:
  vmImage: 'ubuntu-latest'

stages:
  - stage: BuildStage
    jobs:
      - job: JobA
        # Inherits pipeline pool (ubuntu-latest)
        steps:
          - script: echo "Building on Ubuntu..."

      - job: JobB
        # Overrides pipeline pool to use a Windows agent
        pool:
          vmImage: 'windows-latest'
        steps:
          - script: echo "Building on Windows..."
```

* **Rule:** If a job explicitly defines a `pool`, it overrides the top-level pipeline pool. If no pool is specified inside the job, it inherits the top-level pipeline pool.

---

## ⚡ 3. Parallelism in Pipelines

* **Parallel Jobs:** Running multiple independent jobs simultaneously across separate agents.
* **When does it run in parallel?** When there is no `dependsOn` relationship declared between jobs.
* **Advantages:**
  * Drastically reduces pipeline execution time.
  * Allows running linting, unit tests, and security scans in parallel.
* **Sequential Flow with `dependsOn`:**
  ```yaml
  jobs:
    - job: BuildJob
      steps: ...

    - job: TestJob
      dependsOn: BuildJob # Waits for BuildJob to succeed
      steps: ...
  ```

---

## 💡 Key Takeaways
1. Multi-stage pipelines enable clear separation between CI (Build/Test) and CD (Deploy/Validate).
2. Utilize parallel jobs to minimize feedback loop times for developers.
