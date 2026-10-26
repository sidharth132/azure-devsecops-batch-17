# Class Notes: Azure Hub-and-Spoke Network Topology & Security Architecture 🌐

**Date:** 14-02-2026  
**Topic:** Hub & Spoke Architecture, Shared Services, VNet Peering, Azure Firewall, and UDR Routing

---

## 📌 1. Architecture: Hub-and-Spoke Network Model

The Hub-and-Spoke network topology is the Microsoft recommended standard for enterprise networking in Azure:

```
                            [ Internet / Corporate WAN ]
                                         |
                                         v
   +--------------------------------------------------------------------------+
   | HUB VNET (Central Shared Services Network)                               |
   |                                                                          |
   |  Subnet 1: AzureBastionSubnet    Subnet 2: AzureFirewallSubnet           |
   |  [ Azure Bastion Host ]          [ Azure Firewall (Private IP 10.0.1.4) ]|
   |                                                                          |
   |  Subnet 3: GatewaySubnet         Subnet 4: ApplicationGatewaySubnet      |
   |  [ VPN / ExpressRoute Gateway ]  [ Azure Application Gateway (WAF) ]     |
   +--------------------------------------------------------------------------+
                      |                                    |
     VNet Peering     |                                    |  VNet Peering
                      v                                    v
   +-----------------------------+      +-----------------------------+
   | SPOKE 1: HealthMonitoring   |      | SPOKE 2: MovieChannel       |
   |                             |      |                             |
   | Subnet 1: Compute / App VMs |      | Subnet 1: Compute / Web VMs |
   | Subnet 2: Database Subnet   |      | Subnet 2: Private Endpoints |
   +-----------------------------+      +-----------------------------+
                 \                                      /
                  \---- Spoke-to-Spoke Direct Peering -/
                        ❌ NOT RECOMMENDED!
                        (All inter-spoke traffic MUST route
                         through Azure Firewall via UDR)
```

---

## 🔑 2. Core Hub-and-Spoke Principles

1. **Centralized Hub:** Contains shared network infrastructure: Azure Firewall, Bastion, VPN Gateway, and Application Gateway.
2. **Isolated Spokes:** Workload environments (Dev, Test, Prod or different business units) hosted in isolated VNets.
3. **VNet Peering:** High-speed, low-latency private connectivity between Hub and each Spoke across the Microsoft Azure backbone.
4. **No Direct Spoke-to-Spoke Peering:** Direct peering between spokes is an anti-pattern. Inter-spoke communication must route through the central Azure Firewall for traffic inspection.
5. **User-Defined Routes (UDR):** Route tables associated with Spoke subnets override default system routes, directing `0.0.0.0/0` traffic to the Azure Firewall's private IP (`NextHopType = VirtualAppliance`).

---

## 💡 Key Takeaways
1. Hub-and-Spoke delivers centralized security, compliance monitoring, and cost efficiency by sharing expensive resources like Firewalls and VPN Gateways.
2. Configure UDRs on all spoke subnets to ensure all egress and inter-spoke traffic is inspected.
