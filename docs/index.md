# OpenShift MCP Server - SRE Edition

Welcome to the **OpenShift MCP Server** documentation — a native Go-based Model Context Protocol server enabling AI assistants to interact with Kubernetes and OpenShift clusters through natural language.

## 🎯 What is OpenShift MCP Server?

A **direct Kubernetes API client** (not a kubectl wrapper). The upstream server supports many optional toolsets; this demo repo ships a **sample SRE agent** with a curated subset for live cluster diagnostics, must-gather RCA, and network troubleshooting.

Use the tabs above:

| Tab | Use when |
|-----|----------|
| **Home** (this page) | Overview and navigation |
| **[Quick Start](quickstart.md)** | Build MCP server and connect in Cursor |
| **[SRE Workflows](workflows/cluster-health.md)** | Day-to-day SRE playbooks |
| **[SRE Agent](sre-agent/index.md)** | Sample agent, prompts, RCA reports |
| **[Advanced Topics](advanced/must-gather.md)** | Must-gather, multi-cluster, reference |

## 📖 Documentation

Published docs use **six tabs** (MkDocs Material):

| Tab | Contents |
|-----|----------|
| **Home** | This page |
| **Quick Start** | [Setup](quickstart.md) — build and connect in Cursor |
| **SRE Workflows** | [Playbooks](workflows/cluster-health.md) |
| **SRE Agent** | [Sample agent](sre-agent/index.md), prompt examples |
| **Advanced Topics** | Must-gather, multi-cluster, [reference](reference/toolsets.md) |

## 🔗 Resources

- **MCP Server source**: [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Demo repo**: [openshift-mcp-server-demo](https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo)
- **Upstream**: [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- **Protocol**: [modelcontextprotocol.io](https://modelcontextprotocol.io)

---

[→ Quick Start Guide](quickstart.md)
