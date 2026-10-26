# Class Notes: Axion Systems: Database Deployment & Connection Architecture 🗄️

**Date:** 09-08-2026  
**Topic:** Deploying PostgreSQL on Kubernetes, Database Schemas, StatefulSets, and Service Bindings

---

## 📌 1. Database Deployment Architecture

Deploying relational databases on Kubernetes requires strict persistence and stable network identity:

```
 [ PostgreSQL Pod ] (StatefulSet)
   ├── Environment Variables: POSTGRES_DB, POSTGRES_USER, POSTGRES_PASSWORD (via Secret)
   └── Volume Mount: /var/lib/postgresql/data tied to PersistentVolumeClaim (managed-csi)
         ^
         |
 [ PostgreSQL ClusterIP Service: postgres-service (Port 5432) ]
         ^
         | (Queries telemetry and records events)
 [ Backend Microservices (Telemetry Query & Ingestion Services) ]
```

---

## ⚙️ 2. Initializing the Database Schema

* Schema definitions stored in `axion-database-schema`.
* Defines tables: `devices`, `sensor_telemetry`, `alerts`.
* Automated schema migration jobs ensure tables exist before application microservices launch.

---

## 💡 Key Takeaways
1. In production, prefer managed cloud databases (Azure Database for PostgreSQL) over self-hosted Kubernetes database pods.
2. If hosting on AKS, always back database pods with PersistentVolumeClaims on premium SSD storage.
