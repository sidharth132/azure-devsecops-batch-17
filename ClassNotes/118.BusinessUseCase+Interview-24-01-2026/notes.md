# Class Notes: Enterprise Software Development Lifecycle & Role Coordination 🏢

**Date:** 24-01-2026  
**Topic:** Enterprise SDLC, Stakeholder Roles, High-Level vs Low-Level Design (HLD/LLD), and Business Delivery

---

## 📌 1. Enterprise Team Roles & Architecture Handoff

How technical requirements flow through an enterprise software organization:

```
 [ Client / Stakeholders ]
            | (Business Needs)
            v
 [ Product Owner (PO) ] ------------------> Generates Functional Requirements Document (FRD)
            |
            v
 [ Technical Product Manager (TPM) ]
            |
            v
 [ Enterprise / Cloud Architect ] --------> Creates High-Level Design (HLD) & Low-Level Design (LLD)
            |
            +---------------------------------------+
            |                                       |
            v                                       v
 [ Frontend / Backend Developers ]      [ DevOps / Cloud Engineers ]
 (Write application microservices)      (Build Terraform infra & CI/CD pipelines)
```

---

## 📋 2. High-Level Design (HLD) vs. Low-Level Design (LLD)

| Design Document | Scope | Audience | Details Included |
| :--- | :--- | :--- | :--- |
| **High-Level Design (HLD)** | System-wide architecture overview | Executives, Architects, PMs | Network topology, VPC/VNet peering, cloud services used, security boundaries |
| **Low-Level Design (LLD)** | Deep technical implementation blueprint | Developers, DevOps Engineers | Subnet CIDRs, NSG rule priority tables, API schemas, database ER diagrams, exact pipeline YAML |

---

## 💡 Key Takeaways
1. In senior interviews, demonstrate awareness of the entire delivery lifecycle from business requirement to production deployment.
2. DevOps engineers bridge application development and cloud infrastructure by implementing the architect's HLD/LLD via automated code.
