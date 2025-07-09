# ⚡ Advanced Kubectl Commands
## Expert-Level Operations for Kubernetes Masters

> **70 advanced commands for deep debugging, API interactions, and custom operations**

---

## 📋 Table of Contents
- [API Server Interactions](#api-server-interactions)
- [Advanced Resource Management](#advanced-resource-management)
- [Custom Resource Definitions](#custom-resource-definitions)
- [Advanced Debugging](#advanced-debugging)
- [Performance Analysis](#performance-analysis)
- [Advanced Output Formatting](#advanced-output-formatting)
- [Security Operations](#security-operations)
- [Advanced Networking](#advanced-networking)

---

## 🌐 API Server Interactions

### 1. Direct API Call
```bash
kubectl get --raw /api/v1/namespaces/default/pods
# Use: Direct API server access
# Example: kubectl get --raw /api/v1/namespaces/kube-system/pods
# Notes: Bypasses kubectl formatting, returns raw JSON
```

### 2. Get API Discovery
```bash
kubectl get --raw /api/v1/
# Use: Discover available API endpoints
# Example: Shows all available API resources
# Notes: Essential for API exploration
```

### 3. Get Specific API Group
```bash
kubectl get --raw /apis/apps/v1/
# Use: Access specific API group
# Example: kubectl get --raw /apis/networking.k8s.io/v1/
# Notes: Useful for API group exploration
```

### 4. Custom API Call with Authentication
```bash
kubectl get --raw /api/v1/namespaces/default/pods -H "Authorization: Bearer $(kubectl get token)"
# Use: Authenticated API calls
# Example: kubectl get --raw /api/v1/namespaces/default/pods -H "Authorization: Bearer $(kubectl get token)"
# Notes: Manual authentication header
```

### 5. Get API Resources with Full Details
```bash
kubectl get --raw /api/v1/namespaces/default/pods | jq '.items[] | {name: .metadata.name, status: .status.phase}'
# Use: Process API response with jq
# Example: kubectl get --raw /api/v1/namespaces/default/pods | jq '.items[] | {name: .metadata.name, status: .status.phase}'
# Notes: Requires jq for JSON processing
```

### 6. Get API Server Metrics
```bash
kubectl get --raw /metrics
# Use: Get API server metrics
# Example: Shows Prometheus-formatted metrics
# Notes: Useful for monitoring API server performance
```

### 7. Get Health Check
```bash
kubectl get --raw /healthz
# Use: Check API server health
# Example: Returns "ok" if API server is healthy
# Notes: Simple health check endpoint
```

### 8. Get Ready Check
```bash
kubectl get --raw /readyz
# Use: Check API server readiness
# Example: Returns "ok" if API server is ready
# Notes: Readiness probe endpoint
```

### 9. Get Live Check
```bash
kubectl get --raw /livez
# Use: Check API server liveness
# Example: Returns "ok" if API server is alive
# Notes: Liveness probe endpoint
```

### 10. Get API Server Logs
```bash
kubectl logs -n kube-system kube-apiserver-<node-name>
# Use: Get API server logs
# Example: kubectl logs -n kube-system kube-apiserver-master-1
# Notes: Requires access to kube-system namespace
```

---

## 📦 Advanced Resource Management

### 11. Strategic Merge Patch
```bash
kubectl patch deployment <name> --type='strategic' -p='{"spec":{"template":{"spec":{"containers":[{"name":"nginx","image":"nginx:1.19"}]}}}}'
# Use: Strategic merge for complex updates
# Example: kubectl patch deployment my-app --type='strategic' -p='{"spec":{"template":{"spec":{"containers":[{"name":"app","image":"myapp:v2"}]}}}}'
# Notes: Intelligent merging of arrays and objects
```

### 12. JSON Patch
```bash
kubectl patch deployment <name> --type='json' -p='[{"op": "replace", "path": "/spec/replicas", "value": 3}]'
# Use: JSON patch for precise updates
# Example: kubectl patch deployment my-app --type='json' -p='[{"op": "replace", "path": "/spec/replicas", "value": 5}]'
# Notes: RFC 6902 JSON patch operations
```

### 13. Merge Patch
```bash
kubectl patch deployment <name> --type='merge' -p='{"spec":{"replicas":3}}'
# Use: Merge patch for simple updates
# Example: kubectl patch deployment my-app --type='merge' -p='{"spec":{"replicas":3}}'
# Notes: Simple object merging
```

### 14. Apply with Server-Side Apply
```bash
kubectl apply -f <filename> --server-side
# Use: Server-side apply for conflict resolution
# Example: kubectl apply -f deployment.yaml --server-side
# Notes: Resolves conflicts on server side
```

### 15. Apply with Field Manager
```bash
kubectl apply -f <filename> --server-side --field-manager=my-tool
# Use: Apply with custom field manager
# Example: kubectl apply -f deployment.yaml --server-side --field-manager=helm
# Notes: Tracks ownership of fields
```

### 16. Diff Resources
```bash
kubectl diff -f <filename>
# Use: Show differences between local and server state
# Example: kubectl diff -f deployment.yaml
# Notes: Shows what would change
```

### 17. Diff with Server-Side Apply
```bash
kubectl diff -f <filename> --server-side
# Use: Diff with server-side apply
# Example: kubectl diff -f deployment.yaml --server-side
# Notes: Shows server-side differences
```

### 18. Validate Resource
```bash
kubectl apply -f <filename> --validate=true
# Use: Validate resource before applying
# Example: kubectl apply -f deployment.yaml --validate=true
# Notes: Server-side validation
```

### 19. Apply with Override
```bash
kubectl apply -f <filename> --overrides='{"spec":{"replicas":3}}'
# Use: Apply with field overrides
# Example: kubectl apply -f deployment.yaml --overrides='{"spec":{"replicas":5}}'
# Notes: Override specific fields during apply
```

### 20. Create with Generator
```bash
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml
# Use: Generate YAML from kubectl create
# Example: kubectl create deployment my-app --image=nginx --dry-run=client -o yaml
# Notes: Generate manifests from kubectl commands
```

---

## 🔧 Custom Resource Definitions

### 21. Get CRDs
```bash
kubectl get crd
# Use: List custom resource definitions
# Example: Shows all CRDs in the cluster
# Notes: Essential for custom resources
```

### 22. Get Custom Resources
```bash
kubectl get <crd-name>
# Use: List instances of custom resource
# Example: kubectl get virtualservices.networking.istio.io
# Notes: Lists custom resource instances
```

### 23. Describe CRD
```bash
kubectl describe crd <crd-name>
# Use: Get detailed CRD information
# Example: kubectl describe crd virtualservices.networking.istio.io
# Notes: Shows CRD schema and details
```

### 24. Get CRD Schema
```bash
kubectl get crd <crd-name> -o yaml
# Use: Get CRD YAML with schema
# Example: kubectl get crd virtualservices.networking.istio.io -o yaml
# Notes: Full CRD definition
```

### 25. Get Custom Resource with Custom Output
```bash
kubectl get <crd-name> -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
# Use: Custom output for custom resources
# Example: kubectl get virtualservices -o custom-columns=NAME:.metadata.name,GATEWAYS:.spec.gateways
# Notes: Custom columns for custom resources
```

### 26. Watch Custom Resources
```bash
kubectl get <crd-name> -w
# Use: Watch custom resources for changes
# Example: kubectl get virtualservices -w
# Notes: Real-time monitoring of custom resources
```

### 27. Get Custom Resource Events
```bash
kubectl get events --field-selector involvedObject.kind=<crd-kind>
# Use: Get events for custom resources
# Example: kubectl get events --field-selector involvedObject.kind=VirtualService
# Notes: Events related to custom resources
```

---

## 🐛 Advanced Debugging

### 28. Debug Pod with Ephemeral Container
```bash
kubectl debug <pod-name> -it --image=busybox --target=<container-name>
# Use: Debug running pod with ephemeral container
# Example: kubectl debug my-pod -it --image=busybox --target=app
# Notes: Adds temporary container for debugging
```

### 29. Debug Node
```bash
kubectl debug node/<node-name> -it --image=busybox
# Use: Debug node with privileged container
# Example: kubectl debug node/worker-1 -it --image=busybox
# Notes: Requires privileged access
```

### 30. Debug with Copy
```bash
kubectl debug <pod-name> -it --image=busybox --copy-to=debug-pod
# Use: Debug by copying pod
# Example: kubectl debug my-pod -it --image=busybox --copy-to=debug-pod
# Notes: Creates copy of pod for debugging
```

### 31. Get Pod with Ephemeral Containers
```bash
kubectl get pod <pod-name> -o yaml | grep -A 10 ephemeralContainers
# Use: Check ephemeral containers in pod
# Example: kubectl get pod my-pod -o yaml | grep -A 10 ephemeralContainers
# Notes: Shows ephemeral container details
```

### 32. Get Container Logs with Previous
```bash
kubectl logs <pod-name> --previous --tail=100
# Use: Get previous container logs
# Example: kubectl logs my-pod --previous --tail=100
# Notes: Useful when container has restarted
```

### 33. Get Logs from Init Containers
```bash
kubectl logs <pod-name> -c <init-container-name>
# Use: Get logs from init container
# Example: kubectl logs my-pod -c init-db
# Notes: Init container logs
```

### 34. Get Pod Status with Conditions
```bash
kubectl get pod <pod-name> -o jsonpath='{.status.conditions[*]}'
# Use: Get pod conditions
# Example: kubectl get pod my-pod -o jsonpath='{.status.conditions[*]}'
# Notes: Detailed pod status information
```

### 35. Get Pod Events with Timestamps
```bash
kubectl get events --field-selector involvedObject.name=<pod-name> --sort-by='.lastTimestamp'
# Use: Get events for specific pod with timestamps
# Example: kubectl get events --field-selector involvedObject.name=my-pod --sort-by='.lastTimestamp'
# Notes: Chronological pod events
```

---

## 📊 Performance Analysis

### 36. Get Resource Usage with Sorting
```bash
kubectl top pods --sort-by=cpu
# Use: Sort pods by CPU usage
# Example: kubectl top pods --sort-by=memory
# Notes: Identify resource-intensive pods
```

### 37. Get Node Resource Usage with Sorting
```bash
kubectl top nodes --sort-by=cpu
# Use: Sort nodes by CPU usage
# Example: kubectl top nodes --sort-by=memory
# Notes: Identify resource-intensive nodes
```

### 38. Get Resource Usage for All Namespaces
```bash
kubectl top pods --all-namespaces
# Use: Get resource usage across all namespaces
# Example: Shows resource usage for all pods
# Notes: Cluster-wide resource monitoring
```

### 39. Get Resource Usage with Custom Output
```bash
kubectl top pods --no-headers | awk '{print $1, $2, $3}' | sort -k2 -nr
# Use: Custom resource usage output
# Example: kubectl top pods --no-headers | awk '{print $1, $2, $3}' | sort -k2 -nr
# Notes: Sorted by CPU usage
```

### 40. Get Resource Quota Usage
```bash
kubectl describe resourcequota | grep -A 5 "Used"
# Use: Get resource quota usage details
# Example: kubectl describe resourcequota | grep -A 5 "Used"
# Notes: Shows quota utilization
```

### 41. Get Limit Range Details
```bash
kubectl describe limitrange | grep -A 10 "Limits"
# Use: Get limit range details
# Example: kubectl describe limitrange | grep -A 10 "Limits"
# Notes: Shows default limits and requests
```

### 42. Get Pod Resource Requests and Limits
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,CPU-REQUEST:.spec.containers[0].resources.requests.cpu,CPU-LIMIT:.spec.containers[0].resources.limits.cpu,MEMORY-REQUEST:.spec.containers[0].resources.requests.memory,MEMORY-LIMIT:.spec.containers[0].resources.limits.memory
# Use: Get pod resource configuration
# Example: Shows resource requests and limits for all pods
# Notes: Useful for resource analysis
```

---

## 📝 Advanced Output Formatting

### 43. Get with Go Template
```bash
kubectl get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
# Use: Custom output using Go templates
# Example: kubectl get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
# Notes: Powerful templating for custom output
```

### 44. Get with JSONPath
```bash
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
# Use: Extract specific fields using JSONPath
# Example: kubectl get pods -o jsonpath='{.items[*].metadata.name}'
# Notes: JSONPath for field extraction
```

### 45. Get with Multiple JSONPath Expressions
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.phase}{"\n"}{end}'
# Use: Multiple field extraction
# Example: kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.phase}{"\n"}{end}'
# Notes: Tab-separated output
```

### 46. Get with Custom Columns and Sorting
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,AGE:.metadata.creationTimestamp --sort-by=.metadata.creationTimestamp
# Use: Custom columns with sorting
# Example: kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,AGE:.metadata.creationTimestamp --sort-by=.metadata.creationTimestamp
# Notes: Sorted custom output
```

### 47. Get with Template File
```bash
kubectl get pods -o go-template-file=template.txt
# Use: Use external template file
# Example: kubectl get pods -o go-template-file=template.txt
# Notes: External template for complex formatting
```

### 48. Get with Custom Columns for Services
```bash
kubectl get services -o custom-columns=NAME:.metadata.name,CLUSTER-IP:.spec.clusterIP,EXTERNAL-IP:.status.loadBalancer.ingress[0].ip,PORT:.spec.ports[0].port
# Use: Custom service output
# Example: Shows service details in custom format
# Notes: Useful for service monitoring
```

### 49. Get with Custom Columns for Nodes
```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status,VERSION:.status.nodeInfo.kubeletVersion,AGE:.metadata.creationTimestamp
# Use: Custom node output
# Example: Shows node details in custom format
# Notes: Useful for node monitoring
```

### 50. Get with Custom Columns for Events
```bash
kubectl get events -o custom-columns=TIME:.lastTimestamp,NAME:.involvedObject.name,KIND:.involvedObject.kind,REASON:.reason,MESSAGE:.message
# Use: Custom event output
# Example: Shows events in custom format
# Notes: Useful for event monitoring
```

---

## 🔒 Security Operations

### 51. Get Service Accounts
```bash
kubectl get serviceaccounts
# Use: List service accounts
# Example: Shows all service accounts in namespace
# Notes: Essential for RBAC understanding
```

### 52. Get Roles
```bash
kubectl get roles
# Use: List roles
# Example: Shows all roles in namespace
# Notes: Namespace-scoped roles
```

### 53. Get Role Bindings
```bash
kubectl get rolebindings
# Use: List role bindings
# Example: Shows all role bindings in namespace
# Notes: Namespace-scoped role bindings
```

### 54. Get Cluster Roles
```bash
kubectl get clusterroles
# Use: List cluster roles
# Example: Shows all cluster roles
# Notes: Cluster-scoped roles
```

### 55. Get Cluster Role Bindings
```bash
kubectl get clusterrolebindings
# Use: List cluster role bindings
# Example: Shows all cluster role bindings
# Notes: Cluster-scoped role bindings
```

### 56. Check Permissions
```bash
kubectl auth can-i create pods
# Use: Check if current user can perform action
# Example: kubectl auth can-i delete pods
# Notes: Permission checking
```

### 57. Check Permissions for Specific Namespace
```bash
kubectl auth can-i create pods --namespace=default
# Use: Check permissions in specific namespace
# Example: kubectl auth can-i create pods --namespace=production
# Notes: Namespace-specific permission checking
```

### 58. Get Secrets
```bash
kubectl get secrets
# Use: List secrets
# Example: Shows all secrets in namespace
# Notes: Sensitive data storage
```

### 59. Get Secret Details
```bash
kubectl get secret <secret-name> -o yaml
# Use: Get secret details
# Example: kubectl get secret my-secret -o yaml
# Notes: Shows secret structure (values are base64 encoded)
```

### 60. Get ConfigMaps
```bash
kubectl get configmaps
# Use: List config maps
# Example: Shows all config maps in namespace
# Notes: Configuration data storage
```

---

## 🌐 Advanced Networking

### 61. Get Network Policies
```bash
kubectl get networkpolicies
# Use: List network policies
# Example: Shows all network policies in namespace
# Notes: Network security policies
```

### 62. Get Ingress Resources
```bash
kubectl get ingress
# Use: List ingress resources
# Example: Shows all ingress resources in namespace
# Notes: External access configuration
```

### 63. Get Endpoints
```bash
kubectl get endpoints
# Use: List endpoints
# Example: Shows all endpoints in namespace
# Notes: Service endpoint details
```

### 64. Get Endpoint Slices
```bash
kubectl get endpointslices
# Use: List endpoint slices
# Example: Shows all endpoint slices in namespace
# Notes: Modern endpoint management
```

### 65. Get Services with External IPs
```bash
kubectl get services -o custom-columns=NAME:.metadata.name,TYPE:.spec.type,EXTERNAL-IP:.status.loadBalancer.ingress[0].ip
# Use: Get services with external IPs
# Example: Shows services with external access
# Notes: External service monitoring
```

### 66. Get Pod Network Information
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,IP:.status.podIP,NODE:.spec.nodeName
# Use: Get pod network information
# Example: Shows pod IPs and nodes
# Notes: Network troubleshooting
```

### 67. Get Service Endpoints
```bash
kubectl get endpoints <service-name> -o yaml
# Use: Get detailed endpoint information
# Example: kubectl get endpoints my-service -o yaml
# Notes: Shows which pods the service routes to
```

### 68. Get Network Policy Details
```bash
kubectl describe networkpolicy <name>
# Use: Get network policy details
# Example: kubectl describe networkpolicy default-deny
# Notes: Shows network policy rules
```

### 69. Get Ingress Details
```bash
kubectl describe ingress <name>
# Use: Get ingress details
# Example: kubectl describe ingress my-ingress
# Notes: Shows ingress configuration
```

### 70. Get Services by Type
```bash
kubectl get services --field-selector spec.type=LoadBalancer
# Use: Get services by type
# Example: kubectl get services --field-selector spec.type=NodePort
# Notes: Filter services by type
```

---

## 🎯 Quick Reference

### Most Used Advanced Commands
```bash
# API server interactions
kubectl get --raw /api/v1/namespaces/default/pods
kubectl get --raw /metrics
kubectl get --raw /healthz

# Advanced resource management
kubectl patch deployment <name> --type='strategic' -p
kubectl apply -f <file> --server-side
kubectl diff -f <file>

# Custom resources
kubectl get crd
kubectl get <crd-name>
kubectl describe crd <crd-name>

# Advanced debugging
kubectl debug <pod-name> -it --image=busybox
kubectl get events --field-selector involvedObject.name=<pod-name>

# Advanced output
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
kubectl get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
```

### Common Patterns
```bash
# API exploration
kubectl get --raw /api/v1/ | jq '.resources[].name'

# Custom resource monitoring
kubectl get <crd-name> -w -o custom-columns=NAME:.metadata.name,STATUS:.status.phase

# Performance analysis
kubectl top pods --sort-by=cpu --no-headers | head -10

# Security audit
kubectl auth can-i --list
kubectl get clusterrolebindings -o custom-columns=NAME:.metadata.name,SUBJECTS:.subjects[*].name
```

---

## 📚 Next Steps

After mastering these advanced commands, proceed to:
- [Expert Commands](04-expert-commands.md) - Master-level administration
- [One-Liners & Scripts](05-one-liners-scripts.md) - Ready-to-use commands

---

*This file contains 70 advanced kubectl commands for Kubernetes experts.* 