# Class Notes: Kubernetes Network Policies & Zero-Trust Pod Security 🛡️

**Date:** 04-07-2026  
**Topic:** Default-Allow vs Default-Deny, Ingress & Egress Rules, podSelector, and CNI Requirements

---

## 📌 1. Architecture: Default-Allow vs. Network Policy

By default, Kubernetes implements an open network: any pod can talk to any other pod across any namespace. **NetworkPolicy** acts as a distributed firewall restricting pod traffic:

```
 WITHOUT NetworkPolicy (Open Network):
   [ Frontend Pod ] --------(Direct Traffic Allowed)--------> [ Database Pod ]
   [ Untrusted Pod ] -------(Direct Traffic Allowed)--------> [ Database Pod ] ❌ INSECURE!

 WITH NetworkPolicy (Zero-Trust):
   [ Frontend Pod ] --------(Allowed: Matches podSelector)----> [ Database Pod ] ✅
   [ Untrusted Pod ] -------(BLOCKED: Dropped by CNI)--------> [ Database Pod ] ❌
```

---

## 📜 2. NetworkPolicy Manifest Example

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-db
  namespace: default
spec:
  # Target Pods to protect
  podSelector:
    matchLabels:
      role: database
  policyTypes:
    - Ingress
  ingress:
    # Allow traffic ONLY from pods labeled role: frontend on Port 5432
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 5432
```

* **CNI Requirement:** NetworkPolicies require an underlying CNI plugin that supports policy enforcement (e.g., Azure Network Policy, Calico, or Cilium).

---

## 💡 Key Takeaways
1. Network policies enforce east-west traffic security inside the Kubernetes cluster.
2. In production, implement a default-deny ingress policy for sensitive database and backend namespaces.
