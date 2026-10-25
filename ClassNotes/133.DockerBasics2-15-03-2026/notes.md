# Class Notes: Docker Architecture & Core Components 🏗️

**Date:** 15-03-2026  
**Topic:** Docker Client-Server Architecture, Docker Daemon (dockerd), Containerd, and Registries

---

## 📌 1. Architecture: Docker Client-Server Model

Docker follows a client-server architecture communicating over local Unix sockets or REST API:

```
 [ Docker Client ] (CLI)
   └── User types: docker run, docker build, docker pull
            |
            v (REST API over /var/run/docker.sock)
 [ Docker Host ]
   ├── Docker Daemon (dockerd)
   │     └── Manages Images, Networks, Volumes, Containers
   │
   ├── Containerd (High-level container runtime)
   │     └── runc (Low-level OCI runtime interacting with Linux kernel)
   │
   └── Local Storage (/var/lib/docker)
            |
            v (Network / HTTPS)
 [ Docker Registry ] (Docker Hub / Azure Container Registry)
   └── Public and private image repositories
```

---

## 🧩 2. Core Components Explained

1. **Docker Client (`docker`):** The primary command-line interface used by engineers to instruct Docker.
2. **Docker Daemon (`dockerd`):** Persistent background service managing containers, images, volumes, and networks.
3. **Containerd & runc:** Industry-standard container runtime executing containers according to the OCI (Open Container Initiative) specification.
4. **Docker Registry:** Centralized store for versioned container images (e.g., Docker Hub, ACR, ECR).

---

## 💡 Key Takeaways
1. The CLI is merely a client; the Docker daemon (`dockerd`) performs the heavy lifting.
2. Docker adheres to open container standards (OCI) through containerd and runc.
