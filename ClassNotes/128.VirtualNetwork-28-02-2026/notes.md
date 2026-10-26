# Class Notes: Azure Virtual Networks (VNet) Deep Dive & Routing 🔌

**Date:** 28-02-2026  
**Topic:** Azure VNet Architecture, Subnet Allocations, Azure Virtual Router, and Default Routing

---

## 📌 1. Azure Virtual Network Architecture

A VNet (`10.12.0.0/16` providing 65,536 addresses) acts as your isolated private datacenter in Azure:

```
 +-----------------------------------------------------------------------+
 | Azure Virtual Network (10.12.0.0/16)                                  |
 |                                                                       |
 |  [ Azure Built-in Virtual Router ] (Routes traffic between subnets)   |
 |                                                                       |
 |   Subnet 1 (10.12.1.0/24)                  Subnet 2 (10.12.2.0/24)    |
 |   ├── VM 01 (10.12.1.4)                    ├── VM 02 (10.12.2.4)      |
 |   └── VM 02 (10.12.1.5)                    └── VM 03 (10.12.2.5)      |
 +-----------------------------------------------------------------------+
```

---

## 🛣️ 2. Azure Default System Routing

By default, Azure automatically creates system routes for every subnet:
* **Intra-VNet Routing:** Any VM in any subnet of a VNet can communicate with any other subnet in the same VNet by default.
* **Internet Egress:** VMs with private IPs are permitted outbound internet access via Azure default SNAT (unless blocked by NSG or overridden by UDR).
* **Cross-VNet Traffic:** Blocked by default until explicit VNet Peering or VPN connectivity is configured.

---

## 💡 Key Takeaways
1. Subnets inside the same VNet communicate natively without requiring manual routers or gateways.
2. Network Security Groups (NSGs) must be applied to subnets to enforce least-privilege traffic boundaries.
