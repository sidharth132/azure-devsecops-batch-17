# Class Notes: Service Endpoints vs. Private Endpoints (Private Link) 🔒

**Date:** 08-03-2026  
**Topic:** PaaS Security, Service Endpoints vs Private Endpoints, Private Link, and Private DNS Integration

---

## 📌 1. Architecture: Service Endpoint vs. Private Endpoint

Securing Azure PaaS services (Azure SQL, Storage Accounts, Key Vault):

```
 APPROACH 1: Service Endpoint
   VM (Subnet) -------------(Azure Backbone)-------------> PaaS Service (Public IP)
   - PaaS service keeps public IP, but firewall allows traffic only from your subnet.
   - Traffic does not leave Azure backbone.

 APPROACH 2: Private Endpoint (Private Link) - BEST PRACTICE
   VM (Subnet) -------------(Private IP in Subnet)-------> Private Endpoint (NIC) ---> PaaS Service
   - PaaS service is assigned a PRIVATE IP directly inside your subnet (e.g. 10.24.2.5).
   - Public IP can be COMPLETELY DISABLED. Zero internet exposure.
```

---

## ⚖️ 2. Comparison Matrix

| Feature | Service Endpoint | Private Endpoint (Private Link) |
| :--- | :--- | :--- |
| **IP Address Used** | Public IP of PaaS service | **Private IP inside your Subnet** |
| **Public Access** | Must remain enabled (filtered by VNet) | **Can be completely disabled (Deny All)** |
| **On-Premises Access** | Cannot be accessed over VPN/ExpressRoute | **Fully accessible via VPN / ExpressRoute** |
| **Cost** | Free of charge | Small hourly fee per endpoint + processed GB |
| **DNS Requirements** | Standard public DNS | Requires **Azure Private DNS Zone** integration |

---

## 💡 Key Takeaways
1. Private Endpoints are the gold standard for enterprise PaaS security because they eliminate public exposure entirely.
2. Private Endpoints enable seamless hybrid access from on-premises datacenters over VPN and ExpressRoute.
