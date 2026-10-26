# Class Notes: Docker CLI Essentials & Mastering `docker run` 🏃‍♂️

**Date:** 28-03-2026  
**Topic:** CLI Syntax, Foreground vs Background Mode, Port Publishing, and Container Lifecycle

---

## 📌 1. Anatomy of `docker run`

The `docker run` command pulls an image (if not present locally), creates the container layer, and starts the container process:

```bash
docker run -d --name my-web -p 8080:80 -v /data:/app/data --restart always nginx:latest
```

```
 docker run
   ├── -d               : Detached mode (runs in background, returns container ID)
   ├── --name my-web    : Assigns a memorable container name (instead of random names)
   ├── -p 8080:80       : Port mapping (Host Port 8080 -> Container Port 80)
   ├── -v /data:/app    : Volume mount for persistent data
   ├── --restart always : Automatic restart policy on crash or daemon restart
   └── nginx:latest     : Image name and tag
```

---

## ⚙️ 2. Essential Flags Explained

* **`-it` (Interactive TTY):** Keeps STDIN open and allocates a pseudo-TTY (used to get an interactive shell: `docker run -it ubuntu /bin/bash`).
* **`--rm`:** Automatically deletes the container when it exits (great for one-off testing scripts).
* **`-e KEY=VALUE`:** Injects environment variables into the container runtime.

---

## 💡 Key Takeaways
1. Always name production containers with `--name` for easy log inspection and monitoring.
2. Use `-d` for web servers and daemons; use `-it --rm` for interactive debugging sessions.
