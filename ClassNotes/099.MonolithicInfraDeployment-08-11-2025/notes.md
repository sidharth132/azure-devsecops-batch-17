# Class Notes: Monolithic Infrastructure Deployment with Terraform on Azure 🏗️

**Date:** 08-11-2025  
**Topic:** End-to-End Monolithic Infrastructure: Resource Group, VNet, Subnets, VM, NIC, and Public IP

---

## 📌 1. Architecture: Monolithic 3-Tier Infrastructure

Deploying the complete foundation for a monolithic 3-tier web application using automated Terraform pipelines.

```
 +-----------------------------------------------------------------------------------------+
 | Resource Group: rg-todo-monolith                                                        |
 |                                                                                         |
 |  +-----------------------------------------------------------------------------------+  |
 |  | Virtual Network: 10.0.0.0/16                                                      |  |
 |  |                                                                                   |  |
 |  |  +------------------------------+       +--------------------------------------+  |  |
 |  |  | Frontend Subnet (10.0.1.0/24)|       | Backend Subnet (10.0.2.0/24)         |  |  |
 |  |  |                              |       |                                      |  |  |
 |  |  |  [ Public IP ]               |       |  [ Backend VM ]                      |  |  |
 |  |  |        |                     |       |  - Private IP only                   |  |  |
 |  |  |        v                     |       |  - Node.js / Python API              |  |  |
 |  |  |  [ Frontend VM ]             |       +--------------------------------------+  |  |
 |  |  |  - NGINX Web Server          |                                                 |  |
 |  |  |  - Serves React UI           |       +--------------------------------------+  |  |
 |  |  +------------------------------+       | Database Subnet (10.0.3.0/24)        |  |  |
 |  |                                         |  [ Azure SQL / PostgreSQL ]          |  |  |
 |  |                                         +--------------------------------------+  |  |
 |  +-----------------------------------------------------------------------------------+  |
 +-----------------------------------------------------------------------------------------+
```

---

## 🛠️ 2. Core Infrastructure Components in Code

1. **`azurerm_resource_group`:** Logical boundary containing all project resources.
2. **`azurerm_virtual_network`:** Private address space (`10.0.0.0/16`).
3. **`azurerm_subnet`:** Isolated subnets for frontend, backend, and database tiers.
4. **`azurerm_public_ip`:** Allocated to the frontend VM for external browser access.
5. **`azurerm_network_interface`:** Connects VM to the frontend subnet and attaches the public IP.
6. **`azurerm_linux_virtual_machine`:** Compute instance configured with Ubuntu 22.04 LTS.

---

## 💡 Key Takeaways
1. Maintain strict tier isolation: Only the frontend tier receives a public IP; backend and database tiers remain entirely private.
2. Terraform coordinates resource creation in proper order through implicit dependencies.
