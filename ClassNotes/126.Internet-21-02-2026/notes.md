# Class Notes: Networking Fundamentals: IP Addressing, CIDR & Subnet Sizing 🔢

**Date:** 21-02-2026  
**Topic:** IPv4 Fundamentals, CIDR Notation, Subnet Calculation, and VNet Address Planning

---

## 📌 1. CIDR (Classless Inter-Domain Routing) Explained

IP addresses consist of 32 bits divided into 4 octets. CIDR notation (`/prefix`) specifies how many bits represent the network prefix, leaving the remaining bits for host addresses.

### Formula for Usable IP Addresses:
$$\text{Total IPs} = 2^{(32 - \text{CIDR})}$$

| CIDR Prefix | Subnet Mask | Total IPs | Azure Usable IPs (Total - 5) | Best Used For |
| :--- | :--- | :--- | :--- | :--- |
| `/16` | `255.255.0.0` | 65,536 | 65,531 | Complete VNet Address Space |
| `/24` | `255.255.255.0`| 256 | 251 | Standard App/Frontend Subnet |
| `/27` | `255.255.255.224`| 32 | 27 | Azure Firewall / Gateway Subnet |
| `/28` | `255.255.255.240`| 16 | 11 | Bastion / Small Service Subnet |

---

## ⚠️ 2. The 5 Reserved IP Addresses in Azure Subnets

In traditional on-premises networking, 2 IP addresses are reserved per subnet (Network ID and Broadcast). **Azure reserves 5 IP addresses in EVERY subnet**:

* `x.x.x.0`: Network Address.
* `x.x.x.1`: Default Gateway assigned by Azure.
* `x.x.x.2`, `x.x.x.3`: Azure DNS mapping addresses.
* `x.x.x.255`: Network Broadcast address.

---

## 💡 Key Takeaways
1. Always account for Azure's 5 reserved IP addresses when planning subnet CIDR sizes.
2. Design non-overlapping address spaces across VNets to allow future peering and hybrid VPN connections.
