# Class Notes: Azure VM Provisioning with Terraform Part 2 & Data Blocks 🖥️

**Date:** 02-11-2025  
**Topic:** Terraform Data Sources (`data`), Dynamic Subnet Resolution, Network Interfaces, and Multi-VM Deployment

---

## 📌 1. Architecture: Scalable Multi-VM Provisioning

Provisioning multiple Virtual Machines cleanly requires referencing existing network infrastructure (VNets and Subnets) using Terraform **Data Sources**.

```
 [ Existing VNet / Subnet ]  <---- (Queried via data "azurerm_subnet")
           |
           v
 [ Network Interface (NIC) ] <---- (Created for each VM using for_each)
           |
           v
 [ Linux Virtual Machine ]  <---- (Attached to NIC with custom_data / SSH key)
```

---

## 🔍 2. Using Terraform Data Sources (`data` block)

When subnets or virtual networks were created outside the current module or pre-exist in Azure, use `data` blocks to read their attributes:

```hcl
data "azurerm_subnet" "subnet" {
  for_each             = var.vms3
  name                 = each.value.subnet_name
  virtual_network_name = each.value.vnet_name
  resource_group_name  = each.value.rg_name
}
```

* **Purpose:** Queries the Azure ARM API at plan/apply time to fetch existing resources without managing their lifecycle.

---

## ⚙️ 3. Nested Map Definition for VMs

```hcl
vms3 = {
  vm1_frontend = {
    vm_name      = "vm-frontend-01"
    subnet_name  = "frontend-subnet"
    vnet_name    = "vnet-pilu-dev-todoapp-01"
    rg_name      = "rg-pilu-dev-01"
    vm_size      = "Standard_B2s"
    admin_user   = "azureuser"
  }
  vm2_backend = {
    vm_name      = "vm-backend-01"
    subnet_name  = "backend-subnet"
    vnet_name    = "vnet-pilu-dev-todoapp-01"
    rg_name      = "rg-pilu-dev-01"
    vm_size      = "Standard_B2s"
    admin_user   = "azureuser"
  }
}
```

---

## 💡 Key Takeaways
1. Data blocks allow modular Terraform code to bind to pre-existing landing zones without duplicating resource management.
2. Pair `for_each` with complex maps to dynamically generate NICs, Disks, and VMs in a single block.
