# Class Notes: YAML Syntax & Kubernetes Networking Fundamentals 📜

**Date:** 07-06-2026  
**Topic:** YAML Structure, Scalars, Maps, Lists, and Pod-to-Pod Communication Models

---

## 📌 1. YAML Rules for Kubernetes Manifests

YAML (YAML Ain't Markup Language) is human-readable and indentation-sensitive:
* **Indentation:** Always use **2 spaces**. Never use TAB characters.
* **Key-Value Pairs (Maps / Dictionaries):**
  ```yaml
  apiVersion: v1
  kind: Pod
  ```
* **Lists / Arrays (Prefixed with `- `):**
  ```yaml
  containers:
    - name: web
      image: nginx
    - name: logger
      image: busybox
  ```

---

## 🌐 2. Kubernetes Networking Model (The 4 Fundamental Principles)

Kubernetes enforces a flat, non-NAT networking model across all nodes:
1. All Pods can communicate with all other Pods without NAT.
2. All Nodes can communicate with all Pods without NAT.
3. The IP a Pod sees as its own address is the exact same IP other Pods see it as.
4. Each Pod receives a unique, routable cluster IP address.

---

## 💡 Key Takeaways
1. Validate YAML syntax using linters before applying to clusters to prevent parsing errors.
2. Kubernetes assumes an open, routed IP-per-pod network across the entire cluster.
