# Class Notes: Cloud Monitoring & Observability: Prometheus & Grafana 📊

**Date:** 24-10-2026  
**Topic:** Observability Pillars, Prometheus Pull Architecture, Exporters, and Grafana Dashboards

---

## 📌 1. Architecture: The Prometheus & Grafana Monitoring Stack

Observability allows engineering teams to detect, diagnose, and resolve production issues before users are affected:

```
 [ Kubernetes Cluster Workloads ]
   ├── Node Exporter (Gathers host CPU, Memory, Disk, Network metrics)
   ├── kube-state-metrics (Gathers Pod counts, Deployment statuses, restarts)
   └── Application Pods (Exposes /metrics endpoint)
            ^
            | (Scrapes HTTP metrics every 15s - PULL MODEL)
 [ Prometheus Server ]
   ├── Time-Series Database (TSDB)
   ├── PromQL Query Engine
   └── Alertmanager (Sends Slack / PagerDuty alerts on high CPU or crash loops)
            ^
            | (Queries metrics via PromQL)
 [ Grafana Visualization Server ]
   └── Interactive Dashboards (Cluster Health, Node Capacity, Pod Latency)
```

---

## 🔍 2. Push vs. Pull Monitoring Models

* **Prometheus Uses the Pull Model:** The Prometheus server periodically scrapes HTTP `/metrics` endpoints exposed by monitored targets.
* **Advantages:** Central server controls scrape frequency; targets do not need configuration pointing to monitoring servers.

---

## 💡 Key Takeaways
1. Install Prometheus and Grafana using the community `kube-prometheus-stack` Helm chart.
2. Build dashboards tracking the **Four Golden Signals**: Latency, Traffic, Errors, and Saturation.
