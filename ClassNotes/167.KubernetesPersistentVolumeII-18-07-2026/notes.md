# Class Notes: Dynamic Provisioning with StorageClasses (SC) ⚡

**Date:** 18-07-2026  
**Topic:** Static vs Dynamic Provisioning, StorageClass Parameters, and Volume Binding Modes

---

## 📌 1. Static vs. Dynamic Volume Provisioning

```
 STATIC PROVISIONING (Manual / Painful):
   1. Cloud Admin manually creates Azure Disk in Azure Portal.
   2. Admin writes PersistentVolume (PV) YAML referencing Azure Disk URI.
   3. Developer writes PVC.
   ❌ Slow, does not scale, requires manual intervention for every pod.

 DYNAMIC PROVISIONING (Automated / Cloud Native):
   1. Admin creates a StorageClass (e.g., 'managed-csi' using Azure Disk).
   2. Developer requests a PVC referencing storageClassName: managed-csi.
   3. Kubernetes CSI Driver AUTOMATICALLY creates the Azure Disk in Azure ARM
      and binds the PV on the fly!
   ✅ 100% automated, zero admin intervention required.
```

---

## 📜 2. Dynamic PVC Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: managed-csi
  resources:
    requests:
      storage: 20Gi
```

---

## 💡 Key Takeaways
1. Modern cloud Kubernetes relies almost exclusively on **Dynamic Provisioning** using StorageClasses.
2. In AKS, default StorageClasses (`managed-csi`, `azurefile-csi`) are pre-installed out of the box.
