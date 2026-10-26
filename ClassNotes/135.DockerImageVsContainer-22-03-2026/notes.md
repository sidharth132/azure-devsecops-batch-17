# Class Notes: Docker Images vs. Containers & Storage Layers 📦

**Date:** 22-03-2026  
**Topic:** Image Immutability, Copy-on-Write (CoW), Container Layers, and Storage Drivers

---

## 📌 1. Image vs. Container Analogy

```
   Docker Image (Blueprint / Class)  ----(docker run)---->  Docker Container (Live Instance / Object)
   - Read-Only template                                      - Running process
   - Frozen in time                                          - Has state, memory, network IP
   - Stored in registry / disk                               - Has a thin Read-Write container layer
```

---

## 🍰 2. The Layered Storage Architecture (Union File System)

Docker images are composed of multiple **read-only layers**. When a container is launched, Docker adds a thin **Read-Write Layer** on top:

```
 +---------------------------------------------------------------+
 | Container Layer (Read-Write Layer) - Created on docker run   |  <-- Writes, temp files, logs
 +---------------------------------------------------------------+
 | Layer 3: Application Code (Read-Only)                         |  <-- FROM image
 +---------------------------------------------------------------+
 | Layer 2: Installed Dependencies / NGINX (Read-Only)          |  <-- FROM image
 +---------------------------------------------------------------+
 | Layer 1: Base OS Rootfs: Ubuntu / Alpine (Read-Only)          |  <-- FROM image
 +---------------------------------------------------------------+
```

* **Copy-on-Write (CoW):** If a container modifies an existing file from a lower image layer, Docker makes a copy of the file into the top Read-Write layer and modifies the copy, leaving the underlying image pristine.
* **Ephemerality:** When a container is deleted (`docker rm`), its top Read-Write layer is destroyed, and all unstored data is permanently lost.

---

## 💡 Key Takeaways
1. Docker images are immutable; they can never be modified once built.
2. Persistent data must be stored outside the container layer using Docker Volumes or Bind Mounts.
