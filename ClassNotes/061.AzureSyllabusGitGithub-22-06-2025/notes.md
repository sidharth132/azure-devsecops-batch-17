# Class Notes: Terraform Child & Parent Modules & Git/GitHub Foundations 🗂️

**Date:** 22-06-2025  
**Topic:** Terraform Modular Architecture, Parent-Child Relationships, and Version Control Integration

---

## 📌 1. Architecture: Modular Terraform Structure

As infrastructure scales, monolithic Terraform files (`main.tf` with hundreds of resources) become unmaintainable. Modularization isolates components and promotes code reuse.

```
 [ Root / Parent Module ]  (environments/dev)
        |
        |---> Calls Module: VNet Module (creates virtual network & subnets)
        |---> Calls Module: Network Security Group (NSG)
        |---> Calls Module: Compute VM (creates Frontend VM1 & VM2 with Private IPs)
        |
 [ Input: terraform.tfvars ] -----> Supplies Environment-Specific Values
```

---

## 🧱 2. Parent Module vs. Child Module

* **Child Module:** The self-contained blueprint declaring resources.
  * `variables.tf`: Declares input parameters accepted by the module.
  * `main.tf`: Declares the resource blocks using the variables.
  * `outputs.tf`: Exports resource attributes (e.g., Subnet ID, Private IP) to callers.
* **Parent (Root) Module:** The calling configuration.
  * Instantiates the child module using a `module` block:
  ```hcl
  module "vnet_todo_app" {
    source              = "../../modules/vnet"
    vnet_name           = "vnet-todo-dev"
    resource_group_name = azurerm_resource_group.rg.name
    address_space       = ["10.0.0.0/16"]
    subnets             = var.subnet_configurations
  }
  ```

---

## 🌿 3. Git & GitHub Foundations for DevOps Engineers

Version control is the foundation of modern DevSecOps.

```
 Working Directory  ---(git add)--->  Staging Area  ---(git commit)--->  Local Repository  ---(git push)--->  GitHub / Azure Repos
   (Modified Files)                     (Indexed)                           (.git folder)                           (Remote)
```

### Core Workflow:
1. `git init`: Initializes a new local Git repository.
2. `git clone <url>`: Clones a remote repository to your local workstation.
3. `git status`: Checks the state of the working directory and staging index.
4. `git add .`: Stages modified and new files.
5. `git commit -m "feat: message"`: Records a snapshot to local history.
6. `git push origin <branch>`: Transmits commits to the remote hosting platform.

---

## 💡 Key Takeaways
1. Child modules should be environment-agnostic; all variables should be injected by the parent module.
2. Terraform code must always be stored in Git repositories with branch protection and code reviews.
