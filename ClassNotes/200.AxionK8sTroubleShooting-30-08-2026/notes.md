# Class Notes: Kubernetes Troubleshooting: CrashLoopBackOff & Error Mastery 🩺

**Date:** 30-08-2026  
**Topic:** Kubernetes Debugging Workflow, Pod States, Exit Codes, and Diagnostic Commands

---

## 📌 1. The 5-Step Diagnostic Procedure

When a pod fails or shows unhealthy status:

```
 Step 1: kubectl get pods -n <namespace> -o wide       (Check STATUS, RESTARTS, and NODE)
                  |
 Step 2: kubectl describe pod <pod_name>               (Inspect EVENTS at the bottom!)
                  |
 Step 3: kubectl logs <pod_name> [--previous]          (Read application stdout/stderr logs)
                  |
 Step 4: kubectl get events --sort-by=.metadata.creationTimestamp
                  |
 Step 5: kubectl exec -it <pod_name> -- sh             (Verify files, DNS, and database connectivity)
```

---

## 🚨 2. Common Failure States & Solutions

* **`CrashLoopBackOff`:** The container process crashed immediately after starting.
  * *Cause:* Missing environment variable, database connection failed, or typo in startup command.
  * *Fix:* Check `kubectl logs <pod> --previous` to see the unhandled application crash exception.
* **`ImagePullBackOff` / `ErrImagePull`:** Kubernetes cannot download the container image.
  * *Cause:* Typo in image name/tag, private registry authentication failed, or image does not exist.
  * *Fix:* Check ACR permissions (`AcrPull` role) or docker-registry secret.
* **`OOMKilled` (Exit Code 137):** Container exceeded its memory limit (`resources.limits.memory`).
  * *Fix:* Increase container memory limit or resolve memory leaks in application code.
* **`Pending`:** Scheduler cannot find a node that satisfies the pod's requirements.
  * *Cause:* Insufficient CPU/memory across nodes, node affinity mismatch, or missing PVC binding.

---

## 💡 Key Takeaways
1. 90% of Kubernetes pod failures can be diagnosed by checking the **Events** section in `kubectl describe pod`.
2. Understand container exit codes: Exit Code 0 (Success), 1 (Application Error), 137 (OOMKilled / SIGKILL).
