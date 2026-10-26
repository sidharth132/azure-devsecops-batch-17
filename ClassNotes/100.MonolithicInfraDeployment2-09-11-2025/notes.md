# Class Notes: Automated VM Bootstrapping with Terraform custom_data ⚡

**Date:** 09-11-2025  
**Topic:** VM Provisioning Automation, custom_data, Cloud-Init, Base64 Encoding, and Eliminating Manual SSH

---

## 📌 1. Manual SSH vs. Automated custom_data

In production DevOps, manually SSHing into virtual machines to run commands is an anti-pattern:

```
 Manual Approach (Anti-Pattern):
   Terraform provisions VM -> Engineer finds Public IP -> SSH into VM -> apt update -> apt install nginx
   ❌ Slow, error-prone, violates immutable infrastructure principles.

 Automated custom_data Approach (Best Practice):
   Terraform Configuration contains bootstrap script
          |
          v (Base64 Encode)
   Azure ARM API injects custom_data into VM metadata
          |
          v (VM Boots)
   Cloud-Init decodes Base64 and executes script automatically on first boot
   ✅ 100% automated, repeatable, zero manual SSH required.
```

---

## 🔐 2. Why Base64 Encoding?

Azure VM metadata APIs require `custom_data` payloads to be **Base64 encoded**:
* **Data Integrity:** Protects special characters, newlines, quotes, and binary content from corruption during HTTP transit.
* **Terraform Function:** `base64encode(file("${path.module}/scripts/init.sh"))`.

---

## 🛠️ 3. Implementation Example

```hcl
resource "azurerm_linux_virtual_machine" "frontend_vm" {
  name                = "vm-frontend-dev"
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  size                = "Standard_B1s"
  admin_username      = "azureuser"

  network_interface_ids = [
    azurerm_network_interface.frontend_nic.id,
  ]

  admin_ssh_key {
    username   = "azureuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-jammy"
    sku       = "22_04-lts"
    version   = "latest"
  }

  # Automated Cloud-Init Bootstrap Script
  custom_data = base64encode(<<-EOF
              #!/bin/bash
              apt-get update -y
              apt-get install -y nginx
              systemctl start nginx
              systemctl enable nginx
              echo "<h1>Deployed via Terraform custom_data</h1>" > /var/www/html/index.html
              EOF
  )
}
```

---

## 💡 Key Takeaways
1. Never configure virtual machines manually via SSH in automated environments.
2. Use `custom_data` (cloud-init) for bootstrapping packages, system configurations, and startup daemons.
