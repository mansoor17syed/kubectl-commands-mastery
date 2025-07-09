# 🔧 Intermediate Kubectl Commands
## Advanced Operations for Kubernetes Power Users

> **60 intermediate commands for advanced resource management and debugging**

---

## 📋 Table of Contents
- [Advanced Resource Management](#advanced-resource-management)
- [Deployment Operations](#deployment-operations)
- [Config Management](#config-management)
- [Multi-Resource Operations](#multi-resource-operations)
- [Advanced Filtering](#advanced-filtering)
- [Resource Optimization](#resource-optimization)
- [Advanced Debugging](#advanced-debugging)
- [Network Operations](#network-operations)

---

## 📦 Advanced Resource Management

### 1. Create Resource from File
```bash
kubectl apply -f <filename>
# Use: Create or update resources from YAML/JSON
# Example: kubectl apply -f deployment.yaml
# Notes: Idempotent operation - safe to run multiple times
```

### 2. Create Resource with Dry Run
```bash
kubectl apply -f <filename> --dry-run=client
# Use: Preview changes without applying them
# Example: kubectl apply -f deployment.yaml --dry-run=client
# Notes: Useful for validating manifests before applying
```

### 3. Create Resource with Server Validation
```bash
kubectl apply -f <filename> --dry-run=server
# Use: Validate against server without applying
# Example: kubectl apply -f deployment.yaml --dry-run=server
# Notes: More thorough validation than client dry-run
```

### 4. Delete Resource
```bash
kubectl delete <resource> <name>
# Use: Delete specific resource
# Example: kubectl delete pod my-pod
# Notes: Immediate deletion, no confirmation
```

### 5. Delete Resource with Grace Period
```bash
kubectl delete <resource> <name> --grace-period=30
# Use: Delete resource with custom grace period
# Example: kubectl delete pod my-pod --grace-period=60
# Notes: Gives time for graceful shutdown
```

### 6. Force Delete Resource
```bash
kubectl delete <resource> <name> --force --grace-period=0
# Use: Force delete resource immediately
# Example: kubectl delete pod my-pod --force --grace-period=0
# Notes: Dangerous - bypasses graceful shutdown
```

### 7. Delete Resource from File
```bash
kubectl delete -f <filename>
# Use: Delete resources defined in file
# Example: kubectl delete -f deployment.yaml
# Notes: Deletes all resources in the file
```

### 8. Replace Resource
```bash
kubectl replace -f <filename>
# Use: Replace entire resource (delete and recreate)
# Example: kubectl replace -f deployment.yaml
# Notes: Destructive operation - recreates resource
```

### 9. Force Replace Resource
```bash
kubectl replace --force -f <filename>
# Use: Force replace even if resource doesn't exist
# Example: kubectl replace --force -f deployment.yaml
# Notes: Useful when resource might not exist
```

### 10. Patch Resource
```bash
kubectl patch deployment <name> -p '{"spec":{"replicas":5}}'
# Use: Apply partial updates to resources
# Example: kubectl patch deployment my-app -p '{"spec":{"replicas":3}}'
# Notes: Non-destructive way to update specific fields
```

---

## 🚀 Deployment Operations

### 11. Scale Deployment
```bash
kubectl scale deployment <name> --replicas=3
# Use: Scale deployment to specific number of replicas
# Example: kubectl scale deployment my-app --replicas=5
# Notes: Immediate scaling operation
```

### 12. Scale Deployment with Current Replicas
```bash
kubectl scale deployment <name> --current-replicas=2 --replicas=5
# Use: Scale only if current replicas match
# Example: kubectl scale deployment my-app --current-replicas=2 --replicas=5
# Notes: Safe scaling with precondition
```

### 13. Set Deployment Image
```bash
kubectl set image deployment/<name> <container>=<new-image>
# Use: Update container image in deployment
# Example: kubectl set image deployment/my-app nginx=nginx:1.19
# Notes: Triggers rolling update
```

### 14. Set Deployment Environment Variable
```bash
kubectl set env deployment/<name> <key>=<value>
# Use: Set environment variable in deployment
# Example: kubectl set env deployment/my-app DEBUG=true
# Notes: Triggers rolling update
```

### 15. Remove Environment Variable
```bash
kubectl set env deployment/<name> <key>-
# Use: Remove environment variable from deployment
# Example: kubectl set env deployment/my-app DEBUG-
# Notes: The dash (-) removes the variable
```

### 16. Set Resource Limits
```bash
kubectl set resources deployment/<name> --limits=cpu=200m,memory=512Mi
# Use: Set resource limits for deployment
# Example: kubectl set resources deployment/my-app --limits=cpu=500m,memory=1Gi
# Notes: Affects all containers in deployment
```

### 17. Set Resource Requests
```bash
kubectl set resources deployment/<name> --requests=cpu=100m,memory=256Mi
# Use: Set resource requests for deployment
# Example: kubectl set resources deployment/my-app --requests=cpu=200m,memory=512Mi
# Notes: Minimum resources required
```

### 18. Rollout Status
```bash
kubectl rollout status deployment/<name>
# Use: Monitor deployment rollout progress
# Example: kubectl rollout status deployment/my-app
# Notes: Shows real-time rollout progress
```

### 19. Rollout History
```bash
kubectl rollout history deployment/<name>
# Use: View deployment rollout history
# Example: kubectl rollout history deployment/my-app
# Notes: Shows revision history
```

### 20. Rollout Undo
```bash
kubectl rollout undo deployment/<name>
# Use: Rollback to previous deployment version
# Example: kubectl rollout undo deployment/my-app
# Notes: Reverts to previous revision
```

### 21. Rollout Undo to Specific Revision
```bash
kubectl rollout undo deployment/<name> --to-revision=2
# Use: Rollback to specific revision
# Example: kubectl rollout undo deployment/my-app --to-revision=2
# Notes: Reverts to specific revision number
```

### 22. Pause Rollout
```bash
kubectl rollout pause deployment/<name>
# Use: Pause deployment rollout
# Example: kubectl rollout pause deployment/my-app
# Notes: Stops rolling updates
```

### 23. Resume Rollout
```bash
kubectl rollout resume deployment/<name>
# Use: Resume paused deployment rollout
# Example: kubectl rollout resume deployment/my-app
# Notes: Continues rolling updates
```

---

## ⚙️ Config Management

### 24. View Current Context
```bash
kubectl config current-context
# Use: Check which cluster/context is active
# Example: Shows current context name
# Notes: Essential for multi-cluster environments
```

### 25. List All Contexts
```bash
kubectl config get-contexts
# Use: List all available contexts
# Example: Shows all configured clusters/contexts
# Notes: Useful for context management
```

### 26. Switch Context
```bash
kubectl config use-context <context-name>
# Use: Switch to different cluster/context
# Example: kubectl config use-context production-cluster
# Notes: Changes active cluster
```

### 27. View Kubeconfig
```bash
kubectl config view
# Use: Display current kubeconfig
# Example: Shows current configuration
# Notes: Useful for debugging config issues
```

### 28. View Kubeconfig in Raw Format
```bash
kubectl config view --raw
# Use: Display raw kubeconfig without redaction
# Example: Shows full configuration including certificates
# Notes: Useful for debugging authentication issues
```

### 29. Set Namespace for Current Context
```bash
kubectl config set-context --current --namespace=<namespace>
# Use: Set default namespace for current context
# Example: kubectl config set-context --current --namespace=production
# Notes: Affects all subsequent commands
```

### 30. Set Cluster in Context
```bash
kubectl config set-context <context-name> --cluster=<cluster-name>
# Use: Set cluster for specific context
# Example: kubectl config set-context my-context --cluster=my-cluster
# Notes: Useful for context configuration
```

### 31. Set User in Context
```bash
kubectl config set-context <context-name> --user=<user-name>
# Use: Set user for specific context
# Example: kubectl config set-context my-context --user=my-user
# Notes: Useful for context configuration
```

### 32. Delete Context
```bash
kubectl config delete-context <context-name>
# Use: Delete specific context
# Example: kubectl config delete-context old-cluster
# Notes: Removes context from kubeconfig
```

---

## 🔄 Multi-Resource Operations

### 33. Get Multiple Resource Types
```bash
kubectl get pods,services,deployments
# Use: Get multiple resource types in one command
# Example: kubectl get pods,services,deployments,configmaps
# Notes: Efficient way to get multiple resource types
```

### 34. Get All Resources in Namespace
```bash
kubectl get all
# Use: Get all common resources in namespace
# Example: Shows pods, services, deployments, replicasets
# Notes: Quick overview of namespace
```

### 35. Get All Resources with Specific Label
```bash
kubectl get all -l app=myapp
# Use: Get all resources with specific label
# Example: kubectl get all -l environment=production
# Notes: Useful for application-specific resources
```

### 36. Watch Resources
```bash
kubectl get pods -w
# Use: Watch pods for changes in real-time
# Example: kubectl get pods -w -o wide
# Notes: Useful for monitoring deployments
```

### 37. Watch with Specific Format
```bash
kubectl get pods -w -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
# Use: Watch with custom output format
# Example: kubectl get pods -w -o custom-columns=NAME:.metadata.name,IP:.status.podIP
# Notes: Customized real-time monitoring
```

### 38. Apply Multiple Files
```bash
kubectl apply -f <directory>
# Use: Apply all YAML files in directory
# Example: kubectl apply -f ./manifests/
# Notes: Processes all .yaml and .yml files
```

### 39. Apply with Kustomize
```bash
kubectl apply -k <directory>
# Use: Apply Kustomize overlay
# Example: kubectl apply -k ./overlays/production/
# Notes: Uses Kustomize for configuration management
```

### 40. Delete Multiple Resources
```bash
kubectl delete pods,services,deployments -l app=myapp
# Use: Delete multiple resource types with label selector
# Example: kubectl delete all -l environment=staging
# Notes: Bulk deletion with label filtering
```

---

## 🔍 Advanced Filtering

### 41. Get Resources with Multiple Labels
```bash
kubectl get pods -l app=nginx,version=v1
# Use: Filter with multiple label criteria
# Example: kubectl get pods -l app=myapp,environment=production
# Notes: All labels must match (AND operation)
```

### 42. Get Resources with Label Set
```bash
kubectl get pods -l 'app in (nginx,apache)'
# Use: Filter with label set
# Example: kubectl get pods -l 'environment in (dev,staging)'
# Notes: IN operator for multiple values
```

### 43. Get Resources with Label Exists
```bash
kubectl get pods -l app
# Use: Filter resources that have specific label
# Example: kubectl get pods -l environment
# Notes: Returns resources with label regardless of value
```

### 44. Get Resources with Label Not Exists
```bash
kubectl get pods -l '!app'
# Use: Filter resources that don't have specific label
# Example: kubectl get pods -l '!environment'
# Notes: Exclamation mark negates the selector
```

### 45. Get Resources with Complex Selector
```bash
kubectl get pods -l 'app=nginx,version!=v1'
# Use: Complex label selector with negation
# Example: kubectl get pods -l 'app=myapp,environment!=production'
# Notes: Combines equality and inequality
```

### 46. Get Resources by Field Selector
```bash
kubectl get pods --field-selector status.phase=Running
# Use: Filter by specific field values
# Example: kubectl get pods --field-selector spec.nodeName=worker-1
# Notes: Filters by resource fields, not labels
```

### 47. Get Resources with Multiple Field Selectors
```bash
kubectl get pods --field-selector status.phase=Running,spec.nodeName=worker-1
# Use: Multiple field selectors
# Example: kubectl get pods --field-selector status.phase=Running,spec.restartPolicy=Always
# Notes: All field selectors must match
```

---

## ⚡ Resource Optimization

### 48. Get Resource Usage with Custom Output
```bash
kubectl top pods --no-headers | awk '{print $1, $2, $3}'
# Use: Get resource usage in custom format
# Example: kubectl top pods --no-headers | sort -k2 -nr
# Notes: Useful for resource analysis
```

### 49. Get Resource Usage for Specific Namespace
```bash
kubectl top pods -n <namespace>
# Use: Get resource usage in specific namespace
# Example: kubectl top pods -n production
# Notes: Namespace-specific resource monitoring
```

### 50. Get Node Resource Usage
```bash
kubectl top nodes
# Use: Get node CPU and memory usage
# Example: Shows resource consumption per node
# Notes: Requires metrics-server to be installed
```

### 51. Get Resource Quotas
```bash
kubectl get resourcequota
# Use: Check resource quotas
# Example: Shows quota limits and usage
# Notes: Useful for resource management
```

### 52. Get Limit Ranges
```bash
kubectl get limitrange
# Use: Check limit ranges
# Example: Shows default limits and requests
# Notes: Useful for resource policy enforcement
```

### 53. Describe Resource Quota
```bash
kubectl describe resourcequota <name>
# Use: Get detailed quota information
# Example: kubectl describe resourcequota compute-quota
# Notes: Shows quota details and usage
```

---

## 🐛 Advanced Debugging

### 54. Get Events with Custom Output
```bash
kubectl get events -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,KIND:.involvedObject.kind,REASON:.reason,MESSAGE:.message
# Use: Custom event output format
# Example: Shows formatted events for easier reading
# Notes: Useful for event analysis
```

### 55. Get Events for Specific Resource
```bash
kubectl get events --field-selector involvedObject.name=<pod-name>
# Use: Get events related to specific resource
# Example: kubectl get events --field-selector involvedObject.name=my-pod
# Notes: Useful for debugging specific resources
```

### 56. Get Events by Type
```bash
kubectl get events --field-selector type=Warning
# Use: Get only warning events
# Example: kubectl get events --field-selector type=Normal
# Notes: Filter events by type
```

### 57. Get Events by Reason
```bash
kubectl get events --field-selector reason=FailedScheduling
# Use: Get events by specific reason
# Example: kubectl get events --field-selector reason=BackOff
# Notes: Useful for specific issue debugging
```

### 58. Get Events with Time Range
```bash
kubectl get events --field-selector lastTimestamp>=2023-01-01T00:00:00Z
# Use: Get events within time range
# Example: kubectl get events --field-selector lastTimestamp>=2023-12-01T00:00:00Z
# Notes: ISO 8601 timestamp format
```

### 59. Get Events for Namespace
```bash
kubectl get events -n <namespace>
# Use: Get events in specific namespace
# Example: kubectl get events -n production
# Notes: Namespace-specific event monitoring
```

### 60. Get Events with Sorting
```bash
kubectl get events --sort-by='.lastTimestamp' --reverse
# Use: Get events sorted by time (newest first)
# Example: kubectl get events --sort-by='.lastTimestamp' --reverse
# Notes: Useful for recent event analysis
```

---

## 🌐 Network Operations

### 61. Start Proxy Server
```bash
kubectl proxy
# Use: Start local proxy to API server
# Example: kubectl proxy --port=8001
# Notes: Useful for API access and debugging
```

### 62. Proxy with Specific Port
```bash
kubectl proxy --port=8001
# Use: Start proxy on specific port
# Example: kubectl proxy --port=8080
# Notes: Custom port for proxy server
```

### 63. Proxy with Address Binding
```bash
kubectl proxy --address=0.0.0.0 --port=8001
# Use: Bind proxy to all interfaces
# Example: kubectl proxy --address=0.0.0.0 --port=8001
# Notes: Allows external access to proxy
```

### 64. Port Forward with Background
```bash
kubectl port-forward service/<service-name> 8080:80 &
# Use: Port forward in background
# Example: kubectl port-forward service/my-app 8080:80 &
# Notes: Runs in background, use & to detach
```

### 65. Port Forward with Multiple Ports
```bash
kubectl port-forward <pod-name> 8080:80 8443:443
# Use: Forward multiple ports
# Example: kubectl port-forward my-pod 8080:80 8443:443
# Notes: Multiple port mappings
```

---

## 🎯 Quick Reference

### Most Used Intermediate Commands
```bash
# Resource management
kubectl apply -f <file>              # Apply resources
kubectl delete -f <file>             # Delete resources
kubectl patch <resource> <name> -p   # Patch resources

# Deployment operations
kubectl scale deployment <name> --replicas=3
kubectl set image deployment/<name> <container>=<image>
kubectl rollout status deployment/<name>

# Multi-resource operations
kubectl get all                      # Get all resources
kubectl get pods,services,deployments
kubectl delete all -l app=myapp

# Advanced filtering
kubectl get pods -l app=nginx,version=v1
kubectl get pods --field-selector status.phase=Running
```

### Common Patterns
```bash
# Apply with validation
kubectl apply -f <file> --dry-run=client

# Scale with precondition
kubectl scale deployment <name> --current-replicas=2 --replicas=5

# Watch with custom output
kubectl get pods -w -o custom-columns=NAME:.metadata.name,STATUS:.status.phase

# Get events for debugging
kubectl get events --sort-by='.lastTimestamp' -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,REASON:.reason,MESSAGE:.message
```

---

## 📚 Next Steps

After mastering these intermediate commands, proceed to:
- [Advanced Commands](03-advanced-commands.md) - Expert-level operations
- [Expert Commands](04-expert-commands.md) - Master-level administration
- [One-Liners & Scripts](05-one-liners-scripts.md) - Ready-to-use commands

---

*This file contains 60 intermediate kubectl commands for Kubernetes power users.* 