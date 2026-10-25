# Class Notes: Terraform Crash Course: Providers, Resources & State Management 🪔

**Date:** 14-10-2025  
**Topic:** Terraform Fundamentals, Cloud Comparison, State Management, and Provider Plugins

---

## 📌 1. Why Terraform? Cloud Comparison

| Feature | Terraform | ARM Templates / Bicep | AWS CloudFormation |
| :--- | :--- | :--- | :--- |
| **Vendor** | HashiCorp (Open Source) | Microsoft (Azure only) | AWS (AWS only) |
| **Multi-Cloud** | **Yes (Azure, AWS, GCP, K8s)**| No (Azure only) | No (AWS only) |
| **Community & Ecosystem**| Massive global ecosystem | Microsoft ecosystem | AWS ecosystem |
| **State Management** | Centralized, cross-cloud | Managed by Azure ARM | Managed by AWS CloudFormation|

---

## ⚙️ 2. Core Terraform Workflow & Pre-requisites

### Pre-requisites:
1. VS Code editor installed with HashiCorp Terraform extension.
2. Azure CLI (`az`) installed and authenticated (`az login`).
3. Terraform binary installed and added to system PATH.

### Workflow:
1. `terraform init`: Initializes working directory, downloads provider plugins (`hashicorp/azurerm`).
2. `terraform plan`: Generates execution plan showing additions, changes, and deletions.
3. `terraform apply`: Executes API calls against cloud provider to provision resources.
4. `terraform show`: Inspects human-readable state file content.

---

## 💡 Key Takeaways
1. Terraform's multi-cloud capability allows enterprise teams to standardize on one IaC language across disparate clouds.
2. Always verify the plan output before applying to prevent unintended infrastructure deletion.
