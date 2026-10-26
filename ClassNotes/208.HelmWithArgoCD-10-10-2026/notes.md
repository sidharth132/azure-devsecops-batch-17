# Class Notes: Helm Package Management & GitOps with ArgoCD ⎈

**Date:** 10-10-2026  
**Topic:** Helm Charts Anatomy, Templating, Values Files, and ArgoCD Declarative Application CRDs

---

## 📌 1. Why Helm? Templating Raw Kubernetes YAMLs

Raw Kubernetes YAML manifests are static and repetitive across environments (Dev, QA, Prod). **Helm** is the package manager for Kubernetes:

```
 helm-chart/
 ├── Chart.yaml          # Metadata (name, version, description)
 ├── values.yaml         # Default configuration values
 ├── values-dev.yaml     # Dev environment overrides (replicas: 1, small cpu)
 ├── values-prod.yaml    # Prod environment overrides (replicas: 5, large cpu)
 └── templates/          # Parameterized Kubernetes manifests
     ├── deployment.yaml
     ├── service.yaml
     └── ingress.yaml
```

---

## 🐙 2. GitOps with ArgoCD

ArgoCD continuously reconciles the live cluster state with the desired state stored in a Git repository:

```
 [ Git Repository (Helm Chart) ] <====== (Watches for commits) ====== [ ArgoCD Controller ]
                                                                             |
                                                                             | (Reconciliation Loop)
                                                                             v
                                                                 [ Live AKS Cluster State ]
```

---

## 💡 Key Takeaways
1. Helm parameterizes Kubernetes manifests into reusable, version-controlled charts.
2. ArgoCD pulls configurations from Git, eliminating the need to expose cluster API credentials to CI runners.
