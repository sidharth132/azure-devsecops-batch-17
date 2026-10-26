# Class Notes: Infrastructure Architecture Design Inputs & Azure Governance Recap 🏛️

**Date:** February 2026  
**Topic:** Infrastructure Architect Prerequisites, Input Gathering from Application Teams, and Azure Governance

---

## 📌 1. What Inputs Does an Infrastructure Architect Need?

Before designing a cloud High-Level Design (HLD) for a 3-tier monolithic or microservices application, the Cloud/Infra Architect must collect key requirements from application architects:

```
 APPLICATION ARCHITECT INPUTS:
   ├── 1. Traffic Expectations (RPS, concurrent users, peak load)
   ├── 2. SLA & High Availability Requirements (99.9% vs 99.99%)
   ├── 3. RTO and RPO Targets (Disaster Recovery expectations)
   ├── 4. Technology Stack (OS, runtime versions: Java/Node/Python, Middleware)
   ├── 5. Compliance & Regulatory Standards (PCI-DSS, HIPAA, GDPR)
   └── 6. Database Characteristics (Read/Write ratios, IOPS requirements)
                    |
                    v
 INFRASTRUCTURE ARCHITECT DELIVERABLES:
   ├── VNet CIDR sizing and subnet segmentation
   ├── Compute sizing (VM sizes, scale sets, autoscale metrics)
   ├── Storage tiers and redundancy levels (LRS, ZRS, GRS)
   └── Security architecture (WAF, NSG, Key Vault, Private Link)
```

---

## 🛡️ 2. Azure Governance Hierarchy

* **Management Groups:** Apply organization-wide governance, policies, and compliance across multiple subscriptions.
* **Subscriptions:** Billing and resource quota boundary.
* **Resource Groups:** Lifecycle and access control boundary for related resources.
* **Role-Based Access Control (RBAC):** Assign least-privilege permissions to Users, Groups, and Managed Identities.

---

## 💡 Key Takeaways
1. Never design infrastructure in a vacuum; all compute, storage, and networking choices depend directly on application load profiles.
2. Enforce governance early using Azure Policy and landing zone design principles.
