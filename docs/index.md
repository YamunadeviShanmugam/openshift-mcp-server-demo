# OpenShift MCP Server - SRE Edition

Welcome to the **OpenShift MCP Server** documentation - a native Go-based Model Context Protocol server enabling AI assistants to interact with Kubernetes and OpenShift clusters through natural language.

## 🎯 What is OpenShift MCP Server?

A **direct Kubernetes API client** (not a kubectl wrapper) with **18+ specialized toolsets** for advanced cluster operations.

## 📚 Quick Navigation

| | |
|---|---|
| **New Users** | [Quick Start (5 min)](getting-started/quickstart.md) |
| **Install** | [Installation Guide](getting-started/installation.md) |
| **Config** | [Configuration](getting-started/configuration.md) |
| **Workflows** | [SRE Workflows](workflows/cluster-health.md) |

## 🛠️ 18+ Available Toolsets

### Core & Diagnostics
`core` • `cluster-diagnostics` • `openshift`

### Virtualization & Compute
`kubevirt` • `vm_troubleshoot` • `helm` • `tekton`

### Networking
`cni-diagnostics` • `ovn-kubernetes` • `netedge` • `netobserv`

### Service Mesh & Multi-tenancy
`ossm` • `kcp`

### Observability & Monitoring
`observability/metrics` • `observability/logs` • `observability/traces` • `observability/otelcol`

### Backup & Recovery
`oadp`

## 🚀 30-Second Setup

```bash
# Install
npx -y openshift-mcp-server@latest

# Add to Cursor (~/.cursor/mcp.json)
{
  "openshift-mcp-server": {
    "command": "npx",
    "args": ["-y", "openshift-mcp-server@latest"],
    "env": {"KUBECONFIG": "~/.kube/config"}
  }
}

# Use in Cursor
"Show me cluster health and any pods in error state"
```

## 💡 Example Workflows

**Cluster Health**: "How is my cluster? Show nodes, pod status, and resource usage"

**VM Troubleshooting**: "Why is my VM stuck in Provisioning? How do I fix it?"

**Network Diagnostics**: "Diagnose connectivity issues between namespaces"

**Observability**: "Show me Prometheus metrics and Loki logs for errors"

**Backup**: "Create a backup of my database namespace using OADP"

## 📖 Documentation

- [Getting Started](getting-started/quickstart.md) - Quick Start & Installation
- [SRE Workflows](workflows/cluster-health.md) - 7 workflow guides
- [Custom Tools Support](advanced/multi-cluster.md) - Multi-cluster, API, Security
- [Reference](reference/toolsets.md) - Toolsets, Config, Troubleshooting

## 🔗 Resources

- **Repository**: [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Docs**: [GitHub Docs](https://github.com/openshift/openshift-mcp-server/tree/main/docs)
- **Issues**: [GitHub Issues](https://github.com/openshift/openshift-mcp-server/issues)

---

[→ Quick Start Guide](getting-started/quickstart.md)
