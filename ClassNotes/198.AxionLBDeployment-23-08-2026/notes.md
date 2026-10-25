# Class Notes: Axion Systems: Load Balancers, PgAdmin & External Management ⚖️

**Date:** 23-08-2026  
**Topic:** Public LoadBalancer Services, Deploying PgAdmin for DBA Management, and Port Allocations

---

## 📌 1. Architecture: PgAdmin Management Access

Exposing the database administration UI securely for DBAs:

```
 [ DBA Browser ]
        |
        v (Public IP: 135.237.18.16:8080)
 [ Azure Public Load Balancer ]
        |
        v
 [ PgAdmin Service (Type: LoadBalancer) ]
        |
        v
 [ PgAdmin Pod ] ---------------------> [ PostgreSQL Pod (ClusterIP:5432) ]
 (Web Interface for Database)            (Internal Database)
```

---

## 🔒 2. Security Considerations

* Never expose the raw database port (5432) directly to the public internet.
* Restrict PgAdmin ingress to corporate VPN IP ranges using `loadBalancerSourceRanges` or Network Security Groups.

---

## 💡 Key Takeaways
1. Database administration tooling should always be protected with IP whitelisting.
2. Only expose user-facing web endpoints to the public internet.
