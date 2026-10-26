# Class Notes: How Terraform Works Under the Hood & HCL Structure 🏗️

**Date:** 15-03-2025  
**Topic:** Terraform Workflow, Core CLI Commands, Block Anatomy, and Provider Plugins

---

## 📌 1. Architecture & Working Mechanism

Terraform is a declarative Infrastructure as Code tool that provisions and manages cloud resources by communicating with cloud provider APIs.

```
 [ Configuration (.tf) ]
            |
            v
   terraform init  -----> Downloads Provider Plugins (e.g. azurerm.exe) into .terraform/
            |
            v
   terraform plan  -----> Compares Code vs State vs Cloud API -> Generates Execution Plan
            |
            v
   terraform apply -----> Executes API Calls -> Creates Resources -> Writes terraform.tfstate
```

---

## 🧱 2. Anatomy of a Terraform Resource Block

Every resource declaration in HashiCorp Configuration Language (HCL) follows a strict structure:

```hcl
resource "azurerm_resource_group" "dhimaka" {
  name     = "dhondhu-rg"
  location = "West Europe"
}
```

* **`resource`**: The block type (declares an infrastructure resource).
* **`"azurerm_resource_group"`**: The resource type (specifies the cloud provider `azurerm` and component `resource_group`).
* **`"dhimaka"`**: The local resource name / identifier (used internally by Terraform to reference this block).
* **Arguments (`name`, `location`)**: Configuration parameters passed to the Azure API.

---

## ⚙️ 3. The Core Terraform Lifecycle Commands

### 1. `terraform init`
* Scans all `.tf` files in the current working directory.
* Identifies required providers (e.g., `hashicorp/azurerm`).
* Downloads provider plugin executables (e.g., `azurerm.exe`) into the hidden `.terraform/` folder.
* Generates `.terraform.lock.hcl` to lock exact provider versions and guarantee build reproducibility.

### 2. `terraform validate`
* Checks syntax and internal consistency of configuration files without calling cloud APIs.

### 3. `terraform plan`
* Refreshes current state from cloud APIs.
* Compares desired configuration (`.tf` files) with existing state (`terraform.tfstate`).
* Outputs planned changes:
  * `+` resource to be created
  * `~` resource to be modified in-place
  * `-` resource to be destroyed

### 4. `terraform apply`
* Requests user confirmation (unless `-auto-approve` is passed).
* Invokes cloud provider REST APIs to provision/update resources.
* Records resulting resource IDs and metadata in `terraform.tfstate`.

### 5. `terraform destroy`
* Deletes all managed infrastructure tracked in the state file in reverse dependency order.

---

## 🗺️ 4. Data Types in Terraform: Maps, Lists & Strings

Terraform uses standard data structures for parameterized configurations:
* **String:** `"West Europe"`
* **List:** `["subnet1", "subnet2", "subnet3"]`
* **Map:** Key-value pairs used for dictionary lookups:
  ```hcl
  variable "resource_tags" {
    type = map(string)
    default = {
      environment = "dev"
      project     = "todo-app"
      owner       = "devops-team"
    }
  }
  ```

---

## 💡 Key Takeaways
1. Terraform searches for `.tf` files in the directory where the command is executed.
2. The `.terraform` folder contains vendor plugins; it should be added to `.gitignore`.
3. The `.terraform.lock.hcl` file should be committed to version control to pin provider versions across team members.
