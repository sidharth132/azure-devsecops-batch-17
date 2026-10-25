# Class Notes: Kubernetes Storage: The Need for Persistent Volumes 💾

**Date:** 05-07-2026  
**Topic:** Ephemeral Containers, Data Loss Risks, EmptyDir vs Persistent Volumes

---

## 📌 1. The Ephemeral Nature of Pods

**"Pod is Ephemeral (अल्पकालिक)."**  
Pods are designed to be disposable. When a pod crashes, is deleted, or rescheduled by a node restart or cluster upgrade:
* The container filesystem is completely wiped.
* If a database (e.g., PostgreSQL or MongoDB) stores its data inside the container filesystem, **all data is permanently lost!**

```
 Pod Crashes / Upgrades:
   [ Container Filesystem ] =====> DELETED & RE-CREATED EMPTY ❌ (Data Lost)

 With External Persistent Volume:
   [ Pod ] ----(Mounts)----> [ Persistent Volume (Azure Disk / NetApp) ]
                                |
     (If Pod dies, new Pod mounts the SAME volume -> Data Preserved! ✅)
```

---

## 📁 2. Temporary Storage: `emptyDir`

* Allocated when a Pod is assigned to a node.
* Initially empty; shared between containers in the same pod.
* **Lifespan:** Tied directly to the Pod. If the Pod is deleted, the `emptyDir` data is deleted forever. Useful only for scratch spaces, cache, or logs.

---

## 💡 Key Takeaways
1. Never store production state or database records on the ephemeral container filesystem.
2. Decouple storage lifecycle from compute lifecycle using Persistent Volumes.
