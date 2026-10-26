# Class Notes: Azure Kubernetes Service (AKS) & Cloud Managed Clusters ☁️

**Date:** 23-05-2026  
**Topic:** Managed Kubernetes vs Self-Hosted, AKS Architecture, Node Pools, and Cluster Deployment

---

## 📌 1. Managed Kubernetes (AKS) vs. Self-Hosted (On-Premises / IaaS)

Managing Kubernetes control planes manually is complex and operationally expensive:

```
 SELF-HOSTED (Do It Yourself):
   [ Master Nodes (VMs) ] -----> Engineer manages etcd backups, API server upgrades, OS patches
   [ Worker Nodes (VMs) ] -----> Engineer scales and maintains worker nodes
   ❌ High operational overhead, disaster recovery challenges.

 MANAGED KUBERNETES (Azure AKS):
   [ Master Nodes / Control Plane ] -----> 100% Managed by Microsoft (FREE, HA, Auto-upgrades)
   [ Worker Nodes (VMSS) ]          -----> Managed Node Pools (User pays only for VM compute)
   ✅ Zero control plane maintenance, enterprise SLA, automated scaling.
```

---

## 🛠️ 2. AKS Deployment Workflow

```bash
# 1. Create Resource Group
az group create --name rg-aks-dev --location eastus

# 2. Create AKS Cluster with System Node Pool
az aks create   --resource-group rg-aks-dev   --name aks-todo-cluster   --node-count 2   --node-vm-size Standard_B2s   --enable-managed-identity   --generate-ssh-keys

# 3. Download Cluster Kubeconfig to Local Machine
az aks get-credentials --resource-group rg-aks-dev --name aks-todo-cluster

# 4. Verify Cluster Connectivity
kubectl get nodes
```

---

## 💡 Key Takeaways
1. Azure manages the control plane (`etcd`, API server, Scheduler) at zero extra licensing cost.
2. Always integrate Azure Managed Identities and Azure Container Registry (ACR) role attachments with AKS.
