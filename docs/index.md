# OpenShift MCP Server - SRE Edition

Welcome to the **OpenShift MCP Server** documentation - a native Go-based Model Context Protocol server that enables AI assistants (Claude, Cursor, etc.) to interact with your Kubernetes and OpenShift clusters through natural language.

## 🎯 What is OpenShift MCP Server?

The OpenShift MCP Server is a **direct Kubernetes API client** - NOT a kubectl wrapper. It provides 18+ specialized toolsets for advanced cluster operations, enabling SREs to leverage AI for:

- **Advanced Diagnostics** - Automated root-cause detection and troubleshooting
- **Multi-Cluster Management** - Seamless operations across multiple clusters
- **OpenShift Virtualization** - VM management and automated troubleshooting
- **Service Mesh Operations** - OSSM (OpenShift Service Mesh) management
- **Network Troubleshooting** - CNI, OVN, and NetObserv integration
- **Observability Stack** - Prometheus, Loki, Tempo, and OpenTelemetry
- **Backup Automation** - OADP/Velero backup and restore operations
- **Security & Compliance** - RBAC audits, policy enforcement, secrets management
- **Performance Analysis** - Resource metrics, bottleneck identification
- **CI/CD Integration** - Tekton pipeline management and monitoring

## 🚀 18+ Available Toolsets

| Toolset | Purpose | Status |
|---------|---------|--------|
| **core** | Pod, namespace, resource management | ✓ Default |
| **cluster-diagnostics** | Advanced cluster health & diagnostics | Optional |
| **openshift** | OpenShift-specific operations | Optional |
| **kubevirt** | OpenShift Virtualization / VM management | Optional |
| **vm_troubleshoot** | Automated VM issue detection & fixes | Optional |
| **ossm** | OpenShift Service Mesh (Istio) | Optional |
| **cni-diagnostics** | Container Network Interface diagnostics | Optional |
| **ovn-kubernetes** | OVN network troubleshooting | Optional |
| **netedge** | Edge network diagnostics | Optional |
| **netobserv** | Network observability & flows | Optional |
| **observability/metrics** | Prometheus queries & metrics | Optional |
| **observability/logs** | Loki log queries | Optional |
| **observability/traces** | Distributed tracing (Tempo) | Optional |
| **observability/otelcol** | OpenTelemetry Collector config | Optional |
| **oadp** | Velero backup/restore automation | Optional |
| **helm** | Helm chart management | Optional |
| **tekton** | Tekton pipeline management | Optional |
| **kcp** | KCP workspaces & multi-tenancy | Optional |

## 📊 Common SRE Workflows

### 1. Cluster Health & Diagnostics

```
"Show me cluster health status, node resource usage, and any pods in error states"
```

Available toolsets: `cluster-diagnostics`, `core`, `observability/metrics`

### 2. VM Troubleshooting (OpenShift Virtualization)

```
"Why is my VM stuck in Provisioning state? Show me the issue and how to fix it"
```

Available toolsets: `kubevirt`, `vm_troubleshoot`

### 3. Service Mesh Operations

```
"Deploy a VirtualService with traffic routing for my application"
```

Available toolsets: `ossm`, `openshift`

### 4. Network Diagnostics

```
"Diagnose network connectivity issues between pods in different namespaces"
```

Available toolsets: `cni-diagnostics`, `ovn-kubernetes`, `netobserv`

### 5. Observability & Monitoring

```
"Query Prometheus for CPU usage, show Loki logs for errors, and trace requests through Tempo"
```

Available toolsets: `observability/metrics`, `observability/logs`, `observability/traces`

### 6. Backup & Disaster Recovery

```
"Create a backup of my database namespace using OADP and verify it's restorable"
```

Available toolsets: `oadp`, `core`

### 7. CI/CD Pipeline Management

```
"Show me all Tekton pipelines and their recent runs"
```

Available toolsets: `tekton`

## 🛠️ Why Choose OpenShift MCP Server?

| Feature | OpenShift MCP | kubectl wrapper |
|---------|---------------|-----------------|
| Native Implementation | ✓ Direct Kubernetes API | ✗ Shell commands |
| Toolsets | ✓ 18+ specialized | ✗ Limited |
| Multi-Cluster | ✓ Built-in | ✗ Context switching |
| VM Management | ✓ Full KubeVirt support | ✗ Not available |
| Service Mesh | ✓ OSSM integration | ✗ Not available |
| Network Tools | ✓ Advanced diagnostics | ✗ Basic only |
| Observability | ✓ Full stack (Prometheus, Loki, Tempo, OTEL) | ✗ Partial |
| Backup Automation | ✓ OADP integration | ✗ Not available |
| No Dependencies | ✓ Single binary | ✗ Requires kubectl, helm, etc. |
| Performance | ✓ Direct API calls | ✗ Shell overhead |

## 📖 Documentation Structure

```
📚 Documentation
├── Getting Started
│   ├── Quick Start (5 minutes)
│   ├── Installation (Multiple methods)
│   ├── Configuration (Toolsets & features)
│   └── Cursor Integration (IDE setup)
│
├── SRE Workflows (Tab-based)
│   ├── Cluster Health Monitoring
│   ├── Troubleshooting & Debugging
│   ├── Security & Compliance
│   ├── Performance Tuning
│   ├── Disaster Recovery
│   ├── Observability Setup
│   └── Incident Response
│
├── Custom Tools Support
│   ├── Multi-Cluster Management
│   ├── Custom Toolsets
│   ├── API Reference
│   └── Security Best Practices
│
└── Reference
    ├── Toolsets Guide (All 18+ toolsets)
    ├── Configuration Reference
    ├── Troubleshooting
    └── FAQ
```

## 🔧 Installation (30 seconds)

### 1. Choose Your Method

=== "npm (Recommended)"

```bash
npx -y openshift-mcp-server@latest --toolsets core,openshift,cluster-diagnostics,helm,oadp,kubevirt,observability/metrics,observability/logs
```

=== "Native Binary"

```bash
wget https://github.com/openshift/openshift-mcp-server/releases/download/v1.0.0/openshift-mcp-server-linux-x86_64
chmod +x openshift-mcp-server-linux-x86_64
./openshift-mcp-server-linux-x86_64 --toolsets core,openshift,cluster-diagnostics
```

=== "Docker"

```bash
docker run -v ~/.kube/config:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  -p 8080:8080 \
  ghcr.io/openshift/openshift-mcp-server:latest \
  --port 8080 --toolsets core,openshift
```

### 2. Add to Cursor

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "openshift-mcp-server": {
      "command": "npx",
      "args": ["-y", "openshift-mcp-server@latest"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      }
    }
  }
}
```

### 3. Start Using

Ask Cursor: **"Show me cluster health and any pods in error state"**

Or: **"Troubleshoot why my VM is stuck in Provisioning state"**

## ✨ Next Steps

* **New to OpenShift MCP?** Start with [Quick Start Guide](getting-started/quickstart.md)
* **Need to debug?** Check [Troubleshooting Workflows](workflows/troubleshooting.md)
* **Manage VMs?** See [Virtualization Guide](custom-tools-support/kubevirt-guide.md)
* **Monitor cluster?** Review [Observability Setup](workflows/observability.md)
* **Production setup?** Read [Security Best Practices](custom-tools-support/security.md)

## 📚 Key Resources

- **Official Repository**: [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Full Documentation**: [GitHub Docs](https://github.com/openshift/openshift-mcp-server/tree/main/docs)
- **Kubernetes MCP Base**: [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- **Community & Support**: [GitHub Issues](https://github.com/openshift/openshift-mcp-server/issues)

## 🤝 Community & Support

* **GitHub Issues**: Report bugs or suggest features
* **Discussions**: Ask questions and share knowledge
* **Documentation**: Official GitHub Repo
* **Community**: Active contributors and SREs

## 📝 License

This documentation and OpenShift MCP Server are licensed under the Apache License 2.0.

---

**Ready to get started?** → [Quick Start Guide](getting-started/quickstart.md)
