# OpenShift MCP Server - SRE Edition

Welcome to the **OpenShift MCP Server** documentation — a native Go-based Model Context Protocol server enabling AI assistants to interact with Kubernetes and OpenShift clusters through natural language.

## 🎯 What is OpenShift MCP Server?

A **direct Kubernetes API client** (not a kubectl wrapper). The upstream server supports many optional toolsets; this demo repo ships a **sample SRE agent** with a curated subset for live cluster diagnostics, must-gather RCA, and network troubleshooting.

## 📚 Quick Navigation

| | |
|---|---|
| **Start here** | [Quick Start](quickstart.md) — build + `--toolsets` in Cursor |
| **MCP Server** | [Generic server setup](mcp-server/index.md) — CLI flags only |
| **Sample Agent** | [SRE Agent](sre-agent/index.md) — `--config` + `sre-agent.toml` |
| **Workflows** | [SRE Workflows](workflows/cluster-health.md) |
| **All toolsets** | [Toolsets Guide](reference/toolsets.md) |

## 🛠️ Sample agent toolsets

Configured in [`agents/sre/sre-agent.toml`](https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo/blob/main/agents/sre/sre-agent.toml):

| Toolset | Purpose |
|---------|---------|
| `core` | Pods, events, nodes, resources |
| `config` | Kubeconfig contexts, targets |
| `openshift` | Projects, OpenShift resources |
| `cluster-diagnostics` | Node debug, stats, logs |
| `openshift/mustgather` | Offline must-gather RCA |
| `cni-diagnostics` | conntrack, iptables, tcpdump, … |
| `ovn-kubernetes` | OVN trace, flows, ovs tools |

MCP prompts and RCA report rules live in `agents/sre/conf.d/`. Enable other toolsets in your own TOML — see [Toolsets Guide](reference/toolsets.md).

## 💡 Example prompts

**Cluster health**: "Show me cluster health — nodes, resource pressure, pods in error, namespace usage"

**Live RCA**: `/live-cluster-rca` (saves report to `agents/sre/reports/`)

**Must-gather**: `/must-gather-rca` with path to extracted bundle directory

**Network**: "Trace pod-to-service connectivity using OVN tools"

Full catalog: [Prompt Examples](sre-agent/prompt-examples.md)

## 📖 Documentation

- [Quick Start](quickstart.md) — build + `--toolsets` (no TOML)
- [MCP Server](mcp-server/index.md) — install, Cursor, flags
- [Sample Agent](sre-agent/index.md) — `--config`, prompts, RCA reports
- [SRE Workflows](workflows/cluster-health.md) — 7 workflow guides
- [Advanced Topics](advanced/must-gather.md) — must-gather, multi-cluster
- [Reference](reference/toolsets.md) — toolsets, config, FAQ

## 🔗 Resources

- **MCP Server source**: [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Demo repo**: [openshift-mcp-server-demo](https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo)
- **Upstream**: [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- **Protocol**: [modelcontextprotocol.io](https://modelcontextprotocol.io)

---

[→ Quick Start Guide](quickstart.md)
