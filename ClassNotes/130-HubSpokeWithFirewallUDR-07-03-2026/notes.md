# Class Notes: Hub & Spoke Routing with Azure Firewall & User-Defined Routes (UDR) 🛡️

**Date:** 07-03-2026  
**Topic:** Azure Firewall Deployment, Route Tables (UDR), Egress Traffic Inspection, and Default Route Overriding

---

## 📌 1. Architecture: Enforcing Hub Inspection with UDR

To secure enterprise traffic, all outbound internet requests and inter-spoke traffic must pass through the centralized **Azure Firewall**:

```
 [ Spoke 1 VNet (10.15.0.0/16) ]                       [ Spoke 2 VNet (10.156.0.0/16) ]
   └── App VM (10.15.0.4)                                └── App VM (10.156.0.4)
          |                                                     ^
          | (UDR forces NextHop to Firewall)                    |
          v                                                     |
 +--------------------------------------------------------------+---------+
 | HUB VNET                                                               |
 |   [ Azure Firewall (Private IP: 10.10.1.4) ]                           |
 |     ├── Network Rules: Allow TCP/UDP on specific ports                 |
 |     ├── Application Rules: Allow HTTP/HTTPS to approved FQDNs          |
 |     └── Threat Intelligence: Blocks malicious IPs automatically        |
 +------------------------------------------------------------------------+
          |
          v (Inspects & SNATs)
     [ Internet ]
```

---

## 🛣️ 2. User-Defined Route (UDR) Configuration

Default Azure routing sends internet traffic straight out via default SNAT. We associate a **Route Table** with each Spoke Subnet to override this:

```hcl
resource "azurerm_route_table" "spoke_rt" {
  name                = "rt-spoke-to-firewall"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  route {
    name                   = "route-all-traffic-to-firewall"
    address_prefix         = "0.0.0.0/0"
    next_hop_type          = "VirtualAppliance"
    next_hop_in_ip_address = "10.10.1.4" # Azure Firewall Private IP
  }
}

resource "azurerm_subnet_route_table_association" "assoc" {
  subnet_id      = azurerm_subnet.spoke_subnet.id
  route_table_id = azurerm_route_table.spoke_rt.id
}
```

---

## 💡 Key Takeaways
1. Without UDRs, spoke workloads will bypass the hub firewall and connect directly to the internet.
2. Route `0.0.0.0/0` to `VirtualAppliance` with the Firewall's private IP to enforce centralized egress inspection.
