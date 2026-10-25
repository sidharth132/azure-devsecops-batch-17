# Class Notes: Cloud Service Models, Imperative vs Declarative & Azure Entra ID ☁️

**Date:** 12-01-2025  
**Topic:** Cloud Computing (IaaS, PaaS, SaaS), Imperative vs Declarative Automation, Token-Based IAM, and Azure Networking Foundations

---

## 📌 1. Overview & Cloud Service Models

Cloud computing provides on-demand IT resources over the internet with pay-as-you-go pricing. Microsoft Azure ("Microsoft ki Dukan") provides infrastructure, platforms, and software solutions.

### Shared Responsibility Model

```
+--------------------+------------+------------+------------+------------+
| Layer              | On-Premises|    IaaS    |    PaaS    |    SaaS    |
+--------------------+------------+------------+------------+------------+
| Application Code   | Customer   | Customer   | Customer   | Provider   |
| Middleware/Runtimes| Customer   | Customer   | Provider   | Provider   |
| Operating System   | Customer   | Customer   | Provider   | Provider   |
| Virtualization     | Customer   | Provider   | Provider   | Provider   |
| Hardware/Servers   | Customer   | Provider   | Provider   | Provider   |
| Networking         | Customer   | Provider   | Provider   | Provider   |
| Cooling & Facility | Customer   | Provider   | Provider   | Provider   |
+--------------------+------------+------------+------------+------------+
```

* **On-Premises:** Customer manages everything from physical servers, cooling, electricity to OS and application code.
* **IaaS (Infrastructure as a Service):** Cloud provider provides virtual machines, raw storage, and virtual networks (e.g., Azure Virtual Machines). Customer controls OS and application stack.
* **PaaS (Platform as a Service):** Cloud provider manages infrastructure and OS runtimes. Customer only provides application code (e.g., Azure App Services, AKS).
* **SaaS (Software as a Service):** Complete turnkey application hosted by provider (e.g., Microsoft 365, Outlook, Teams).

---

## ⚡ 2. Imperative vs. Declarative Automation

There are two primary paradigms for provisioning cloud infrastructure:

| Criteria | Imperative Approach | Declarative Approach |
| :--- | :--- | :--- |
| **Philosophy** | **"HOW"** to do it step-by-step | **"WHAT"** end state is desired |
| **Analogy** | Teaching a chef step-by-step: *"Take pot -> add milk -> turn on gas -> add tea leaves -> boil"* | Ordering at restaurant: *"I want two cups of tea."* |
| **Tooling** | Azure CLI (`az`), Azure PowerShell (`Az`), Bash scripts | **Terraform (HCL)**, Azure Bicep, ARM Templates |
| **Idempotency** | Difficult; scripts must check if resources exist | Built-in; engine reconciles differences automatically |
| **State Tracking** | No built-in state mechanism | Managed via state files (e.g., `terraform.tfstate`) |
| **Industry Value** | Good for ad-hoc tasks | **Industry standard for production DevSecOps** |

---

## 🔐 3. Identity and Access Management (IAM) & Token-Based Security

### Authentication vs. Authorization

* **Authentication (AuthN):** "Who are you?" Validates identity using credentials (Username, Password, MFA).
* **Authorization (AuthZ):** "What are you permitted to do?" Enforces permissions and Role-Based Access Control (RBAC).

### Whiteboard Token Analogy (Waterpark Slide / Ramu Kaka)
* At the gate, you present your ID and purchase access (Authentication).
* You receive a stamped token / wristband indicating which slides you can enter (e.g., High Slide vs. Kids Slide).
* The guard at each slide checks your token before letting you in (Authorization).

### Microsoft Entra ID (formerly Azure Active Directory)
1. User authenticates against Microsoft Entra ID (`login.microsoftonline.com`).
2. Entra ID generates a signed **JSON Web Token (JWT)** containing claims, tenant identity, and roles.
3. User / Service Principal presents the JWT to Azure Resource Manager (`portal.azure.com` / ARM API).
4. ARM validates the token signature and evaluates RBAC permissions before executing actions.
5. **Tenant ID:** Globally unique GUID representing your organization's dedicated Entra ID directory.

---

## 🌐 4. Azure Networking Roadmap Preview

Foundational infrastructure components covered:
* **Virtual Networks (VNets):** Isolated private networks in Azure.
* **Subnets:** Logical subdivisions of a VNet address space (Frontend, Backend, Database).
* **VNet Peering:** Connecting two VNets over Azure's private backbone.
* **Hybrid Connectivity:** Point-to-Site (P2S) VPN, Site-to-Site (S2S) VPN, and ExpressRoute.
* **Security & Traffic Control:** Network Security Groups (NSGs), Azure Firewall, Application Gateway, and Load Balancers.

---

## 💡 Key Takeaways
1. Declarative Infrastructure as Code (Terraform) is preferred over imperative scripts for reproducible enterprise deployments.
2. Entra ID is the centralized identity authority across Microsoft cloud ecosystems; RBAC enforces least-privilege authorization.
