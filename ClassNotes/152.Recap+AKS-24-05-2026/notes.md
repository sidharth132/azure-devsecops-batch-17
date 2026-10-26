# Class Notes: AKS Architecture Recap, Node Pools & Cluster Authentication 🔐

**Date:** 24-05-2026  
**Topic:** System vs User Node Pools, Kubeconfig Architecture, Contexts, and API Authentication

---

## 📌 1. Architecture: AKS Node Pools

AKS organizes worker nodes into distinct **Node Pools** backed by Virtual Machine Scale Sets (VMSS):

```
 +-----------------------------------------------------------------------+
 | AKS CLUSTER: aks-todo-cluster                                         |
 |                                                                       |
 |  [ System Node Pool ] (mode = System)                                 |
 |    - Runs core cluster pods: CoreDNS, kube-proxy, metrics-server      |
 |    - Critical for cluster health and control plane communication      |
 |                                                                       |
 |  [ User Node Pool ] (mode = User)                                     |
 |    - Dedicated to customer application workloads (Frontend, Backend)   |
 |    - Autoscale enabled (min 2, max 10 nodes)                         |
 |    - Taints/tolerations isolate customer pods from system workloads   |
 +-----------------------------------------------------------------------+
```

---

## 🔑 2. How `az aks get-credentials` Works

* Generates or updates the local `~/.kube/config` file.
* Inserts:
  1. **Cluster URL:** The managed API server FQDN (e.g., `aksmicrodev-xxx.hcp.eastus.azmk8s.io`).
  2. **CA Certificate:** Validates cluster SSL trust.
  3. **User Authentication:** Azure CLI bearer token or client certificate.
* Allows `kubectl` commands executed from your local workstation to communicate securely with the remote cluster.

---

## 💡 Key Takeaways
1. Isolate user workloads on dedicated User Node Pools to prevent noisy application pods from starving critical system daemons.
2. Store `kubeconfig` securely; never commit it to public version control.
