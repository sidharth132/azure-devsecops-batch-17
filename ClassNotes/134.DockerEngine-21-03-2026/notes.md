# Class Notes: Docker Engine Deep Dive & Linux Kernel Primitives ⚙️

**Date:** 21-03-2026  
**Topic:** Linux Kernel Isolation, Namespaces, Control Groups (cgroups), and Storage Drivers

---

## 📌 1. How Docker Implements Isolation

Containers are not magical boxes; they are standard Linux processes isolated using two core Linux kernel features:

```
                      [ Linux Host Operating System ]
                                     |
    +--------------------------------+--------------------------------+
    |                                                                 |
    v                                                                 v
 [ Linux Namespaces ] (ISOLATION)                  [ Control Groups (cgroups) ] (RESOURCE LIMITS)
  - PID: Process IDs (Process sees itself as PID 1)  - CPU Quotas: Max 2 CPU cores
  - NET: Network stack (isolated IP & port space)   - Memory Limits: Max 1 GiB RAM
  - MNT: Mount points (isolated root filesystem)    - Block I/O: Disk read/write throttling
  - UTS: Hostname and domain name isolation
  - IPC: Inter-process communication isolation
```

---

## 🛡️ 2. Linux Kernel Primitives Explained

* **Namespaces:** Provide what a container **can see**. Ensures container A cannot see the processes, network interfaces, or filesystem of container B or the host.
* **cgroups (Control Groups):** Provide what a container **can use**. Restricts memory, CPU, and disk I/O to prevent a single runaway container from starving the host system (noisy neighbor problem).

---

## 💡 Key Takeaways
1. A container is a standard Linux process running under specific namespaces and restricted by cgroups.
2. Always define memory and CPU limits on containers to protect host stability.
