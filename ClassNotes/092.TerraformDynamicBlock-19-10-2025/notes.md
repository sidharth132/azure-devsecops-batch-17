# Class Notes: Terraform Dynamic Blocks & Monolithic Deployment Prep 🧩

**Date:** 19-10-2025  
**Topic:** Dynamic Blocks (`dynamic`), `for_each` Iteration, and Architecture Setup for Monolithic 3-Tier Application

---

## 📌 1. Architecture: Monolithic 3-Tier Architecture on Azure

The goal is deploying a secure, highly available monolithic application:

```
 [ Internet Users ]
         |
         v
 [ Azure Application Gateway (AppGw) ] ---- Public Facing
         |
         v
 [ Frontend Subnet ]
   ├── Frontend VM 01 (F01)
   └── Frontend VM 02 (F02)
         |
         v
 [ Internal Load Balancer (LB) ]
         |
         v
 [ Backend Subnet ]
   ├── Backend VM 01 (B01)
   └── Backend VM 02 (B02)
         |
         v
 [ Database Subnet ] (Managed DB)
```

---

## 🔁 2. Terraform Dynamic Blocks Explained

When configuring resources with multiple repetitive nested blocks (such as Network Security Group rules, subnets in a VNet, or App Gateway routing rules), declaring each block manually causes severe code duplication.

### The Solution: `dynamic` Block

```hcl
variable "nsg_rules" {
  type = list(object({
    name                   = string
    priority               = number
    direction              = string
    access                 = string
    protocol               = string
    source_port_range      = string
    destination_port_range = string
    source_address_prefix  = string
    destination_address_prefix = string
  }))
}

resource "azurerm_network_security_group" "nsg" {
  name                = "app-nsg"
  location            = "West Europe"
  resource_group_name = "rg-monolith"

  dynamic "security_rule" {
    for_each = var.nsg_rules
    content {
      name                       = security_rule.value.name
      priority                   = security_rule.value.priority
      direction                  = security_rule.value.direction
      access                     = security_rule.value.access
      protocol                   = security_rule.value.protocol
      source_port_range          = security_rule.value.source_port_range
      destination_port_range     = security_rule.value.destination_port_range
      source_address_prefix      = security_rule.value.source_address_prefix
      destination_address_prefix = security_rule.value.destination_address_prefix
    }
  }
}
```

* `dynamic "security_rule"`: Generates multiple nested `security_rule` blocks dynamically.
* `for_each`: Iterates over the collection.
* `content`: Defines the block attributes populated from `security_rule.value.<field>`.

---

## 💡 Key Takeaways
1. Use dynamic blocks to eliminate repetitive nested blocks in HCL.
2. Dynamic blocks make complex resources like NSGs, VNets, and Application Gateways completely data-driven.
