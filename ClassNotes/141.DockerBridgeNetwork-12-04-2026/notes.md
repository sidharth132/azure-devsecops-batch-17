# Class Notes: Docker Networking: Bridge Networks, Host Mode & DNS Discovery 🌐

**Date:** 12-04-2026  
**Topic:** Docker Network Drivers, Default Bridge (`docker0`), Custom User-Defined Bridge, and Container DNS

---

## 📌 1. Architecture: Docker Bridge Network

Docker creates a software bridge interface (`docker0` on `172.17.0.1/16`) to route traffic between containers on the same host:

```
 [ Docker Host ]
   ├── docker0 Bridge (172.17.0.1)
   │     ├── veth0 <---> Container 1 (172.17.0.2, NGINX:80)
   │     └── veth1 <---> Container 2 (172.17.0.3, Python API:5000)
   │
   └── NAT (iptables) enables outbound access to internet
```

---

## ⚖️ 2. Default Bridge vs. User-Defined Custom Bridge

| Feature | Default Bridge (`bridge`) | Custom Bridge (`docker network create`) |
| :--- | :--- | :--- |
| **Automatic DNS Resolution** | ❌ No (Can only connect via volatile IP addresses) | **✅ Yes (Containers resolve each other by container name!)** |
| **Isolation** | All containers attach by default (low security) | Dedicated network per project (isolated from unrelated containers) |
| **Hot Connect/Disconnect** | ❌ Must restart container | ✅ Can attach/detach on the fly |

### Creating and Using a Custom Bridge:
```bash
# 1. Create isolated network
docker network create todo-network

# 2. Attach containers to the network
docker run -d --name backend-api --network todo-network api-image
docker run -d --name frontend-ui --network todo-network -p 80:80 ui-image

# 3. Frontend can now reach backend using URL: http://backend-api:5000
```

---

## 💡 Key Takeaways
1. Never use the default bridge in production; always create a **User-Defined Bridge** network so containers resolve each other by name.
2. Custom bridge networks provide automatic service discovery and network-level isolation.
