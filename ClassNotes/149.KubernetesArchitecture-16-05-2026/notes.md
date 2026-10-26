# Class Notes: Kubernetes Architecture: Control Plane & Worker Nodes ☸️

**Date:** 16-05-2026  
**Topic:** Kubernetes Master Node Components, Worker Node Daemons, and Cluster Orchestration

---

## 📌 1. Architecture: Kubernetes Master vs. Worker Nodes

Kubernetes is a distributed container orchestration system separating control plane logic from worker compute:

```
 +-------------------------------------------------------------------------------+
 | CONTROL PLANE (MASTER NODE)                                                   |
 |                                                                               |
 |  [ API Server (kube-apiserver) ] <==== The central gateway / Brain            |
 |         |                 |                                                   |
 |         v                 v                                                   |
 |  [ etcd Store ]    [ Scheduler (kube-scheduler) ]   [ Controller Manager ]   |
 |  (Cluster state)   (Selects node for pods)          (Maintains desired state) |
 +-------------------------------------------------------------------------------+
                                         |
                       Communicates via  | HTTPS
                                         v
 +-------------------------------------------------------------------------------+
 | WORKER NODES (Compute Capacity)                                               |
 |                                                                               |
 |   Worker Node 1                                Worker Node 2                  |
 |  +---------------------------------+          +-----------------------------+ |
 |  | Kubelet (Node agent)            |          | Kubelet                     | |
 |  | Kube-Proxy (Networking rules)   |          | Kube-Proxy                  | |
 |  | Container Runtime (containerd)  |          | Container Runtime           | |
 |  |                                 |          |                             | |
 |  |  [ Pod 1 ]       [ Pod 2 ]      |          |  [ Pod 3 ]     [ Pod 4 ]    | |
 |  +---------------------------------+          +-----------------------------+ |
 +-------------------------------------------------------------------------------+
```

---

## 🧩 2. Core Control Plane Components

* **API Server (`kube-apiserver`):** The front door of the control plane; exposes the Kubernetes REST API. Every component communicates through the API server; components never communicate with each other directly.
* **etcd:** Distributed, consistent key-value store holding the entire cluster state, configuration, and metadata.
* **Scheduler (`kube-scheduler`):** Watches for newly created pods without assigned nodes and selects the optimal worker node based on resource requests, taints, and affinities.
* **Controller Manager (`kube-controller-manager`):** Runs reconciliation loops (Node Controller, ReplicaSet Controller, Endpoint Controller) to bring current state to desired state.

---

## 🛠️ 3. Core Worker Node Components

* **Kubelet:** The primary node agent. Receives `PodSpec` assignments from the API server and instructs the local container runtime to launch or stop containers.
* **Kube-Proxy:** Maintains network rules on nodes (using iptables or IPVS) to handle Service IP forwarding and load balancing across pod endpoints.
* **Container Runtime:** Software responsible for running containers (e.g., `containerd`, `CRI-O`).

---

## 💡 Key Takeaways
1. The API Server is the only component that directly communicates with `etcd`.
2. Worker nodes run workloads; master nodes manage scheduling, lifecycle, and desired state.
