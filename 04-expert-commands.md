# 🎯 Expert Kubectl Commands
## Master-Level Cluster Administration

> **60 expert commands for direct etcd access, scheduler debugging, and cluster diagnostics**

---

## 📋 Table of Contents
- [Direct etcd Access](#direct-etcd-access)
- [Scheduler Debugging](#scheduler-debugging)
- [Cluster Diagnostics](#cluster-diagnostics)
- [Advanced Security](#advanced-security)
- [Performance Tuning](#performance-tuning)
- [Advanced Troubleshooting](#advanced-troubleshooting)
- [Custom Operations](#custom-operations)
- [Automation Scripts](#automation-scripts)

---

## 🗄️ Direct etcd Access

### 1. Get etcd Endpoints
```bash
kubectl get endpoints -n kube-system etcd
# Use: Find etcd service endpoints
# Example: Shows etcd service endpoints
# Notes: Required for direct etcd access
```

### 2. Direct etcd Query
```bash
etcdctl get /registry/pods/default/<pod-name>
# Use: Direct etcd data access
# Example: etcdctl get /registry/pods/default/my-pod
# Notes: Requires etcdctl and proper authentication
```

### 3. List etcd Keys
```bash
etcdctl get /registry/pods/default/ --prefix
# Use: List all pod keys in etcd
# Example: etcdctl get /registry/pods/default/ --prefix
# Notes: Shows all pod data in etcd
```

### 4. Watch etcd Changes
```bash
etcdctl watch /registry/pods/default/ --prefix
# Use: Monitor etcd changes in real-time
# Example: etcdctl watch /registry/pods/default/ --prefix
# Notes: Real-time etcd monitoring
```

### 5. Backup etcd
```bash
etcdctl snapshot save backup.db
# Use: Create etcd backup
# Example: etcdctl snapshot save etcd-backup-$(date +%Y%m%d).db
# Notes: Essential for disaster recovery
```

### 6. Restore etcd from Backup
```bash
etcdctl snapshot restore backup.db --data-dir=/var/lib/etcd-restored
# Use: Restore etcd from backup
# Example: etcdctl snapshot restore backup.db --data-dir=/var/lib/etcd-restored
# Notes: Destructive operation - stops etcd
```

### 7. Get etcd Member List
```bash
etcdctl member list
# Use: List etcd cluster members
# Example: Shows all etcd members and their status
# Notes: Useful for etcd cluster management
```

### 8. Get etcd Cluster Health
```bash
etcdctl endpoint health
# Use: Check etcd cluster health
# Example: Shows health status of all endpoints
# Notes: Essential for etcd monitoring
```

### 9. Get etcd Cluster Status
```bash
etcdctl endpoint status
# Use: Get detailed etcd cluster status
# Example: Shows leader, version, and other details
# Notes: Detailed etcd cluster information
```

### 10. Delete etcd Key
```bash
etcdctl del /registry/pods/default/<pod-name>
# Use: Delete specific etcd key
# Example: etcdctl del /registry/pods/default/my-pod
# Notes: Dangerous - directly modifies etcd
```

---

## ⚙️ Scheduler Debugging

### 11. Get Scheduler Logs
```bash
kubectl logs -n kube-system kube-scheduler-<node-name>
# Use: Get scheduler logs
# Example: kubectl logs -n kube-system kube-scheduler-master-1
# Notes: Essential for scheduler debugging
```

### 12. Get Scheduler Configuration
```bash
kubectl get configmap -n kube-system kube-scheduler -o yaml
# Use: Get scheduler configuration
# Example: Shows scheduler configuration
# Notes: Useful for scheduler customization
```

### 13. Get Scheduler Metrics
```bash
kubectl get --raw /metrics | grep scheduler
# Use: Get scheduler metrics
# Example: Shows scheduler performance metrics
# Notes: Useful for scheduler monitoring
```

### 14. Debug Pod Scheduling
```bash
kubectl describe pod <pod-name> | grep -A 10 -B 10 "Events:"
# Use: Check pod scheduling events
# Example: kubectl describe pod my-pod | grep -A 10 -B 10 "Events:"
# Notes: Shows scheduling decisions and failures
```

### 15. Get Unschedulable Pods
```bash
kubectl get pods --field-selector status.phase=Pending
# Use: Find pods that can't be scheduled
# Example: Shows all pending pods
# Notes: Useful for scheduling issue identification
```

### 16. Get Pod Scheduling Details
```bash
kubectl get pod <pod-name> -o jsonpath='{.status.conditions[?(@.type=="PodScheduled")]}'
# Use: Get pod scheduling condition
# Example: kubectl get pod my-pod -o jsonpath='{.status.conditions[?(@.type=="PodScheduled")]}'
# Notes: Shows scheduling status details
```

### 17. Get Node Scheduling Status
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,SCHEDULABLE:.spec.unschedulable,TAINTS:.spec.taints[*].key
# Use: Check node scheduling status
# Example: Shows node schedulability and taints
# Notes: Useful for node scheduling analysis
```

### 18. Get Scheduler Policy
```bash
kubectl get configmap -n kube-system scheduler-policy -o yaml
# Use: Get custom scheduler policy
# Example: Shows custom scheduling policies
# Notes: Only if custom scheduler policy is configured
```

### 19. Get Scheduler Pods
```bash
kubectl get pods -n kube-system -l component=kube-scheduler
# Use: Get scheduler pod details
# Example: Shows all scheduler pods
# Notes: Useful for scheduler troubleshooting
```

### 20. Get Scheduler Events
```bash
kubectl get events -n kube-system --field-selector involvedObject.kind=Pod,reason=FailedScheduling
# Use: Get scheduler failure events
# Example: Shows scheduling failure events
# Notes: Useful for scheduling issue analysis
```

---

## 🔍 Cluster Diagnostics

### 21. Get Node Conditions
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status
# Use: Check node readiness status
# Example: Shows node readiness status
# Notes: Essential for node health monitoring
```

### 22. Get Node Capacity and Allocatable
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,CAPACITY-CPU:.status.capacity.cpu,CAPACITY-MEMORY:.status.capacity.memory,ALLOCATABLE-CPU:.status.allocatable.cpu,ALLOCATABLE-MEMORY:.status.allocatable.memory
# Use: Get node resource capacity
# Example: Shows node resource capacity and allocatable
# Notes: Useful for capacity planning
```

### 23. Get Node Taints
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints[*].key
# Use: Get node taints
# Example: Shows node taints for scheduling
# Notes: Useful for node scheduling analysis
```

### 24. Get Node Labels
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,LABELS:.metadata.labels
# Use: Get node labels
# Example: Shows all node labels
# Notes: Useful for node organization
```

### 25. Get Node System Info
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,KERNEL:.status.nodeInfo.kernelVersion,OS:.status.nodeInfo.operatingSystem,ARCH:.status.nodeInfo.architecture
# Use: Get node system information
# Example: Shows node system details
# Notes: Useful for system compatibility
```

### 26. Get Cluster Events by Component
```bash
kubectl get events --all-namespaces --field-selector involvedObject.kind=Node
# Use: Get node-related events
# Example: Shows all node events across namespaces
# Notes: Useful for cluster-wide node monitoring
```

### 27. Get Component Status
```bash
kubectl get componentstatuses
# Use: Check health of cluster components
# Example: Shows scheduler, controller-manager, etcd status
# Notes: Deprecated but still useful
```

### 28. Get API Server Version
```bash
kubectl version --short
# Use: Get API server version
# Example: Shows client and server versions
# Notes: Useful for version compatibility
```

### 29. Get Cluster Info Dump
```bash
kubectl cluster-info dump --output-directory=/tmp/cluster-dump
# Use: Get comprehensive cluster information
# Example: kubectl cluster-info dump --output-directory=/tmp/cluster-dump
# Notes: Creates large output files
```

### 30. Get Cluster Events Summary
```bash
kubectl get events --all-namespaces --sort-by='.lastTimestamp' | tail -50
# Use: Get recent cluster events
# Example: Shows last 50 events across all namespaces
# Notes: Useful for cluster health overview
```

---

## 🔒 Advanced Security

### 31. Get All Service Accounts
```bash
kubectl get serviceaccounts --all-namespaces
# Use: List all service accounts across namespaces
# Example: Shows all service accounts in cluster
# Notes: Essential for security audit
```

### 32. Get All Roles
```bash
kubectl get roles --all-namespaces
# Use: List all roles across namespaces
# Example: Shows all namespace-scoped roles
# Notes: Useful for RBAC analysis
```

### 33. Get All Cluster Roles
```bash
kubectl get clusterroles
# Use: List all cluster roles
# Example: Shows all cluster-scoped roles
# Notes: Essential for cluster-wide permissions
```

### 34. Get All Role Bindings
```bash
kubectl get rolebindings --all-namespaces
# Use: List all role bindings across namespaces
# Example: Shows all namespace-scoped role bindings
# Notes: Useful for permission mapping
```

### 35. Get All Cluster Role Bindings
```bash
kubectl get clusterrolebindings
# Use: List all cluster role bindings
# Example: Shows all cluster-scoped role bindings
# Notes: Essential for cluster-wide permissions
```

### 36. Check All Permissions
```bash
kubectl auth can-i --list
# Use: List all permissions for current user
# Example: Shows all actions current user can perform
# Notes: Useful for permission audit
```

### 37. Check Permissions for Specific Resource
```bash
kubectl auth can-i get pods --all-namespaces
# Use: Check permissions across all namespaces
# Example: kubectl auth can-i create deployments --all-namespaces
# Notes: Useful for cross-namespace permission checking
```

### 38. Get Secret Details
```bash
kubectl get secrets --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,TYPE:.type,AGE:.metadata.creationTimestamp
# Use: Get all secrets across namespaces
# Example: Shows all secrets in cluster
# Notes: Essential for security audit
```

### 39. Get Pod Security Context
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,SECURITY-CONTEXT:.spec.securityContext.runAsUser
# Use: Get pod security context
# Example: Shows pod security settings
# Notes: Useful for security compliance
```

### 40. Get Network Policies
```bash
kubectl get networkpolicies --all-namespaces
# Use: List all network policies
# Example: Shows all network policies in cluster
# Notes: Essential for network security audit
```

---

## ⚡ Performance Tuning

### 41. Get Resource Usage by Namespace
```bash
kubectl top pods --all-namespaces --sort-by=cpu
# Use: Get resource usage across all namespaces
# Example: Shows CPU usage across all namespaces
# Notes: Useful for resource optimization
```

### 42. Get Node Resource Distribution
```bash
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,NODE:.spec.nodeName | sort | uniq -c
# Use: Get pod distribution across nodes
# Example: Shows how many pods are on each node
# Notes: Useful for load balancing analysis
```

### 43. Get Resource Quota Usage by Namespace
```bash
kubectl get resourcequota --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,USED:.status.used.cpu,LIMIT:.spec.hard.cpu
# Use: Get resource quota usage across namespaces
# Example: Shows quota usage across all namespaces
# Notes: Useful for resource planning
```

### 44. Get Persistent Volume Usage
```bash
kubectl get pv -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.capacity.storage,STORAGE-CLASS:.spec.storageClassName
# Use: Get persistent volume usage
# Example: Shows all persistent volumes and their status
# Notes: Useful for storage planning
```

### 45. Get Persistent Volume Claims
```bash
kubectl get pvc --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.resources.requests.storage
# Use: Get persistent volume claims across namespaces
# Example: Shows all PVCs and their status
# Notes: Useful for storage analysis
```

### 46. Get Storage Class Information
```bash
kubectl get storageclass -o custom-columns=NAME:.metadata.name,PROVISIONER:.provisioner,RECLAIM-POLICY:.reclaimPolicy
# Use: Get storage class details
# Example: Shows all storage classes and their configuration
# Notes: Useful for storage configuration
```

### 47. Get Node Resource Pressure
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,PRESSURE:.status.conditions[?(@.type=="MemoryPressure")].status
# Use: Check node resource pressure
# Example: Shows memory pressure status for all nodes
# Notes: Useful for node health monitoring
```

### 48. Get Pod Resource Requests vs Limits
```bash
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,CPU-REQUEST:.spec.containers[0].resources.requests.cpu,CPU-LIMIT:.spec.containers[0].resources.limits.cpu,MEMORY-REQUEST:.spec.containers[0].resources.requests.memory,MEMORY-LIMIT:.spec.containers[0].resources.limits.memory
# Use: Compare resource requests vs limits
# Example: Shows resource configuration for all pods
# Notes: Useful for resource optimization
```

### 49. Get Node Affinity and Anti-Affinity
```bash
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,NODE-SELECTOR:.spec.nodeSelector
# Use: Get pod node affinity settings
# Example: Shows node selectors for all pods
# Notes: Useful for scheduling analysis
```

### 50. Get Pod Disruption Budgets
```bash
kubectl get pdb --all-namespaces
# Use: List all pod disruption budgets
# Example: Shows all PDBs across namespaces
# Notes: Useful for availability planning
```

---

## 🐛 Advanced Troubleshooting

### 51. Get All Events with Custom Format
```bash
kubectl get events --all-namespaces --sort-by='.lastTimestamp' -o custom-columns=TIME:.lastTimestamp,NAMESPACE:.metadata.namespace,NAME:.involvedObject.name,KIND:.involvedObject.kind,REASON:.reason,MESSAGE:.message
# Use: Get all events with custom format
# Example: Shows all events across namespaces in custom format
# Notes: Useful for cluster-wide event analysis
```

### 52. Get Events by Time Range
```bash
kubectl get events --all-namespaces --field-selector lastTimestamp>=2023-12-01T00:00:00Z
# Use: Get events within time range
# Example: kubectl get events --all-namespaces --field-selector lastTimestamp>=2023-12-01T00:00:00Z
# Notes: Useful for time-based troubleshooting
```

### 53. Get Events by Type
```bash
kubectl get events --all-namespaces --field-selector type=Warning
# Use: Get only warning events
# Example: Shows all warning events across namespaces
# Notes: Useful for problem identification
```

### 54. Get Events by Reason
```bash
kubectl get events --all-namespaces --field-selector reason=FailedScheduling
# Use: Get events by specific reason
# Example: kubectl get events --all-namespaces --field-selector reason=BackOff
# Notes: Useful for specific issue debugging
```

### 55. Get Pod Status with Conditions
```bash
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,READY:.status.containerStatuses[0].ready,RESTARTS:.status.containerStatuses[0].restartCount
# Use: Get comprehensive pod status
# Example: Shows pod status across all namespaces
# Notes: Useful for pod health monitoring
```

### 56. Get Service Endpoints
```bash
kubectl get endpoints --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,ENDPOINTS:.subsets[0].addresses[*].ip
# Use: Get service endpoints across namespaces
# Example: Shows all service endpoints
# Notes: Useful for service connectivity troubleshooting
```

### 57. Get ConfigMap Usage
```bash
kubectl get configmaps --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,AGE:.metadata.creationTimestamp
# Use: Get all config maps across namespaces
# Example: Shows all config maps in cluster
# Notes: Useful for configuration audit
```

### 58. Get Secret Usage
```bash
kubectl get secrets --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,TYPE:.type,AGE:.metadata.creationTimestamp
# Use: Get all secrets across namespaces
# Example: Shows all secrets in cluster
# Notes: Essential for security audit
```

### 59. Get Ingress Status
```bash
kubectl get ingress --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,ADDRESS:.status.loadBalancer.ingress[0].ip
# Use: Get ingress status across namespaces
# Example: Shows all ingress resources and their status
# Notes: Useful for ingress troubleshooting
```

### 60. Get Network Policy Status
```bash
kubectl get networkpolicies --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,POD-SELECTOR:.spec.podSelector.matchLabels
# Use: Get network policy status across namespaces
# Example: Shows all network policies and their selectors
# Notes: Useful for network security analysis
```

---

## 🎯 Quick Reference

### Most Used Expert Commands
```bash
# etcd operations
etcdctl get /registry/pods/default/ --prefix
etcdctl snapshot save backup.db
etcdctl endpoint health

# Scheduler debugging
kubectl logs -n kube-system kube-scheduler-<node-name>
kubectl get pods --field-selector status.phase=Pending
kubectl describe pod <pod-name> | grep -A 10 -B 10 "Events:"

# Cluster diagnostics
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status
kubectl get events --all-namespaces --sort-by='.lastTimestamp' | tail -50

# Security audit
kubectl auth can-i --list
kubectl get clusterrolebindings
kubectl get secrets --all-namespaces

# Performance analysis
kubectl top pods --all-namespaces --sort-by=cpu
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,NODE:.spec.nodeName | sort | uniq -c
```

### Common Patterns
```bash
# etcd backup and restore
etcdctl snapshot save etcd-backup-$(date +%Y%m%d).db
etcdctl snapshot restore backup.db --data-dir=/var/lib/etcd-restored

# Scheduler analysis
kubectl get events -n kube-system --field-selector involvedObject.kind=Pod,reason=FailedScheduling
kubectl get nodes -o custom-columns=NAME:.metadata.name,SCHEDULABLE:.spec.unschedulable,TAINTS:.spec.taints[*].key

# Security audit
kubectl get serviceaccounts --all-namespaces
kubectl get roles --all-namespaces
kubectl get clusterroles

# Performance monitoring
kubectl top pods --all-namespaces --sort-by=cpu | head -10
kubectl get resourcequota --all-namespaces
```

---

## 📚 Next Steps

After mastering these expert commands, proceed to:
- [One-Liners & Scripts](05-one-liners-scripts.md) - Ready-to-use commands

---

*This file contains 60 expert kubectl commands for Kubernetes masters.* 