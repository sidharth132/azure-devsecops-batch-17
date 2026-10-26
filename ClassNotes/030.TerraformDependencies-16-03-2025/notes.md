# Class Notes: Terraform Dependencies & State Drift Scenarios 🧠

**Date:** 16-03-2025  
**Topic:** Terraform State File, Implicit vs Explicit Dependencies, and Configuration Drift Management

---

## 📌 1. Overview: The Brain of Terraform

**"Terraform ka Dimag = Terraform State File (`.tfstate`)"**  
The state file maps real-world cloud resources to your configuration files, tracks metadata, stores resource attributes, and improves performance when managing large architectures.

---

## ⚡ 2. Parallel Execution vs. Dependencies

By default, Terraform attempts to execute resource creations **in parallel** for maximum performance. However, cloud resources often have parent-child hierarchies (e.g., an Azure Storage Account cannot be created before its Resource Group exists).

```
Without Dependency:
  RG Creation -----------> [Running] ----------------> [Created]
  Storage Acct ----------> [Running] ------> [FAILED: RG does not exist!]

With Dependency:
  RG Creation -----------> [Running] ------> [Created]
                                                 |
  Storage Acct ---------------------------------> [Running] ------> [Created]
```

---

## 🔗 3. Implicit vs. Explicit Dependencies

### Implicit Dependency ("Andar-Andar")
Occurs automatically when an argument of one resource references an attribute output of another resource. Terraform inspects this graph and infers creation order.

```hcl
resource "azurerm_resource_group" "santara" {
  name     = "rg-nimbu"
  location = "West Europe"
}

# Implicit Dependency: Uses santara.name and santara.location
resource "azurerm_storage_account" "stg" {
  name                     = "nimbustorage123"
  resource_group_name      = azurerm_resource_group.santara.name
  location                 = azurerm_resource_group.santara.location
  account_tier             = "Standard"
  account_replication_type = "GRS"
}
```
* **Advantage:** *"Ek teer se do nishane"* — passes dynamic values (preventing typos) and automatically constructs dependency ordering.

### Explicit Dependency ("Bahar-Bahar")
Declared manually using the `depends_on` meta-argument when Terraform cannot infer relationships from configuration attributes alone.

```hcl
resource "azurerm_storage_account" "stg" {
  depends_on               = [azurerm_resource_group.santara]
  name                     = "nimbustorage123"
  resource_group_name      = "rg-nimbu"
  location                 = "West Europe"
  account_tier             = "Standard"
  account_replication_type = "GRS"
}
```

---

## 🔄 4. The 4 State Drift Scenarios

Configuration drift occurs when the reality in the cloud diverges from code or state:

```
        +-------------------------------------------------------+
        |                  TERRAFORM DRIFT MATRIX               |
        +-------------------------------------------------------+
        | Scenario | Code (.tf) | State (.tfstate) | Azure Cloud| Action on terraform apply |
        +----------+------------+------------------+------------+---------------------------+
        |    1     | Changed/New| Old/None         | Old/None   | Creates or updates resource|
        |    2     | Present    | Present          | DELETED ❌ | Re-creates missing resource|
        |    3     | REMOVED ❌ | Present          | Present    | Destroys resource in cloud|
        |    4     | None (Ghost| None             | CREATED ❌ | Ignored (Untracked)       |
        +-------------------------------------------------------+
```

* **Scenario 1 (Code Added/Modified):** Terraform updates the cloud resource to match new code specifications.
* **Scenario 2 (Manual Deletion in Cloud):** Someone deletes a resource directly via Azure Portal. Terraform detects the missing resource during refresh and provisions it anew.
* **Scenario 3 (Resource Deleted from Code):** Developer removes block from `.tf`. Terraform destroys the cloud resource to maintain declarative parity.
* **Scenario 4 (Resource Created Directly via Portal):** An engineer creates an unmanaged resource ("Ghost resource"). Terraform ignores it because it is not in the configuration or state file. (Must use `terraform import` to bring under management).

---

## 💡 Key Takeaways
1. Always prefer **implicit dependencies** by referencing resource attributes; only use `depends_on` when necessary.
2. Terraform state drift is reconciled automatically during `terraform apply` by comparing actual cloud API responses against desired configuration.
