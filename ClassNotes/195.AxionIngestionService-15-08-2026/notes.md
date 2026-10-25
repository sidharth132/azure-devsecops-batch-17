# Class Notes: Axion Systems: Ingestion Service & Data Simulator Deployment 📡

**Date:** 15-08-2026  
**Topic:** High-Throughput Ingestion, IoT Data Simulation, and Database Write Optimization

---

## 📌 1. Architecture: Real-Time Telemetry Pipeline

```
 [ axion-data-simulator ] (Generates fake IoT device telemetry)
        |
        | (HTTP POST JSON payload: device_id, temperature, rpm)
        v
 [ axion-ingestion-service ] (Validates and batches sensor packets)
        |
        | (INSERT INTO sensor_telemetry)
        v
 [ PostgreSQL Database ]
        ^
        | (SELECT * WHERE temperature > 85°C)
 [ axion-telemetry-query-service ]
        ^
        |
 [ axion-ui ] (Highlights overheating refinery machinery)
```

---

## ⚙️ 2. Telemetry Processing Rules

* **Ingestion Microservice:** Validates temperature and RPM thresholds.
* **Alert Triggering:** Machinery operating above 85°C triggers immediate alert events in the telemetry database.

---

## 💡 Key Takeaways
1. Separate ingestion workloads from query workloads to prevent bulk writes from locking analytical reads.
2. Ingest telemetry using lightweight, non-blocking asynchronous APIs.
