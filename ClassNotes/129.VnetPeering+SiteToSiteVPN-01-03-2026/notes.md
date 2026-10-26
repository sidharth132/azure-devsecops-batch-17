# Class Notes: VNet Peering vs. Site-to-Site VPN & Azure Private DNS 🔗

**Date:** 01-03-2026  
**Topic:** VNet Peering, Gateway Transit, Site-to-Site (S2S) VPN, and Azure Private DNS Zones

---

## 📌 1. VNet Peering vs. Site-to-Site VPN

Connecting separate networks in cloud architectures:

| Feature | VNet Peering | Site-to-Site (S2S) VPN |
| :--- | :--- | :--- |
| **Path Traversed** | Private Microsoft Backbone | Public Internet (IPsec encrypted tunnel) |
| **Latency & Bandwidth** | Ultra-low latency, up to line rate | Limited by VPN Gateway SKU (e.g., 650 Mbps – 1.25 Gbps) |
| **Encryption** | Traffic stays on private fiber | Encrypted via IPsec / IKE |
| **Primary Use Case** | Cloud-to-Cloud (Azure to Azure VNets) | On-Premises Datacenter to Azure VNet |
| **Transitive Routing** | Non-transitive by default | Non-transitive without route configuration |

---

## 🌐 2. Azure Private DNS Zones

* **The Problem:** Virtual machines in peered VNets or private subnets need to resolve internal services by name rather than volatile IP addresses.
* **The Solution:** Azure Private DNS Zones (e.g., `privatelink.postgres.database.azure.com`).
* **Virtual Network Link:** Links the private DNS zone to target VNets with auto-registration enabled to automatically register VM hostnames.

---

## 💡 Key Takeaways
1. Use VNet Peering for Azure-to-Azure connectivity and Site-to-Site VPN for hybrid on-premises connectivity.
2. VNet Peering is non-transitive: If VNet A is peered to VNet B, and VNet B is peered to VNet C, A cannot reach C without routing appliances.
