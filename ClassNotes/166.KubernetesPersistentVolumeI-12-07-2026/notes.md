# Class Notes: PersistentVolumes (PV) & PersistentVolumeClaims (PVC) 🧱

**Date:** 12-07-2026  
**Topic:** PV vs PVC Analogy, Access Modes, Reclaim Policies, and Static Provisioning

---

## 📌 1. The Plug-and-Socket Analogy (PV vs. PVC)

Kubernetes separates storage administration from developer consumption:

```
 +-------------------------------------------------------------------------+
 | INFRASTRUCTURE TEAM (Admin)                                             |
 |   Provisions raw storage: Azure Managed Disk, NFS, SAN                  |
 |   Creates: PersistentVolume (PV) = The Electrical Socket                |
 +-------------------------------------------------------------------------+
                                      |
                                      | (Automatic Binding: Capacity & Access)
                                      v
 +-------------------------------------------------------------------------+
 | APPLICATION DEVELOPER (User)                                            |
 |   Requests storage: "I need 10 GiB with ReadWriteOnce access"           |
 |   Creates: PersistentVolumeClaim (PVC) = The Electrical Plug            |
 |   Mounts PVC into Pod spec                                              |
 +-------------------------------------------------------------------------+
```

---

## 🔑 2. Access Modes Explained

* **`ReadWriteOnce` (RWO):** Volume can be mounted as read-write by a **single node** only (e.g., Azure Managed Disk).
* **`ReadOnlyMany` (ROX):** Volume can be mounted as read-only by **many nodes** simultaneously.
* **`ReadWriteMany` (RWX):** Volume can be mounted as read-write by **many nodes** concurrently (e.g., Azure Files / NFS share).

---

## 💡 Key Takeaways
1. A PVC specifies storage size and access mode; Kubernetes automatically matches and binds it to a compatible PV.
2. Reclaim policies (`Retain` vs `Delete`) control whether underlying cloud storage is kept or purged when the PVC is deleted.
