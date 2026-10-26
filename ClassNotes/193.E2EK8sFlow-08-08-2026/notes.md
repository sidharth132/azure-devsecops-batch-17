# Class Notes: Axion Systems E2E Flow: MonoRepo vs PolyRepo & Cloud Architecture 🏭

**Date:** 08-08-2026  
**Topic:** Enterprise Multi-Service Architecture, MonoRepo vs PolyRepo, and Axion Systems Architecture

---

## 📌 1. Architecture: The Axion Systems Enterprise Platform

Axion Systems is an industrial IoT dashboard monitoring real-time telemetry (temperature, RPM, pressure) across rotating refinery components:

```
 [ Industrial IoT Sensors ] ----> [ Ingestion Service ] ----> [ PostgreSQL Database ]
                                                                      ^
                                                                      |
 [ Web Dashboard (Axion UI) ] <---> [ Telemetry Query Service ] -------+
```

---

## 🗄️ 2. MonoRepo vs. PolyRepo (Multi-Repo)

| Criteria | MonoRepo | PolyRepo (Multi-Repo) - Axion Strategy |
| :--- | :--- | :--- |
| **Code Storage** | All services in a single Git repository | Each microservice in its own dedicated Git repository |
| **Access Control** | Hard to restrict developer access per service | Strict least-privilege access per repository |
| **CI/CD Pipelines**| Complex path-filter triggers needed | Clean, isolated pipelines per service |
| **Axion Repositories:** |
| - `axion-ui`: Frontend dashboard (React) |
| - `axion-ingestion-service`: Ingestion API |
| - `axion-telemetry-query-service`: Query API |
| - `axion-data-simulator`: Simulates IoT hardware |
| - `axion-database-schema`: Liquibase / SQL migrations |

---

## 💡 Key Takeaways
1. PolyRepo isolates team ownership, version tagging, and CI/CD triggers in microservice architectures.
2. Structure microservices to communicate via asynchronous messaging or dedicated query APIs rather than sharing direct database access.
