# Class Notes: Terraform AKS Landing Zone & ACR Integration 🏗️

**Date:** 30-05-2026  
**Topic:** Modular Terraform for AKS, Azure Container Registry (ACR), and `AcrPull` Role Assignment

---

## 📌 1. Architecture: AKS and ACR Integration

```
 [ CI/CD Pipeline ]
        | (Docker Build & Push)
        v
 [ Azure Container Registry (ACR) ]
        ^
        | (Pulls image securely without credentials)
        | Role Assignment: AcrPull
 [ AKS Cluster Managed Identity ]
        |
        v
 [ Worker Nodes (Runs application pods) ]
```

---

## 🧱 2. Terraform Module Structure

```hcl
# 1. Container Registry
resource "azurerm_container_registry" "acr" {
  name                = "acrtododev01"
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  sku                 = "Standard"
  admin_enabled       = false
}

# 2. Managed Kubernetes Service
resource "azurerm_kubernetes_cluster" "aks" {
  name                = "aks-todo-dev"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  dns_prefix          = "akstododev"

  default_node_pool {
    name       = "systempool"
    node_count = 2
    vm_size    = "Standard_B2s"
  }

  identity {
    type = "SystemAssigned"
  }
}

# 3. Grant AKS Permission to Pull from ACR (No Secrets Needed!)
resource "azurerm_role_assignment" "aks_acr_pull" {
  principal_id                     = azurerm_kubernetes_cluster.aks.kubelet_identity[0].object_id
  role_definition_name             = "AcrPull"
  scope                            = azurerm_container_registry.acr.id
  skip_service_principal_aad_check = true
}
```

---

## 💡 Key Takeaways
1. Use native Azure Managed Identities with `AcrPull` role assignment instead of embedding secret docker-registry pull secrets in Kubernetes manifests.
2. Modularize AKS and ACR configurations for reproducible environment deployments.
