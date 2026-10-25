# Class Notes: Container Lifecycle, Resource Limits & Autoscaling Foundations 📈

**Date:** 04-04-2026  
**Topic:** Container State Lifecycle, Memory & CPU Limits, and Lightweight NGINX Customization

---

## 📌 1. The Container Lifecycle State Machine

```
               docker create
  [ Image ] --------------------> [ Created ]
                                      |
                                      | docker start
                                      v
                                  [ Running ] <-------+
                                   |       |          |
                      docker stop  |       | docker   | docker restart
                                   v       v pause    |
                               [ Stopped ] [ Paused ]-+
                                   |
                                   | docker rm
                                   v
                             [ Destroyed ]
```

---

## ⚖️ 2. Applying Resource Constraints

Prevent noisy-neighbor containers from consuming all host RAM and CPU:

```bash
# Restrict container to 512MB RAM and 1.5 CPU cores
docker run -d --name constrained-app   --memory="512m"   --cpus="1.5"   -p 80:80 nginx:alpine
```

* If a container exceeds its memory limit, the Linux kernel terminates it with **OOMKilled** (Exit Code 137).

---

## 💡 Key Takeaways
1. Choose lightweight base images (e.g., `alpine` or `distroless`) to reduce attack surface and memory footprints.
2. Always specify memory bounds in multi-tenant environments.
