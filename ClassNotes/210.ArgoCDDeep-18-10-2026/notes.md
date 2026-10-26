# Class Notes: ArgoCD Deep Dive: Declarative GitOps & Continuous Reconciliation 🐙

**Date:** 18-10-2026  
**Topic:** GitOps Principles, ArgoCD Architecture (Server, Repo Server, Controller), and Auto-Sync vs Manual

---

## 📌 1. The 4 Principles of GitOps

1. **Declarative Descriptions:** The entire system is described declaratively in Git (Kubernetes YAML / Helm).
2. **Versioned State of Truth:** Git is the single source of truth; git log provides complete audit history.
3. **Automated Pull Reconciliation:** Software agents pull state from Git; CI pipelines do not push directly into clusters.
4. **Continuous Feedback & Self-Healing:** The controller constantly detects and corrects drift.

---

## ⚙️ 2. ArgoCD Internal Architecture

```
 [ ArgoCD API Server / UI ] <----> Users & Webhooks
         |
 [ ArgoCD Repo Server ]     <----> Clones Git repo, renders Helm/Kustomize manifests
         |
 [ ArgoCD Application Controller ] <----> Compares desired state (Git) vs live state (Cluster)
                                           Triggers sync / self-healing automatically!
```

---

## 💡 Key Takeaways
1. GitOps eliminates configuration drift: If someone manually deletes a pod or modifies a service via `kubectl`, ArgoCD instantly detects the deviation and restores the Git-defined state.
2. Enhances security by operating via **pull** from inside the cluster, requiring zero inbound firewall holes.
