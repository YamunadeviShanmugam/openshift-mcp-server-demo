# OpenShift MCP Server - SRE Edition

Welcome to the **OpenShift MCP Server** documentation - an AI-powered assistant for Site Reliability Engineers managing Kubernetes and OpenShift clusters.

## 🎯 What is This?

The OpenShift MCP Server is a **native Go-based Model Context Protocol server** that enables AI assistants (Claude, Cursor, etc.) to interact with your Kubernetes and OpenShift clusters through natural language. Unlike kubectl wrappers, this is a direct API client with enterprise-grade features.

### Key Capabilities for SREs

- **Cluster Diagnostics** - Real-time cluster health, node status, resource utilization
- **Troubleshooting** - Pod logs, events, resource analysis, and debugging
- **Security & Compliance** - RBAC audits, secret management, policy enforcement
- **Performance Analysis** - Resource metrics, bottleneck identification, optimization
- **Disaster Recovery** - Backup validation, cluster state verification, recovery procedures
- **Multi-Cluster Support** - Manage multiple OpenShift environments simultaneously
- **Observability** - Prometheus queries, Loki logs, Jaeger traces, OTEL integration

## 🚀 Quick Links

| Purpose | Link |
|---------|------|
| **New Users** | [Quick Start →](getting-started/quickstart.md) |
| **Installation** | [Installation Guide →](getting-started/installation.md) |
| **SRE Workflows** | [Cluster Health Monitoring →](workflows/cluster-health.md) |
| **Troubleshooting** | [Debug & Troubleshoot →](workflows/troubleshooting.md) |
| **Advanced Setup** | [Multi-Cluster Management →](advanced/multi-cluster.md) |

## 📊 Common SRE Workflows

### Cluster Health Monitoring
Monitor cluster health, node resources, and pod status with natural language queries.

```
"Show me nodes with high memory usage and any pods in CrashLoopBackOff state"
```

### Incident Response
Quickly diagnose and respond to incidents with comprehensive cluster analysis.

```
"My application deployment is failing - show me the pod status, recent events, and logs"
```

### Security Audits
Check RBAC configurations, secrets, and compliance with security policies.

```
"List all ServiceAccounts with cluster-admin role and audit the last 24 hours of changes"
```

### Performance Optimization
Identify bottlenecks and optimize resource allocation.

```
"Analyze which pods are using the most CPU and memory, and recommend resource limits"
```

## 🛠️ Why Choose OpenShift MCP Server?

| Feature | Benefit |
|---------|---------|
| **Native Implementation** | Direct API calls, no kubectl overhead |
| **No Dependencies** | Single binary, npm package, or container |
| **Multi-Cluster** | Manage multiple clusters from one interface |
| **Enterprise Ready** | TLS, OAuth, RBAC, read-only mode, resource restrictions |
| **AI-Powered** | Leverage Claude, ChatGPT, or any LLM for intelligent analysis |
| **Fast & Efficient** | Low latency, minimal resource usage |
| **Open Source** | MIT licensed, community-driven |

## 📖 Documentation Structure

```
📚 Documentation
├── Getting Started
│   ├── Quick Start (5 minutes)
│   ├── Installation (Multiple methods)
│   ├── Configuration
│   └── Cursor Integration
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
├── Advanced Topics
│   ├── Multi-Cluster Management
│   ├── Custom Toolsets
│   ├── API Reference
│   └── Security Best Practices
│
└── Reference
    ├── Toolsets Guide
    ├── Configuration Reference
    ├── Troubleshooting
    └── FAQ
```

## 🔧 Installation (30 seconds)

### 1. Choose Your Method

=== "npm (Recommended)"

    ```bash
    npx -y kubernetes-mcp-server@latest --read-only
    ```

=== "uvx (Python)"

    ```bash
    uvx kubernetes-mcp-server --read-only
    ```

=== "Native Binary"

    ```bash
    wget https://github.com/containers/kubernetes-mcp-server/releases/download/v1.0.0/kubernetes-mcp-server-linux-x86_64
    chmod +x kubernetes-mcp-server-linux-x86_64
    ./kubernetes-mcp-server-linux-x86_64 --read-only
    ```

=== "Docker"

    ```bash
    docker run -v ~/.kube/config:/kubeconfig:ro \
      -e KUBECONFIG=/kubeconfig \
      -p 8080:8080 \
      ghcr.io/containers/kubernetes-mcp-server:latest \
      --port 8080 --read-only
    ```

### 2. Add to Cursor

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "npx",
      "args": ["-y", "kubernetes-mcp-server@latest", "--read-only"]
    }
  }
}
```

### 3. Start Using

Ask Cursor: **"List all namespaces and show me any pods in error state"**

## ✨ Next Steps

- **New to OpenShift MCP?** Start with [Quick Start Guide](getting-started/quickstart.md)
- **Need to debug an issue?** Check [Troubleshooting Workflows](workflows/troubleshooting.md)
- **Managing multiple clusters?** See [Multi-Cluster Guide](advanced/multi-cluster.md)
- **Want production setup?** Review [Security Best Practices](advanced/security.md)

## 🤝 Community & Support

- **GitHub Issues**: [Report bugs or suggest features](https://github.com/containers/kubernetes-mcp-server/issues)
- **Discussions**: [Ask questions and share knowledge](https://github.com/containers/kubernetes-mcp-server/discussions)
- **Documentation**: [Official GitHub Repo](https://github.com/containers/kubernetes-mcp-server)

## 📝 License

This documentation and OpenShift MCP Server are licensed under the MIT License.

---

**Ready to get started?** → [Quick Start Guide](getting-started/quickstart.md)
