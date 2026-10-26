# Class Notes: Horizontal Pod Autoscaler (HPA) & Metrics Server 📈

**Date:** 12-09-2026  
**Topic:** HPA Autoscaling Algorithm, Metrics-Server Architecture, CPU/Memory Targets, and Stabilization Windows

---

## 📌 1. Architecture: How HPA Works

The Horizontal Pod Autoscaler automatically scales the number of pod replicas based on observed CPU or memory utilization:

```
 [ Metrics Server ] <----(Collects CPU & Memory metrics every 15s from Kubelet cAdvisor)
         ^
         |
 [ HPA Controller ] (Reconciliation Loop: every 15s)
   └── Evaluates: Target CPU = 70% vs Current CPU = 90%
   └── Calculates: Desired Replicas = ceil[Current Replicas * (Current Metric / Target Metric)]
         |
         v (Scales Deployment)
 [ Deployment: axion-ui ] (Replicas scale from 2 -> 5 pods!)
```

---

## 📜 2. HPA Manifest Example

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: axion-ui-hpa
  namespace: default
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: axion-ui
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

* **Requirement:** The target Deployment's containers **MUST** define `resources.requests.cpu`. Without requests, the HPA cannot compute percentage utilization and fails with `unknown` metric!

---

## 💡 Key Takeaways
1. HPA requires `metrics-server` to be installed and running in the cluster.
2. Always declare CPU and memory requests on all containers targeted by HPA.
