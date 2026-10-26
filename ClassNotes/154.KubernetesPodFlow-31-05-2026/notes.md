# Class Notes: The Complete Pod Creation Lifecycle Flow 🔄

**Date:** 31-05-2026  
**Topic:** Step-by-Step Flow from `kubectl apply` to Running Pod on Worker Node

---

## 📌 1. The 8-Step Pod Creation Sequence

What happens when an engineer runs `kubectl apply -f pod.yaml`?

```
 1. [ User (kubectl) ] ----(POST /api/v1/namespaces/default/pods)----> 2. [ API Server ]
                                                                             |
                                                                             v 3. Writes Pending Pod
                                                                        [ etcd Store ]
                                                                             |
 4. [ Scheduler ] <----(Watches for unassigned Pods via API Server)----------+
         |
         +----(Assigns Pod to Node 2 based on resources)-------------> [ API Server ]
                                                                             |
                                                                             v
 5. [ Node 2 Kubelet ] <----(Watches for assigned Pods)----------------------+
         |
         +----(Instructs Runtime: CRI)--------------------------------> 6. [ containerd ]
                                                                             |
                                                                             v 7. Pulls image
                                                                        [ Container Registry ]
                                                                             |
                                                                             v 8. Launches container
                                                                        [ RUNNING POD ]
```

---

## 📋 2. Step-by-Step Sequence Breakdown

1. **Authentication & Validation:** `kubectl` sends signed HTTPS request to API Server. API Server authenticates token and validates manifest schema.
2. **Persistence:** API Server writes Pod object to `etcd` in state `Pending` (no node assigned).
3. **Scheduling:** `kube-scheduler` detects the unscheduled pod. Evaluates filtering (node resource capacity) and scoring (affinities, taints) and binds Pod to selected node.
4. **Execution:** The target node's `kubelet` detects the pod binding.
5. **Runtime Spawning:** `kubelet` calls `containerd` via CRI (Container Runtime Interface).
6. **Network Namespace Creation:** Container runtime sets up network interface and assigns Pod IP via CNI plugin.
7. **Container Launch:** Container runtime pulls image and spawns the application process.
8. **Status Feedback:** `kubelet` monitors status and updates API Server -> saved to `etcd`. Pod enters state `Running`.

---

## 💡 Key Takeaways
1. Every state transition is recorded in `etcd` exclusively through the API Server.
2. The Scheduler only assigns the node name; the target node's Kubelet actually creates the containers.
