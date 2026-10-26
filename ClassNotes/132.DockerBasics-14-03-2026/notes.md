# Class Notes: Why Docker? Virtual Machines vs. Containers 🐳

**Date:** 14-03-2026  
**Topic:** Containerization Fundamentals, Hypervisors vs Container Engine, and Resource Efficiency

---

## 📌 1. Architecture: Virtualization vs. Containerization

Why did the industry move from heavy virtual machines to lightweight containers?

```
 VIRTUALIZATION (Hypervisor-Based)            CONTAINERIZATION (Docker-Based)
 +---------------------------------------+    +---------------------------------------+
 | App 1         | App 2         | App 3 |    | App 1         | App 2         | App 3 |
 +---------------+---------------+-------+    +---------------+---------------+-------+
 | Bins / Libs   | Bins / Libs   | Bins  |    | Bins / Libs   | Bins / Libs   | Bins  |
 +---------------+---------------+-------+    +---------------------------------------+
 | Guest OS      | Guest OS      | Guest |    | Docker Engine (Container Runtime)     |
 | (Ubuntu 4GB)  | (Debian 4GB)  | (RHEL)|    +---------------------------------------+
 +---------------+---------------+-------+    | Host Operating System (Linux Kernel)  |
 | Hypervisor (ESXi / KVM / Hyper-V)     |    +---------------------------------------+
 +---------------------------------------+    | Physical Hardware / Infrastructure    |
 | Physical Hardware / Infrastructure    |    +---------------------------------------+
 +---------------------------------------+
```

---

## ⚖️ 2. Key Differences

| Feature | Virtual Machines | Docker Containers |
| :--- | :--- | :--- |
| **Operating System** | Each VM bundles a complete Guest OS (kernel + userland) | Shares the Host OS kernel; zero duplicate guest OS |
| **Resource Overhead** | High (each VM consumes 1–4+ GB RAM just for OS) | Ultra-lightweight (megabytes; runs as native host process) |
| **Boot Time** | Minutes (boots BIOS, kernel, systemd services) | Milliseconds (process spawn) |
| **Density** | Limited VMs per physical server | 10x–50x higher density of containers per server |
| **Isolation** | Hardware-level isolation via Hypervisor | Process-level isolation via Linux **cgroups** & **namespaces** |

---

## 💡 Key Takeaways
1. Containers solve the *"It worked on my machine"* problem by packaging code together with exact binaries, libraries, and runtime dependencies.
2. Containers do not virtualize hardware; they isolate processes sharing the underlying host Linux kernel.
