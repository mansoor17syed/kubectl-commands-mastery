# 🚀 One-Liners & Scripts
## Ready-to-Use Kubectl Commands and Automation

> **60 one-liners and scripts for quick operations and automation**

---

## 📋 Table of Contents
- [Quick Diagnostics](#quick-diagnostics)
- [Quick Actions](#quick-actions)
- [Advanced One-Liners](#advanced-one-liners)
- [Automation Scripts](#automation-scripts)
- [Monitoring Scripts](#monitoring-scripts)
- [Troubleshooting Scripts](#troubleshooting-scripts)
- [Utility Functions](#utility-functions)

---

## 🔍 Quick Diagnostics

### 1. Get All Pods with Status and Restart Count
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,RESTARTS:.status.containerStatuses[0].restartCount,AGE:.metadata.creationTimestamp
# Use: Quick pod health overview
# Example: Shows pod name, status, restart count, and age
# Notes: Essential for pod monitoring
```

### 2. Get All Services with Endpoints
```bash
kubectl get services -o custom-columns=NAME:.metadata.name,CLUSTER-IP:.spec.clusterIP,EXTERNAL-IP:.status.loadBalancer.ingress[0].ip,PORT:.spec.ports[0].port
# Use: Quick service overview
# Example: Shows service name, IPs, and ports
# Notes: Useful for service monitoring
```

### 3. Get All Nodes with Conditions
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status,VERSION:.status.nodeInfo.kubeletVersion,AGE:.metadata.creationTimestamp
# Use: Quick node health overview
# Example: Shows node name, readiness, version, and age
# Notes: Essential for node monitoring
```

### 4. Get All Events Sorted by Time
```bash
kubectl get events --sort-by='.lastTimestamp' -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,KIND:.involvedObject.kind,REASON:.reason,MESSAGE:.message
# Use: Quick event overview
# Example: Shows recent events in chronological order
# Notes: Essential for troubleshooting
```

### 5. Get All Pods with Resource Requests and Limits
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,CPU-REQUEST:.spec.containers[0].resources.requests.cpu,CPU-LIMIT:.spec.containers[0].resources.limits.cpu,MEMORY-REQUEST:.spec.containers[0].resources.requests.memory,MEMORY-LIMIT:.spec.containers[0].resources.limits.memory
# Use: Quick resource configuration overview
# Example: Shows resource requests and limits for all pods
# Notes: Useful for resource analysis
```

### 6. Get All Secrets
```bash
kubectl get secrets -o custom-columns=NAME:.metadata.name,TYPE:.type,AGE:.metadata.creationTimestamp
# Use: Quick secret overview
# Example: Shows all secrets with type and age
# Notes: Essential for security audit
```

### 7. Get All ConfigMaps
```bash
kubectl get configmaps -o custom-columns=NAME:.metadata.name,AGE:.metadata.creationTimestamp
# Use: Quick config map overview
# Example: Shows all config maps with age
# Notes: Useful for configuration audit
```

### 8. Get All Persistent Volumes
```bash
kubectl get pv -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.capacity.storage,ACCESS-MODE:.spec.accessModes[0],STORAGE-CLASS:.spec.storageClassName
# Use: Quick persistent volume overview
# Example: Shows all PVs with status and capacity
# Notes: Useful for storage monitoring
```

### 9. Get All Persistent Volume Claims
```bash
kubectl get pvc -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.resources.requests.storage,STORAGE-CLASS:.spec.storageClassName
# Use: Quick PVC overview
# Example: Shows all PVCs with status and capacity
# Notes: Useful for storage monitoring
```

### 10. Get All Deployments with Replicas
```bash
kubectl get deployments -o custom-columns=NAME:.metadata.name,DESIRED:.spec.replicas,CURRENT:.status.replicas,READY:.status.readyReplicas,UP-TO-DATE:.status.updatedReplicas,AVAILABLE:.status.availableReplicas
# Use: Quick deployment overview
# Example: Shows deployment status and replica counts
# Notes: Essential for deployment monitoring
```

---

## ⚡ Quick Actions

### 11. Delete All Pods in Namespace
```bash
kubectl delete pods --all -n <namespace>
# Use: Delete all pods in specific namespace
# Example: kubectl delete pods --all -n default
# Notes: Destructive operation - deletes all pods
```

### 12. Scale All Deployments to 0
```bash
kubectl scale deployment --all --replicas=0
# Use: Scale all deployments to zero replicas
# Example: kubectl scale deployment --all --replicas=0
# Notes: Stops all applications in namespace
```

### 13. Get Logs from All Pods with Label
```bash
kubectl logs -l app=myapp --tail=100
# Use: Get logs from all pods with specific label
# Example: kubectl logs -l app=nginx --tail=100
# Notes: Useful for multi-pod applications
```

### 14. Execute Command in All Pods with Label
```bash
kubectl exec -l app=myapp -- ls /app
# Use: Run command in all pods with specific label
# Example: kubectl exec -l app=myapp -- cat /app/config.json
# Notes: Useful for bulk operations
```

### 15. Port Forward to Service
```bash
kubectl port-forward service/<service-name> 8080:80 &
# Use: Port forward to service in background
# Example: kubectl port-forward service/my-app 8080:80 &
# Notes: Runs in background, use & to detach
```

### 16. Get All Resources in Namespace
```bash
kubectl get all -n <namespace>
# Use: Get all common resources in namespace
# Example: kubectl get all -n production
# Notes: Quick namespace overview
```

### 17. Watch Pods with Custom Output
```bash
kubectl get pods -w -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP,NODE:.spec.nodeName
# Use: Watch pods with custom format
# Example: Real-time pod monitoring with custom columns
# Notes: Useful for deployment monitoring
```

### 18. Get All Secrets Across Namespaces
```bash
kubectl get secrets --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,TYPE:.type,AGE:.metadata.creationTimestamp
# Use: Get all secrets across all namespaces
# Example: Shows all secrets in cluster
# Notes: Essential for security audit
```

### 19. Get All ConfigMaps Across Namespaces
```bash
kubectl get configmaps --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,AGE:.metadata.creationTimestamp
# Use: Get all config maps across all namespaces
# Example: Shows all config maps in cluster
# Notes: Useful for configuration audit
```

### 20. Get All Persistent Volumes
```bash
kubectl get pv -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.capacity.storage,ACCESS-MODE:.spec.accessModes[0],STORAGE-CLASS:.spec.storageClassName
# Use: Get all persistent volumes
# Example: Shows all PVs in cluster
# Notes: Useful for storage monitoring
```

---

## 🔥 Advanced One-Liners

### 21. Get All Pods with Node Affinity
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.nodeSelector}{"\n"}{end}'
# Use: Get pod node affinity settings
# Example: Shows pod names and their node selectors
# Notes: Useful for scheduling analysis
```

### 22. Get All Services with Selectors
```bash
kubectl get services -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.selector}{"\n"}{end}'
# Use: Get service selectors
# Example: Shows service names and their selectors
# Notes: Useful for service connectivity analysis
```

### 23. Get All Deployments with Image
```bash
kubectl get deployments -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.template.spec.containers[0].image}{"\n"}{end}'
# Use: Get deployment images
# Example: Shows deployment names and their container images
# Notes: Useful for image management
```

### 24. Get All Pods with Volumes
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.volumes[*].name}{"\n"}{end}'
# Use: Get pod volumes
# Example: Shows pod names and their volume names
# Notes: Useful for storage analysis
```

### 25. Get All Pods with Security Context
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.securityContext.runAsUser}{"\n"}{end}'
# Use: Get pod security context
# Example: Shows pod names and their security context
# Notes: Useful for security audit
```

### 26. Get All Pods with Resource Usage
```bash
kubectl top pods --no-headers | awk '{print $1, $2, $3}'
# Use: Get pod resource usage
# Example: Shows pod names and their CPU/memory usage
# Notes: Requires metrics-server
```

### 27. Get All Nodes with Taints
```bash
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.taints[*].key}{"\n"}{end}'
# Use: Get node taints
# Example: Shows node names and their taints
# Notes: Useful for scheduling analysis
```

### 28. Get All Pods with Tolerations
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.tolerations[*].key}{"\n"}{end}'
# Use: Get pod tolerations
# Example: Shows pod names and their tolerations
# Notes: Useful for scheduling analysis
```

### 29. Get All Events for Specific Pod
```bash
kubectl get events --field-selector involvedObject.name=<pod-name> --sort-by='.lastTimestamp'
# Use: Get events for specific pod
# Example: kubectl get events --field-selector involvedObject.name=my-pod --sort-by='.lastTimestamp'
# Notes: Useful for pod troubleshooting
```

### 30. Get All Pods with Readiness and Liveness Probes
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[0].readinessProbe.httpGet.path}{"\t"}{.spec.containers[0].livenessProbe.httpGet.path}{"\n"}{end}'
# Use: Get pod health check configuration
# Example: Shows pod names and their probe paths
# Notes: Useful for health check analysis
```

---

## 🤖 Automation Scripts

### 31. Pod Health Check Script
```bash
#!/bin/bash
# Pod Health Check Script
NAMESPACE=${1:-default}
echo "Checking pod health in namespace: $NAMESPACE"
kubectl get pods -n $NAMESPACE -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,READY:.status.containerStatuses[0].ready,RESTARTS:.status.containerStatuses[0].restartCount | grep -v "Running\|True\|0"
# Use: Check for unhealthy pods
# Example: ./pod-health-check.sh production
# Notes: Shows pods that are not running, ready, or have restarts
```

### 32. Resource Usage Monitor
```bash
#!/bin/bash
# Resource Usage Monitor
echo "=== Pod Resource Usage ==="
kubectl top pods --no-headers | sort -k2 -nr | head -10
echo -e "\n=== Node Resource Usage ==="
kubectl top nodes --no-headers | sort -k2 -nr
# Use: Monitor resource usage
# Example: ./resource-monitor.sh
# Notes: Shows top resource consumers
```

### 33. Event Monitor Script
```bash
#!/bin/bash
# Event Monitor Script
echo "Recent events (last 50):"
kubectl get events --sort-by='.lastTimestamp' -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,KIND:.involvedObject.kind,REASON:.reason,MESSAGE:.message | tail -50
# Use: Monitor recent events
# Example: ./event-monitor.sh
# Notes: Shows recent cluster events
```

### 34. Service Connectivity Test
```bash
#!/bin/bash
# Service Connectivity Test
SERVICE_NAME=$1
NAMESPACE=${2:-default}
echo "Testing connectivity to service: $SERVICE_NAME in namespace: $NAMESPACE"
kubectl get endpoints $SERVICE_NAME -n $NAMESPACE
kubectl get service $SERVICE_NAME -n $NAMESPACE -o custom-columns=NAME:.metadata.name,CLUSTER-IP:.spec.clusterIP,EXTERNAL-IP:.status.loadBalancer.ingress[0].ip,PORT:.spec.ports[0].port
# Use: Test service connectivity
# Example: ./service-test.sh my-service production
# Notes: Shows service and endpoint information
```

### 35. Namespace Cleanup Script
```bash
#!/bin/bash
# Namespace Cleanup Script
NAMESPACE=$1
echo "Cleaning up namespace: $NAMESPACE"
kubectl delete all --all -n $NAMESPACE
kubectl delete pvc --all -n $NAMESPACE
kubectl delete secrets --all -n $NAMESPACE
kubectl delete configmaps --all -n $NAMESPACE
# Use: Clean up namespace
# Example: ./namespace-cleanup.sh staging
# Notes: Deletes all resources in namespace
```

### 36. Pod Log Aggregator
```bash
#!/bin/bash
# Pod Log Aggregator
LABEL_SELECTOR=$1
echo "Aggregating logs for pods with label: $LABEL_SELECTOR"
kubectl logs -l $LABEL_SELECTOR --tail=100 --timestamps | while read line; do
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $line"
done
# Use: Aggregate logs from multiple pods
# Example: ./log-aggregator.sh app=myapp
# Notes: Shows logs from all pods with label
```

### 37. Resource Quota Monitor
```bash
#!/bin/bash
# Resource Quota Monitor
echo "=== Resource Quotas ==="
kubectl get resourcequota --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,USED:.status.used.cpu,LIMIT:.spec.hard.cpu
echo -e "\n=== Limit Ranges ==="
kubectl get limitrange --all-namespaces
# Use: Monitor resource quotas
# Example: ./quota-monitor.sh
# Notes: Shows quota usage across namespaces
```

### 38. Node Health Check
```bash
#!/bin/bash
# Node Health Check
echo "=== Node Status ==="
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status,VERSION:.status.nodeInfo.kubeletVersion
echo -e "\n=== Node Resource Usage ==="
kubectl top nodes
# Use: Check node health
# Example: ./node-health.sh
# Notes: Shows node status and resource usage
```

### 39. Secret Audit Script
```bash
#!/bin/bash
# Secret Audit Script
echo "=== Secrets Audit ==="
kubectl get secrets --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,TYPE:.type,AGE:.metadata.creationTimestamp
echo -e "\n=== Service Accounts ==="
kubectl get serviceaccounts --all-namespaces
# Use: Audit secrets and service accounts
# Example: ./secret-audit.sh
# Notes: Shows all secrets and service accounts
```

### 40. Deployment Status Monitor
```bash
#!/bin/bash
# Deployment Status Monitor
NAMESPACE=${1:-default}
echo "Deployment status in namespace: $NAMESPACE"
kubectl get deployments -n $NAMESPACE -o custom-columns=NAME:.metadata.name,DESIRED:.spec.replicas,CURRENT:.status.replicas,READY:.status.readyReplicas,UP-TO-DATE:.status.updatedReplicas,AVAILABLE:.status.availableReplicas
# Use: Monitor deployment status
# Example: ./deployment-monitor.sh production
# Notes: Shows deployment replica status
```

---

## 📊 Monitoring Scripts

### 41. Cluster Health Dashboard
```bash
#!/bin/bash
# Cluster Health Dashboard
echo "=== CLUSTER HEALTH DASHBOARD ==="
echo -e "\n1. Node Status:"
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status
echo -e "\n2. Pod Status:"
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase | grep -v "Running"
echo -e "\n3. Recent Events:"
kubectl get events --sort-by='.lastTimestamp' | tail -10
echo -e "\n4. Resource Usage:"
kubectl top nodes --no-headers | head -5
# Use: Quick cluster health overview
# Example: ./cluster-dashboard.sh
# Notes: Comprehensive cluster status
```

### 42. Performance Monitor
```bash
#!/bin/bash
# Performance Monitor
echo "=== PERFORMANCE MONITOR ==="
echo -e "\nTop CPU Consumers:"
kubectl top pods --all-namespaces --sort-by=cpu --no-headers | head -10
echo -e "\nTop Memory Consumers:"
kubectl top pods --all-namespaces --sort-by=memory --no-headers | head -10
echo -e "\nNode Resource Usage:"
kubectl top nodes --no-headers
# Use: Monitor performance metrics
# Example: ./performance-monitor.sh
# Notes: Shows resource consumption patterns
```

### 43. Network Policy Monitor
```bash
#!/bin/bash
# Network Policy Monitor
echo "=== NETWORK POLICIES ==="
kubectl get networkpolicies --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,POD-SELECTOR:.spec.podSelector.matchLabels
echo -e "\n=== INGRESS RESOURCES ==="
kubectl get ingress --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,ADDRESS:.status.loadBalancer.ingress[0].ip
# Use: Monitor network policies
# Example: ./network-monitor.sh
# Notes: Shows network security configuration
```

### 44. Storage Monitor
```bash
#!/bin/bash
# Storage Monitor
echo "=== STORAGE MONITOR ==="
echo -e "\nPersistent Volumes:"
kubectl get pv -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.capacity.storage,STORAGE-CLASS:.spec.storageClassName
echo -e "\nPersistent Volume Claims:"
kubectl get pvc --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,CAPACITY:.spec.resources.requests.storage
# Use: Monitor storage resources
# Example: ./storage-monitor.sh
# Notes: Shows storage usage and status
```

### 45. Security Monitor
```bash
#!/bin/bash
# Security Monitor
echo "=== SECURITY MONITOR ==="
echo -e "\nService Accounts:"
kubectl get serviceaccounts --all-namespaces | wc -l
echo -e "\nSecrets:"
kubectl get secrets --all-namespaces | wc -l
echo -e "\nCluster Roles:"
kubectl get clusterroles | wc -l
echo -e "\nCluster Role Bindings:"
kubectl get clusterrolebindings | wc -l
# Use: Monitor security resources
# Example: ./security-monitor.sh
# Notes: Shows security resource counts
```

---

## 🔧 Troubleshooting Scripts

### 46. Pod Troubleshooting Script
```bash
#!/bin/bash
# Pod Troubleshooting Script
POD_NAME=$1
NAMESPACE=${2:-default}
echo "=== TROUBLESHOOTING POD: $POD_NAME ==="
echo -e "\n1. Pod Status:"
kubectl get pod $POD_NAME -n $NAMESPACE -o wide
echo -e "\n2. Pod Description:"
kubectl describe pod $POD_NAME -n $NAMESPACE
echo -e "\n3. Pod Logs:"
kubectl logs $POD_NAME -n $NAMESPACE --tail=50
echo -e "\n4. Pod Events:"
kubectl get events --field-selector involvedObject.name=$POD_NAME --sort-by='.lastTimestamp'
# Use: Comprehensive pod troubleshooting
# Example: ./pod-troubleshoot.sh my-pod production
# Notes: Shows complete pod information
```

### 47. Service Troubleshooting Script
```bash
#!/bin/bash
# Service Troubleshooting Script
SERVICE_NAME=$1
NAMESPACE=${2:-default}
echo "=== TROUBLESHOOTING SERVICE: $SERVICE_NAME ==="
echo -e "\n1. Service Details:"
kubectl get service $SERVICE_NAME -n $NAMESPACE -o wide
echo -e "\n2. Service Description:"
kubectl describe service $SERVICE_NAME -n $NAMESPACE
echo -e "\n3. Endpoints:"
kubectl get endpoints $SERVICE_NAME -n $NAMESPACE
echo -e "\n4. Related Pods:"
kubectl get pods -n $NAMESPACE -l $(kubectl get service $SERVICE_NAME -n $NAMESPACE -o jsonpath='{.spec.selector}' | tr -d '{}' | tr ':' '=')
# Use: Comprehensive service troubleshooting
# Example: ./service-troubleshoot.sh my-service production
# Notes: Shows complete service information
```

### 48. Node Troubleshooting Script
```bash
#!/bin/bash
# Node Troubleshooting Script
NODE_NAME=$1
echo "=== TROUBLESHOOTING NODE: $NODE_NAME ==="
echo -e "\n1. Node Status:"
kubectl get node $NODE_NAME -o wide
echo -e "\n2. Node Description:"
kubectl describe node $NODE_NAME
echo -e "\n3. Pods on Node:"
kubectl get pods --all-namespaces --field-selector spec.nodeName=$NODE_NAME
echo -e "\n4. Node Events:"
kubectl get events --field-selector involvedObject.name=$NODE_NAME --sort-by='.lastTimestamp'
# Use: Comprehensive node troubleshooting
# Example: ./node-troubleshoot.sh worker-1
# Notes: Shows complete node information
```

### 49. Namespace Troubleshooting Script
```bash
#!/bin/bash
# Namespace Troubleshooting Script
NAMESPACE=$1
echo "=== TROUBLESHOOTING NAMESPACE: $NAMESPACE ==="
echo -e "\n1. All Resources:"
kubectl get all -n $NAMESPACE
echo -e "\n2. Events:"
kubectl get events -n $NAMESPACE --sort-by='.lastTimestamp' | tail -20
echo -e "\n3. Resource Quotas:"
kubectl get resourcequota -n $NAMESPACE
echo -e "\n4. Limit Ranges:"
kubectl get limitrange -n $NAMESPACE
# Use: Comprehensive namespace troubleshooting
# Example: ./namespace-troubleshoot.sh production
# Notes: Shows complete namespace information
```

### 50. Cluster Troubleshooting Script
```bash
#!/bin/bash
# Cluster Troubleshooting Script
echo "=== CLUSTER TROUBLESHOOTING ==="
echo -e "\n1. Cluster Info:"
kubectl cluster-info
echo -e "\n2. Node Status:"
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status
echo -e "\n3. Component Status:"
kubectl get componentstatuses
echo -e "\n4. Recent Events:"
kubectl get events --all-namespaces --sort-by='.lastTimestamp' | tail -20
# Use: Comprehensive cluster troubleshooting
# Example: ./cluster-troubleshoot.sh
# Notes: Shows complete cluster information
```

---

## 🛠️ Utility Functions

### 51. Quick Context Switcher
```bash
# Add to .bashrc or .zshrc
kctx() {
    kubectl config use-context $1
    kubectl config set-context --current --namespace=${2:-default}
    echo "Switched to context: $1, namespace: ${2:-default}"
}
# Use: Quick context and namespace switching
# Example: kctx production-cluster production
# Notes: Convenient context switching
```

### 52. Pod Log Tailer
```bash
# Add to .bashrc or .zshrc
klogs() {
    kubectl logs -f $1 --tail=100 --timestamps
}
# Use: Follow pod logs with timestamps
# Example: klogs my-pod
# Notes: Convenient log following
```

### 53. Pod Exec Helper
```bash
# Add to .bashrc or .zshrc
kexec() {
    kubectl exec -it $1 -- ${2:-/bin/bash}
}
# Use: Quick pod execution
# Example: kexec my-pod /bin/sh
# Notes: Convenient pod access
```

### 54. Resource Getter
```bash
# Add to .bashrc or .zshrc
kget() {
    kubectl get $1 -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,AGE:.metadata.creationTimestamp
}
# Use: Quick resource status
# Example: kget pods
# Notes: Convenient resource checking
```

### 55. Event Monitor
```bash
# Add to .bashrc or .zshrc
kevents() {
    kubectl get events --sort-by='.lastTimestamp' -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,REASON:.reason,MESSAGE:.message | tail -20
}
# Use: Quick event monitoring
# Example: kevents
# Notes: Convenient event checking
```

### 56. Resource Usage Monitor
```bash
# Add to .bashrc or .zshrc
kusage() {
    kubectl top pods --sort-by=cpu --no-headers | head -10
}
# Use: Quick resource usage check
# Example: kusage
# Notes: Convenient resource monitoring
```

### 57. Service Port Forwarder
```bash
# Add to .bashrc or .zshrc
kport() {
    kubectl port-forward service/$1 8080:80 &
    echo "Port forwarding to service $1 on localhost:8080"
}
# Use: Quick service port forwarding
# Example: kport my-service
# Notes: Convenient service access
```

### 58. Pod Status Checker
```bash
# Add to .bashrc or .zshrc
kstatus() {
    kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,READY:.status.containerStatuses[0].ready,RESTARTS:.status.containerStatuses[0].restartCount
}
# Use: Quick pod status check
# Example: kstatus
# Notes: Convenient pod monitoring
```

### 59. Namespace Resource Counter
```bash
# Add to .bashrc or .zshrc
kcount() {
    NAMESPACE=${1:-default}
    echo "Resource count in namespace: $NAMESPACE"
    echo "Pods: $(kubectl get pods -n $NAMESPACE --no-headers | wc -l)"
    echo "Services: $(kubectl get services -n $NAMESPACE --no-headers | wc -l)"
    echo "Deployments: $(kubectl get deployments -n $NAMESPACE --no-headers | wc -l)"
}
# Use: Count resources in namespace
# Example: kcount production
# Notes: Convenient resource counting
```

### 60. Cluster Health Checker
```bash
# Add to .bashrc or .zshrc
khealth() {
    echo "=== CLUSTER HEALTH ==="
    echo "Nodes: $(kubectl get nodes --no-headers | grep -c Ready)/$(kubectl get nodes --no-headers | wc -l) ready"
    echo "Pods: $(kubectl get pods --all-namespaces --no-headers | grep -c Running)/$(kubectl get pods --all-namespaces --no-headers | wc -l) running"
    echo "Recent events: $(kubectl get events --sort-by='.lastTimestamp' | tail -5 | grep -c Warning) warnings"
}
# Use: Quick cluster health check
# Example: khealth
# Notes: Convenient health monitoring
```

---

## 🎯 Quick Reference

### Most Used One-Liners
```bash
# Quick diagnostics
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,RESTARTS:.status.containerStatuses[0].restartCount,AGE:.metadata.creationTimestamp
kubectl get events --sort-by='.lastTimestamp' -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,REASON:.reason,MESSAGE:.message
kubectl top pods --no-headers | sort -k2 -nr | head -10

# Quick actions
kubectl delete pods --all -n <namespace>
kubectl scale deployment --all --replicas=0
kubectl logs -l app=myapp --tail=100
kubectl exec -l app=myapp -- ls /app

# Advanced one-liners
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.phase}{"\n"}{end}'
kubectl get services -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.selector}{"\n"}{end}'
kubectl get deployments -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.template.spec.containers[0].image}{"\n"}{end}'
```

### Common Scripts
```bash
# Health checks
./pod-health-check.sh production
./cluster-dashboard.sh
./performance-monitor.sh

# Troubleshooting
./pod-troubleshoot.sh my-pod production
./service-troubleshoot.sh my-service production
./cluster-troubleshoot.sh

# Monitoring
./resource-monitor.sh
./event-monitor.sh
./storage-monitor.sh
```

---

*This file contains 60 one-liners and scripts for kubectl automation and quick operations.* 