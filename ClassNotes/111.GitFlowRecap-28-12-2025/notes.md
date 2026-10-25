# Class Notes: Enterprise Security, Key Vault Secrets & Disaster Recovery Deep Dive 🔐

**Date:** 28-12-2025  
**Topic:** SSL Termination, Secret Rotation, Backup & DR Strategies, Landing Zone Governance, and Migration Planning

---

## 📌 1. Enterprise Architecture Questions Covered

This session focused on high-level production scenarios frequently encountered in senior DevOps roles:

1. **How is SSL/TLS implemented on Application Gateway using Azure Key Vault?**
   * Upload SSL certificate (PFX) into Azure Key Vault.
   * Enable User-Assigned Managed Identity on Application Gateway.
   * Grant Managed Identity `Get` secret permission on Key Vault.
   * Configure HTTPS listener referencing the Key Vault secret ID.
2. **How do you automate Certificate and Secret Rotation?**
   * Configure Key Vault Secret Expiry notifications via Azure Event Grid.
   * Trigger Azure Functions or Azure Automation Runbooks to rotate credentials automatically.
3. **Backup, Disaster Recovery (DR), and Business Continuity:**
   * **RTO (Recovery Time Objective):** Maximum acceptable duration of downtime.
   * **RPO (Recovery Point Objective):** Maximum acceptable data loss measured in time.
   * Use Azure Backup vaults for VMs and Geo-Redundant Storage (GRS/GZRS) for databases and storage accounts.
4. **VNet Flow Logs & Network Monitoring:**
   * Captures IP traffic flows traversing Network Security Groups into an Azure Storage Account and analyzes them using Traffic Analytics and Log Analytics workspace.

---

## 💡 Key Takeaways
1. Never store plain-text credentials in configuration files; reference them dynamically from Azure Key Vault.
2. Understand RTO and RPO metrics thoroughly when discussing enterprise disaster recovery strategies.
