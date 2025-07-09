# 🚀 Ultimate Kubectl Commands Mastery
## Complete Kubernetes CLI Reference Guide

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Stars](https://img.shields.io/github/stars/yourusername/kubectl-commands-mastery)](https://github.com/yourusername/kubectl-commands-mastery)
[![Forks](https://img.shields.io/github/forks/yourusername/kubectl-commands-mastery)](https://github.com/yourusername/kubectl-commands-mastery)

> **The most comprehensive kubectl commands reference with 300+ commands from beginner to expert level**

---

## 📚 Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Command Categories](#command-categories)
- [Usage Examples](#usage-examples)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

This repository contains **300+ kubectl commands** organized by complexity and use case. Whether you're a Kubernetes beginner or an expert, you'll find commands for every scenario:

- 🔰 **Beginner**: Basic operations and troubleshooting
- 🔧 **Intermediate**: Advanced resource management and debugging
- ⚡ **Advanced**: API server interactions and custom operations
- 🎯 **Expert**: Direct etcd access and cluster administration
- 🚀 **One-Liners**: Ready-to-use complex commands

---

## 🚀 Quick Start

### Prerequisites
- Kubernetes cluster access
- kubectl installed and configured
- Basic understanding of Kubernetes concepts

### Installation
```bash
# Clone the repository
git clone https://github.com/yourusername/kubectl-commands-mastery.git

# Navigate to the directory
cd kubectl-commands-mastery

# Start with basic commands
cat 01-basic-commands.md
```

---

## 📁 Command Categories

### 1. [Basic Commands](01-basic-commands.md) (50 commands)
Essential commands for daily Kubernetes operations:
- Cluster information and discovery
- Basic resource management
- Pod and service operations
- Simple troubleshooting

### 2. [Intermediate Commands](02-intermediate-commands.md) (60 commands)
Advanced operations for power users:
- Advanced selectors and filtering
- Resource optimization
- Multi-resource operations
- Port forwarding and proxy

### 3. [Advanced Commands](03-advanced-commands.md) (70 commands)
Expert-level operations:
- API server interactions
- Custom resource definitions
- Advanced debugging
- Performance analysis

### 4. [Expert Commands](04-expert-commands.md) (60 commands)
Master-level cluster administration:
- Direct etcd access
- Scheduler debugging
- Security auditing
- Cluster diagnostics

### 5. [One-Liners & Scripts](05-one-liners-scripts.md) (60 commands)
Ready-to-use complex commands:
- Quick diagnostics
- Automation scripts
- Advanced one-liners
- Troubleshooting scripts

---

## 💡 Usage Examples

### Quick Start Examples
```bash
# Check cluster status
kubectl cluster-info

# List all pods
kubectl get pods

# Get pod details
kubectl describe pod <pod-name>

# View logs
kubectl logs <pod-name>

# Execute command in pod
kubectl exec -it <pod-name> -- /bin/bash
```

---

## 🎯 Command Structure

Each command includes:
- **Command**: The actual kubectl command
- **Use Case**: When to use this command
- **Example**: Practical example with output
- **Notes**: Additional tips and warnings

### Command Format
Each command in the files follows this format:
```bash
# Command description
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
# Use: Get pods with custom column format
# Example: Lists pods with only name and status columns
# Notes: Useful for scripting and automation
```

---

## 🔧 Prerequisites

### Required Tools
- **kubectl**: Kubernetes command-line tool
- **jq**: JSON processor for advanced output parsing (optional)
- **etcdctl**: For direct etcd access (optional)

### Installation
```bash
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install jq (optional)
sudo apt-get install jq
```

---

## 🚀 Advanced Usage

### Quick Aliases
Add these to your `.bashrc` or `.zshrc`:
```bash
alias k='kubectl'
alias kg='kubectl get'
alias kd='kubectl describe'
alias kl='kubectl logs'
alias ke='kubectl exec -it'
```

---

## 🔍 Troubleshooting

### Common Issues
1. **Permission Denied**: Check RBAC permissions
2. **Connection Refused**: Verify cluster connectivity
3. **Resource Not Found**: Check namespace and resource names
4. **Authentication Failed**: Verify kubeconfig

### Quick Debug
```bash
# Check cluster connectivity
kubectl cluster-info

# Verify authentication
kubectl auth can-i get pods

# Check API server health
kubectl get --raw /healthz
```

---

## 🤝 Contributing

This is a community-driven cheat sheet. Feel free to:
- Add new commands with clear use cases
- Improve existing commands
- Fix any errors or typos
- Suggest better organization

---

## 📊 Statistics

- **Total Commands**: 300+
- **Categories**: 5
- **Complexity Levels**: 4
- **Use Cases**: 50+
- **Examples**: 100+

---

## 🔍 Finding Commands

Use `grep` to find specific commands:
```bash
# Find all commands related to debugging
grep -r "debug" . --include="*.md"

# Find commands for a specific resource
grep -r "pods" . --include="*.md"
```

---

## 📚 Additional Resources

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [kubectl Reference](https://kubernetes.io/docs/reference/kubectl/)
- [k9s](https://k9scli.io/) - Terminal-based UI
- [kubectx](https://github.com/ahmetb/kubectx) - Context switching

---

## 🙏 Acknowledgments

- Kubernetes community for the amazing platform
- All contributors who share their knowledge

---

**Made with ❤️ by the Kubernetes community**

---

*Last updated: December 2024* 