# 🔰 Basic Kubectl Commands
## Essential Commands for Kubernetes Beginners

> **50 essential commands for daily Kubernetes operations**

---

## 📋 Table of Contents
- [Cluster Information](#cluster-information)
- [Basic Resource Operations](#basic-resource-operations)
- [Pod Management](#pod-management)
- [Service Management](#service-management)
- [Namespace Operations](#namespace-operations)
- [Basic Troubleshooting](#basic-troubleshooting)
- [Logs and Debugging](#logs-and-debugging)

---

## 🌐 Cluster Information

### 1. Get Cluster Information
```bash
kubectl cluster-info
# Use: Check cluster status and API server endpoints
# Example: Shows cluster URL and services
# Notes: First command to run when troubleshooting connectivity
```

### 2. Get Detailed Cluster Information
```bash
kubectl cluster-info dump
# Use: Get detailed cluster information for debugging
# Example: Dumps all cluster state to files
# Notes: Creates large output, use for debugging only
```

### 3. Get API Resources
```bash
kubectl api-resources
# Use: List all available API resources in the cluster
# Example: Shows pods, services, deployments, etc.
# Notes: Essential for understanding what resources exist
```

### 4. Get API Versions
```bash
kubectl api-versions
# Use: List all supported API versions
# Example: Shows v1, apps/v1, networking.k8s.io/v1, etc.
# Notes: Important for API compatibility
```

### 5. Get Component Status
```bash
kubectl get componentstatuses
# Use: Check health of cluster components
# Example: Shows scheduler, controller-manager, etcd status
# Notes: Deprecated in newer versions, use kubectl get --raw instead
```

---

## 📦 Basic Resource Operations

### 6. Get All Pods
```bash
kubectl get pods
# Use: List all pods in current namespace
# Example: Shows pod names, status, restart count, age
# Notes: Most commonly used command
```

### 7. Get All Pods in All Namespaces
```bash
kubectl get pods --all-namespaces
# Use: List pods across all namespaces
# Example: Shows namespace, pod name, status, etc.
# Notes: Useful for cluster-wide pod monitoring
```

### 8. Get Specific Resource
```bash
kubectl get <resource> <name>
# Use: Get specific resource by name
# Example: kubectl get pod my-pod
# Notes: Replace <resource> with pods, services, deployments, etc.
```

### 9. Get with Wide Output
```bash
kubectl get pods -o wide
# Use: Get pods with additional details like IP and node
# Example: Shows pod IP, node name, and other details
# Notes: Useful for networking troubleshooting
```

### 10. Get with Custom Columns
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP
# Use: Customize output format for specific information
# Example: Shows only name, status, and IP columns
# Notes: Great for scripting and automation
```

### 11. Get Resources in YAML Format
```bash
kubectl get pods -o yaml
# Use: Get YAML format output for editing
# Example: Full YAML representation of pods
# Notes: Useful for understanding resource structure
```

### 12. Get Resources in JSON Format
```bash
kubectl get pods -o json
# Use: Get JSON format for programmatic processing
# Example: Full JSON representation of pods
# Notes: Useful for automation and scripting
```

### 13. Get Resources with Labels
```bash
kubectl get pods -l app=nginx
# Use: Filter pods by specific labels
# Example: Shows only pods with app=nginx label
# Notes: Essential for working with labeled resources
```

### 14. Get Resources by Field Selector
```bash
kubectl get pods --field-selector status.phase=Running
# Use: Filter by specific field values
# Example: Shows only running pods
# Notes: Useful for filtering by status
```

### 15. Get Resources with Sorting
```bash
kubectl get pods --sort-by=.metadata.creationTimestamp
# Use: Sort resources by specific field
# Example: Sorts pods by creation time (oldest first)
# Notes: Useful for finding oldest/newest resources
```

---

## 🐳 Pod Management

### 16. Describe a Pod
```bash
kubectl describe pod <pod-name>
# Use: Get detailed information about a specific pod
# Example: Shows events, conditions, volumes, etc.
# Notes: Essential for troubleshooting pod issues
```

### 17. Get Pod Logs
```bash
kubectl logs <pod-name>
# Use: View container logs
# Example: Shows application logs
# Notes: Most common debugging command
```

### 18. Follow Pod Logs
```bash
kubectl logs -f <pod-name>
# Use: Stream logs in real-time
# Example: Continuously shows new log entries
# Notes: Useful for monitoring application behavior
```

### 19. Get Logs from Specific Container
```bash
kubectl logs <pod-name> -c <container-name>
# Use: Get logs from multi-container pods
# Example: kubectl logs my-pod -c sidecar
# Notes: Required when pod has multiple containers
```

### 20. Get Logs with Timestamps
```bash
kubectl logs <pod-name> --timestamps
# Use: Get logs with timestamp information
# Example: Shows when each log entry was created
# Notes: Useful for debugging timing issues
```

### 21. Get Previous Container Logs
```bash
kubectl logs <pod-name> --previous
# Use: Get logs from previous container instance
# Example: Useful when container has restarted
# Notes: Only works if container has restarted
```

### 22. Execute Command in Pod
```bash
kubectl exec -it <pod-name> -- /bin/bash
# Use: Interactive shell access to container
# Example: Opens bash shell in container
# Notes: Most common way to debug containers
```

### 23. Execute Command Without Interactive Mode
```bash
kubectl exec <pod-name> -- ls /app
# Use: Run one-off commands in container
# Example: Lists files in /app directory
# Notes: Useful for quick checks
```

### 24. Copy Files from Pod
```bash
kubectl cp <pod-name>:/path/to/file /local/path
# Use: Copy files from pod to local system
# Example: kubectl cp my-pod:/app/logs.txt ./logs.txt
# Notes: Useful for extracting logs or configs
```

### 25. Copy Files to Pod
```bash
kubectl cp /local/path <pod-name>:/path/to/file
# Use: Copy files from local system to pod
# Example: kubectl cp ./config.yaml my-pod:/app/config.yaml
# Notes: Useful for updating configurations
```

---

## 🔗 Service Management

### 26. Get All Services
```bash
kubectl get services
# Use: List all services in current namespace
# Example: Shows service names, types, cluster IPs, ports
# Notes: Essential for understanding networking
```

### 27. Describe a Service
```bash
kubectl describe service <service-name>
# Use: Get detailed service information including endpoints
# Example: Shows service details, endpoints, events
# Notes: Useful for troubleshooting service connectivity
```

### 28. Get Service Endpoints
```bash
kubectl get endpoints <service-name>
# Use: Get endpoints for a service
# Example: Shows which pods the service routes to
# Notes: Essential for debugging service connectivity
```

### 29. Port Forward to Service
```bash
kubectl port-forward service/<service-name> 8080:80
# Use: Forward local port to service port
# Example: Access service on localhost:8080
# Notes: Useful for local testing and debugging
```

### 30. Port Forward to Pod
```bash
kubectl port-forward <pod-name> 8080:80
# Use: Forward local port to pod port
# Example: Access pod directly on localhost:8080
# Notes: Useful for direct pod access
```

---

## 📁 Namespace Operations

### 31. Get All Namespaces
```bash
kubectl get namespaces
# Use: List all namespaces in the cluster
# Example: Shows namespace names and status
# Notes: Essential for multi-tenant environments
```

### 32. Create Namespace
```bash
kubectl create namespace <namespace-name>
# Use: Create a new namespace
# Example: kubectl create namespace my-app
# Notes: Useful for organizing resources
```

### 33. Delete Namespace
```bash
kubectl delete namespace <namespace-name>
# Use: Delete a namespace and all its resources
# Example: kubectl delete namespace my-app
# Notes: Destructive operation - deletes everything in namespace
```

### 34. Set Default Namespace
```bash
kubectl config set-context --current --namespace=<namespace>
# Use: Set default namespace for current context
# Example: kubectl config set-context --current --namespace=default
# Notes: Affects all subsequent commands
```

### 35. Get Resources in Specific Namespace
```bash
kubectl get pods -n <namespace>
# Use: Get resources in specific namespace
# Example: kubectl get pods -n kube-system
# Notes: -n flag specifies namespace
```

---

## 🔧 Basic Troubleshooting

### 36. Get Events
```bash
kubectl get events
# Use: Get cluster events for troubleshooting
# Example: Shows recent events in chronological order
# Notes: Essential for understanding what's happening
```

### 37. Get Events Sorted by Time
```bash
kubectl get events --sort-by='.lastTimestamp'
# Use: Get events sorted by timestamp
# Example: Shows events from oldest to newest
# Notes: Useful for chronological troubleshooting
```

### 38. Get Events for Specific Resource
```bash
kubectl get events --field-selector involvedObject.name=<pod-name>
# Use: Get events related to specific resource
# Example: kubectl get events --field-selector involvedObject.name=my-pod
# Notes: Useful for debugging specific resources
```

### 39. Check Pod Status
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,READY:.status.containerStatuses[0].ready
# Use: Check pod status and readiness
# Example: Shows pod name, phase, and ready status
# Notes: Useful for health monitoring
```

### 40. Get Pod Details
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,RESTARTS:.status.containerStatuses[0].restartCount,AGE:.metadata.creationTimestamp
# Use: Get comprehensive pod information
# Example: Shows name, status, restart count, and age
# Notes: Useful for identifying problematic pods
```

---

## 📝 Logs and Debugging

### 41. Get Logs with Tail
```bash
kubectl logs <pod-name> --tail=100
# Use: Get last N lines of logs
# Example: Shows last 100 log lines
# Notes: Useful for recent log analysis
```

### 42. Get Logs Since Time
```bash
kubectl logs <pod-name> --since=1h
# Use: Get logs since specific time
# Example: Shows logs from last hour
# Notes: Useful for time-based debugging
```

### 43. Get Logs with Labels
```bash
kubectl logs -l app=myapp
# Use: Get logs from all pods with specific label
# Example: Shows logs from all pods with app=myapp label
# Notes: Useful for multi-pod applications
```

### 44. Execute Command in All Pods with Label
```bash
kubectl exec -l app=myapp -- ls /app
# Use: Run command in all pods with specific label
# Example: Lists files in /app for all matching pods
# Notes: Useful for bulk operations
```

### 45. Get Resource Usage
```bash
kubectl top pods
# Use: Get pod CPU and memory usage
# Example: Shows resource consumption per pod
# Notes: Requires metrics-server to be installed
```

### 46. Get Node Resource Usage
```bash
kubectl top nodes
# Use: Get node CPU and memory usage
# Example: Shows resource consumption per node
# Notes: Requires metrics-server to be installed
```

### 47. Check API Server Health
```bash
kubectl get --raw /healthz
# Use: Check API server health
# Example: Returns "ok" if API server is healthy
# Notes: Useful for connectivity testing
```

### 48. Check Authentication
```bash
kubectl auth can-i get pods
# Use: Check if current user can perform action
# Example: Returns "yes" or "no"
# Notes: Useful for permission debugging
```

### 49. Get Current Context
```bash
kubectl config current-context
# Use: Check which cluster/context is active
# Example: Shows current context name
# Notes: Essential for multi-cluster environments
```

### 50. List All Contexts
```bash
kubectl config get-contexts
# Use: List all available contexts
# Example: Shows all configured clusters/contexts
# Notes: Useful for context management
```

---

## 🎯 Quick Reference

### Most Used Commands
```bash
# Daily operations
kubectl get pods                    # List pods
kubectl describe pod <name>         # Get pod details
kubectl logs <pod-name>             # Get logs
kubectl exec -it <pod-name> -- bash # Enter pod
kubectl get services                # List services
kubectl get events                  # Get events
```

### Troubleshooting Flow
```bash
# 1. Check cluster connectivity
kubectl cluster-info

# 2. Check pod status
kubectl get pods

# 3. Get pod details
kubectl describe pod <pod-name>

# 4. Check logs
kubectl logs <pod-name>

# 5. Check events
kubectl get events --sort-by='.lastTimestamp'
```

---

## 📚 Next Steps

After mastering these basic commands, proceed to:
- [Intermediate Commands](02-intermediate-commands.md) - Advanced operations
- [Advanced Commands](03-advanced-commands.md) - Expert-level operations
- [Expert Commands](04-expert-commands.md) - Master-level administration
- [One-Liners & Scripts](05-one-liners-scripts.md) - Ready-to-use commands

---

*This file contains 50 essential kubectl commands for Kubernetes beginners.* 