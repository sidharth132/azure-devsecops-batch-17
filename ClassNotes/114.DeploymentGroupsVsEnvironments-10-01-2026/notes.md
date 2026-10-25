# Class Notes: Azure Pipelines Deployment Groups vs. Environments 🎯

**Date:** 10-01-2026  
**Topic:** Classic Deployment Groups vs. Modern YAML Environments, Security Boundaries, and Rolling Releases

---

## 📌 1. Architecture: Deployment Groups vs. Environments

Deploying application artifacts across a fleet of physical or virtual machines requires an organized target registry:

```
 Classic Deployment (Classic Release Pipelines):
   Deployment Groups ----> Target VMs run Microsoft Deployment Agent
   ❌ Coupled to Classic UI, lacks Git versioning.

 Modern YAML Pipelines (Best Practice):
   Environments ---------> Represents target environment (Dev, Stage, Prod)
   └── Targets (VMs, Kubernetes clusters) registered as resources
   ✅ Version-controlled in Git, supports approval checks, locks, and traceability.
```

---

## 🔒 2. Port 22 Security & Zero-Trust Access

* **The Problem:** Opening SSH Port 22 to the public internet creates severe security vulnerabilities.
* **The Solution:**
  1. Remove Public IPs and close Port 22 on VM Network Security Groups.
  2. Access machines via **Azure Bastion** or internal agent-based polling.
  3. Azure Pipeline agents installed locally on VMs poll outward via HTTPS (Port 443), requiring **zero inbound ports** open.

---

## 💡 Key Takeaways
1. Modern Azure DevOps workflows standardize on **YAML Environments** instead of Classic Deployment Groups.
2. Outbound agent communication eliminates the need to expose inbound management ports like Port 22 or 3389.
