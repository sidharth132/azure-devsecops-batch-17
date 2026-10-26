# Class Notes: Docker Port Mapping & Host-to-Container Traffic Flow 🔌

**Date:** 05-04-2026  
**Topic:** Port Forwarding (`-p`), Container Ports vs Host Ports, and IPTables NAT

---

## 📌 1. How Port Forwarding Works

Containers run inside their own private network namespace with private IP addresses (e.g., `172.17.0.2`). External clients on the host network cannot reach this IP directly without **Port Mapping**:

```
 Client Request: http://localhost:8090
          |
          v
 [ Docker Host ] (Listening on Host Port 8090)
          |
          | (IPTables DNAT Rule forwards 8090 -> 172.17.0.2:80)
          v
 [ Docker Container: dhondhu ]
   └── NGINX listening on Container Port 80
```

---

## ⚙️ 2. Command Syntax: `-p HostPort:ContainerPort`

```bash
# Maps host port 8090 to container port 80
docker run -d --name dhondhu -p 8090:80 nginx:latest
```

* **Host Port (`8090`):** The port exposed to the outside world on the host machine.
* **Container Port (`80`):** The port the application daemon inside the container is configured to listen on.

### Finding Application Document Roots:
* In standard NGINX container: `/usr/share/nginx/html`
* Find command: `docker exec -it <container> find / -iname "index.html"`

---

## 💡 Key Takeaways
1. Multiple containers can listen on port 80 internally, as long as they bind to different external host ports (`-p 8081:80`, `-p 8082:80`).
2. Omitting `-p` makes the container accessible only from within the internal Docker bridge network.
