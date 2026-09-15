# OpenShift MCP Server - SRE Edition

**AI-Powered Kubernetes/OpenShift Operations for Site Reliability Engineers**

An intelligent, native Go-based Model Context Protocol server that enables AI assistants (Claude, Cursor, ChatGPT) to interact with Kubernetes and OpenShift clusters through natural language.

## 🎯 What's This?

This repository contains comprehensive SRE-focused documentation and best practices for using the OpenShift MCP Server to manage production Kubernetes/OpenShift clusters with AI assistance.

## 📚 Documentation Structure

```
📖 SRE Edition Documentation
├── Getting Started (5-30 minutes setup)
│   ├── Quick Start
│   ├── Installation Guide
│   ├── Configuration Guide
│   └── Cursor Integration
│
├── SRE Workflows (Tab-Based)
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

## 🚀 Quick Start

### 1. Install (30 seconds)

```bash
npx -y kubernetes-mcp-server@latest --read-only
```

### 2. Configure (1 minute)

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

### 3. Use (Immediately)

In Cursor chat:
```
"List all pods in my cluster and show me any that are in error state"
```

## 📖 Documentation

- **Getting Started**: [Quick Start Guide](docs/getting-started/quickstart.md)
- **SRE Workflows**:
  - [Cluster Health Monitoring](docs/workflows/cluster-health.md)
  - [Troubleshooting & Debugging](docs/workflows/troubleshooting.md)
  - [Security & Compliance](docs/workflows/security-compliance.md)
  - [Performance Tuning](docs/workflows/performance-tuning.md)
  - [Disaster Recovery](docs/workflows/disaster-recovery.md)
  - [Incident Response](docs/workflows/incident-response.md)

## 🛠️ Features

### Multi-Cluster Support
- Manage multiple Kubernetes/OpenShift clusters simultaneously
- Context switching in natural language
- Unified dashboard view across clusters

### Comprehensive Toolsets
- **Core**: Pods, namespaces, events, nodes
- **Helm**: Chart management and releases
- **Tekton**: Pipeline automation
- **OpenShift**: OpenShift-specific tools
- **Observability**: Prometheus, Loki, Jaeger integration
- **Security**: RBAC audit, policy enforcement

### Enterprise-Ready
- Read-only mode (prevents accidental changes)
- TLS/HTTPS support
- OAuth/OIDC authentication
- Resource access control
- Rate limiting
- Comprehensive audit logging

## 📋 Common SRE Tasks

### Quick Health Check
```
"Give me a health check: nodes ready, pod status, recent errors"
```

### Incident Response
```
"Service is down - quick diagnosis: pod status, logs, events, recent changes"
```

### Capacity Planning
```
"Analyze cluster capacity: utilization, growth, upgrade needs"
```

### Security Audit
```
"RBAC audit: show cluster-admin users, privileged pods, risky configs"
```

### Performance Optimization
```
"Performance analysis: CPU/memory usage, throttling, recommendations"
```

## 🔒 Security

### Always Enabled
- ✅ Read-only mode by default
- ✅ No credentials stored
- ✅ kubeconfig from local filesystem
- ✅ Native implementation (no kubectl wrapper)

### Optional
- 🔐 TLS/HTTPS encryption
- 🔐 OAuth/OIDC authentication
- 🔐 Resource access restrictions
- 🔐 Audit logging

## 📊 Installation Methods

| Method | Best For | Setup Time |
|--------|----------|-----------|
| npm | Most users | 30 seconds |
| uvx | Python users | 1 minute |
| Binary | Production | 2 minutes |
| Docker | CI/CD | 5 minutes |

See [Installation Guide](docs/getting-started/installation.md) for details.

## 🎓 Learning Path

1. **Start**: [Quick Start (5 min)](docs/getting-started/quickstart.md)
2. **Configure**: [Configuration Guide (5 min)](docs/getting-started/configuration.md)
3. **Learn**: [Choose SRE Workflow](#-sre-workflows-tab-based)
4. **Advanced**: [Multi-Cluster Guide](docs/advanced/multi-cluster.md)

## 🔗 Related Projects

- **Official Repository**: [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- **Workshop MCP Server**: [gangwgr/workshop-mcp-server](https://github.com/gangwgr/workshop-mcp-server)
- **Model Context Protocol**: [modelcontextprotocol.io](https://modelcontextprotocol.io)

## 💡 Pro Tips

- Always use `--read-only` in production
- Test backups regularly (not just create them)
- Monitor trends, not just current state
- Document your SLAs and RTO/RPO targets
- Share useful prompts with your team
- Run security audits monthly

## 🤝 Contributing

We welcome contributions! Please:

1. Fork this repository
2. Create a feature branch
3. Add documentation or improvements
4. Submit a pull request

## 📝 License

This documentation is available under the MIT License.

## 🆘 Support

- **Questions?** Open an issue
- **Found a bug?** Report it
- **Want to contribute?** Submit a PR
- **Need help?** Check the [FAQ](docs/reference/faq.md)

---

## 🌟 SRE Workflows at a Glance

### Cluster Health Monitoring
Real-time cluster health, resource utilization, event analysis

### Troubleshooting & Debugging
Pod crashes, node issues, network problems, performance degradation

### Security & Compliance
RBAC audits, secret management, policy enforcement, vulnerability scans

### Performance Tuning
Resource optimization, bottleneck identification, scaling recommendations

### Disaster Recovery
Backup verification, recovery procedures, RTO/RPO calculation

### Incident Response
Rapid detection, diagnosis, mitigation, and post-mortems

---

**Get Started Now**: [Quick Start Guide →](docs/getting-started/quickstart.md)

**Built with ❤️ for SREs**
