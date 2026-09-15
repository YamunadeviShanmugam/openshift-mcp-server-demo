# OpenShift MCP Server - SRE Edition

**AI-Powered Kubernetes/OpenShift Operations for Site Reliability Engineers**

An intelligent, native Go-based Model Context Protocol server that enables AI assistants (Claude, Cursor, ChatGPT) to interact with Kubernetes and OpenShift clusters through natural language.

## 🎯 What's This?

This repository contains comprehensive SRE-focused documentation and best practices for using the OpenShift MCP Server to manage production Kubernetes/OpenShift clusters with AI assistance.

## 📚 Documentation Structure

Published docs use **two tabs** (MkDocs):

| Tab | Contents |
|-----|----------|
| **MCP Server** | Build, install, Cursor stdio, flags/TOML config |
| **SRE Agent** | Team demo, prompts, RCA reports, workflows |

```
📖 SRE Edition Documentation
├── docs/mcp-server/             ← Generic kubernetes-mcp-server setup
├── docs/sre-agent/              ← SRE agent overview
├── agents/sre/                  ← MCP config, prompts, reports (runtime files)
├── SRE Workflows
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

## 🚀 Quick Start (one flow)

```bash
# 1. MCP server
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build

# 2. Demo repo
cd ..
git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cd openshift-mcp-server-demo
```

**3. Apply config** — copy paths from [`agents/sre/mcp.json.example`](agents/sre/mcp.json.example) into `~/.cursor/mcp.json`, restart Cursor, open this repo as workspace.

Full steps: **[Quick Start Guide](docs/getting-started/quickstart.md)**

In Cursor chat: `/live-cluster-rca` → report in `agents/sre/reports/`

## 📖 Documentation

- **MCP Server tab**: [Overview](docs/mcp-server/index.md) — build, Cursor, CLI configuration
- **SRE Agent tab**: [Overview](docs/sre-agent/index.md) — team demo, prompts, RCA reports
- **Prompt Examples**: [Copy-paste catalog](docs/sre-agent/prompt-examples.md)
- **Quick Start**: [SRE Agent Quick Start](docs/getting-started/quickstart.md)
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

### Comprehensive Toolsets (SRE agent)
- **Core**: Pods, namespaces, events, nodes
- **OpenShift**: OpenShift-specific tools
- **Cluster diagnostics**: Node debugging and stats
- **Must-gather**: Offline bundle analysis
- **CNI / OVN**: Network diagnostics

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

See [MCP Server Installation](docs/mcp-server/installation.md) for details.

## 🎓 Learning Path

1. **Start**: [Quick Start — one flow](docs/getting-started/quickstart.md) — clone MCP server → clone demo → apply config
2. **Learn**: [SRE Workflows](#-sre-workflows-at-a-glance)
3. **Advanced**: [Must-Gather Analysis](docs/advanced/must-gather.md)

## 🔗 Related Projects

- **OpenShift fork (build source):** [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server) — `make build` → `kubernetes-mcp-server`
- **Upstream:** [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- **Build guide:** [Build from Source](docs/getting-started/build-from-source.md)
- **Model Context Protocol:** [modelcontextprotocol.io](https://modelcontextprotocol.io)

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
