# Class Notes: Kubernetes Volumes & Decoupling Compute from Storage 🔌

**Date:** 11-07-2026  
**Topic:** Volume Mounts (`volumeMounts`), HostPath, Cloud Disks, and Storage Lifecycles

---

## 📌 1. The USB Drive Analogy

Think of a Kubernetes Volume like a USB flash drive or external hard drive:
* Your laptop (Pod / Container) has its internal hard drive (Container layer).
* When you plug in a USB flash drive (Volume) into a USB port (`volumeMounts`), your files are read and written to external physical media.
* If your laptop breaks or you replace it, you unplug the USB drive and plug it into a new laptop; all your photos and data remain intact.

---

## 🛠️ 2. Volume Syntax in Pod Spec

```yaml
spec:
  containers:
    - name: web
      image: nginx
      volumeMounts:
        # Where to mount inside the container filesystem
        - name: cache-vol
          mountPath: /var/cache/nginx
  volumes:
    # Volume definition
    - name: cache-vol
      emptyDir: {}
```

---

## 💡 Key Takeaways
1. `.spec.volumes`: Defines the storage source available to the pod.
2. `.spec.containers[*].volumeMounts`: Injects the defined volume into specific directories within container filesystems.
