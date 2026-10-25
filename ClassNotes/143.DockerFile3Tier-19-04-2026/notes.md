# Class Notes: Multi-Tier Dockerization & Inter-Container Communication 🚀

**Date:** 19-04-2026  
**Topic:** Containerizing a 3-Tier Application (Frontend, Backend, Database) and Connecting Them Locally

---

## 📌 1. Architecture: 3-Tier Local Container Architecture

Connecting UI, API, and Database containers on a single host using custom bridge networking:

```
  User Browser: http://localhost:8080
         |
         v
  [ Frontend Container: todo-ui ] (Port 80)
    - Serves static React bundle via NGINX
    - Forwards /api calls to backend
         |
         v (Internal DNS: http://todo-backend:5000)
  [ Backend Container: todo-backend ] (Port 5000)
    - Node.js / Express or Python API
    - Connects to database
         |
         v (Internal DNS: db:5432)
  [ Database Container: todo-db ] (Port 5432)
    - PostgreSQL with persistent Docker Volume attached
```

---

## 🛠️ 2. Step-by-Step Execution Workflow

```bash
# Step 1: Create isolated network
docker network create todo-app-net

# Step 2: Launch Database with volume
docker run -d --name todo-db   --network todo-app-net   -e POSTGRES_DB=tododb   -e POSTGRES_PASSWORD=secret   -v pgdata:/var/lib/postgresql/data   postgres:15-alpine

# Step 3: Launch Backend connected to DB by container name
docker run -d --name todo-backend   --network todo-app-net   -e DB_HOST=todo-db   -e DB_NAME=tododb   todo-backend:v1

# Step 4: Launch Frontend with host port published
docker run -d --name todo-ui   --network todo-app-net   -p 8080:80   todo-frontend:v1
```

---

## 💡 Key Takeaways
1. All containers communicate seamlessly using service names as hostnames on the shared custom network.
2. Only the frontend container publishes its port to the host; backend and database containers remain protected inside the internal network.
