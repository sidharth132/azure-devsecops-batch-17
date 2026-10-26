# Class Notes: Managing Docker Containers, Logs & Remote Server Debugging 🛠️

**Date:** 29-03-2026  
**Topic:** Container Inspection, Troubleshooting, Log Management, and Remote Host VS Code Connectivity

---

## 📌 1. Essential Container Management Commands

```bash
# List running containers
docker ps

# List ALL containers (including exited/stopped)
docker ps -a

# View container output logs
docker logs -f <container_name>

# Execute a command inside a running container
docker exec -it <container_name> /bin/bash

# Stop and remove containers
docker stop <container_name>
docker rm <container_name>

# Remove unused images and containers (Cleanup)
docker system prune -af
```

---

## 🔍 2. Troubleshooting Inside Containers

When a container fails or behaves unexpectedly:
1. `docker logs <container_name>`: Inspect stdout/stderr logs.
2. `docker inspect <container_name>`: Displays comprehensive JSON metadata: IP address, port bindings, environment variables, mounts, and exit codes.
3. `docker exec -it <container_name> sh`: Enters container filesystem to verify files and connectivity.

---

## 💡 Key Takeaways
1. `docker exec` runs a new process inside an existing container without restarting it.
2. Regularly execute `docker system prune` on build servers to prevent disk exhaustion from dangling images.
