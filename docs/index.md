# OpenShift MCP Server - SRE Edition

Welcome to the **OpenShift MCP Server** documentation — AI-assisted Kubernetes and OpenShift
operations for Site Reliability Engineers, including the bundled **SRE agent** (live RCA,
must-gather analysis, formatted reports).

## 🎯 What is this repo?

1. **SRE agent demo** — `agents/sre/` config, MCP prompts, and saved RCA reports
2. **Documentation** — setup guides, SRE workflows, and reference material

The MCP server binary is built from [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server).
This demo repo supplies the SRE config and report templates.

## 📚 Quick Navigation

| | |
|---|---|
| **Start here** | [Quick Start — one flow](getting-started/quickstart.md) |
| **SRE Agent** | [SRE Agent Setup](getting-started/sre-agent.md) |
| **Build details** | [Build from Source](getting-started/build-from-source.md) |
| **Cursor** | [Cursor Integration](getting-started/cursor-integration.md) |
| **Workflows** | [SRE Workflows](workflows/cluster-health.md) |

## 🚀 Demo setup (3 steps)

```bash
# 1. Clone and build MCP server
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build

# 2. Clone demo
cd .. && git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cd openshift-mcp-server-demo
```

**3.** Apply [`agents/sre/mcp.json.example`](../agents/sre/mcp.json.example) → `~/.cursor/mcp.json`  
Open this repo in Cursor → `/live-cluster-rca`

[Full Quick Start →](getting-started/quickstart.md)

## 💡 Example prompts

```
/live-cluster-rca
```

```
Give me a quick health check: nodes, pressure, top namespaces, CrashLoopBackOff pods
```

## 📖 Documentation

- [Quick Start](getting-started/quickstart.md) — clone → build → config
- [SRE Workflows](workflows/cluster-health.md)
- [Must-Gather Analysis](advanced/must-gather.md)

## 🔗 Resources

- **MCP server:** [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Demo repo:** [openshift-mcp-server-demo](https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo)

---

**New team members:** [Team Onboarding](../TEAM_ONBOARDING.md)
