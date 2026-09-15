# SRE Agent Setup

This page supplements the **[Quick Start](quickstart.md)** — follow that single flow first.

## One demo flow

| Step | Action |
|------|--------|
| **1** | `git clone https://github.com/openshift/openshift-mcp-server.git` → `make build` |
| **2** | `git clone` this demo repo |
| **3** | Apply [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example) to `~/.cursor/mcp.json` |

Open **openshift-mcp-server-demo** in Cursor. Run `/live-cluster-rca`.

## MCP config template

[`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example):

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "/ABSOLUTE/PATH/openshift-mcp-server/kubernetes-mcp-server",
      "args": [
        "--port", "",
        "--config", "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml",
        "--log-file", "/tmp/kubernetes-mcp-server.log"
      ],
      "env": { "KUBECONFIG": "/Users/YOU/.kube/config" }
    }
  }
}
```

## What's in `agents/sre/`

| Path | Purpose |
|------|---------|
| `sre-agent.toml` | Toolsets, denied resources, loads `conf.d/` |
| `conf.d/10-server-instructions.toml` | Mandatory RCA report output |
| `conf.d/20-prompts.toml` | MCP prompts |
| `conf.d/00-local.toml` | Default kubeconfig |
| `reports/` | Saved RCA files |

## Demo script (presentations)

| # | Show |
|---|------|
| 1 | `make build` → `kubernetes-mcp-server` binary |
| 2 | `agents/sre/` tree + `mcp.json.example` |
| 3 | Cursor MCP connected |
| 4 | `/live-cluster-rca` → new file in `agents/sre/reports/` |

## Health-check prompt

```
Give me a quick health check of my OpenShift cluster:
1. How many nodes and what's their status?
2. Are there nodes with memory or CPU pressure?
3. Which namespaces have the most pod activity?
4. Show me any pods in CrashLoopBackOff or Error states
```

## Must-gather (offline)

```bash
tar -xzf must-gather*.tar.gz -C /tmp/mg-extracted/
```

```
/must-gather-rca /tmp/mg-extracted/.../registry-sha-dir/
```

## Next steps

- [Build from Source](build-from-source.md) — multi-platform build, mcp-inspector
- [Cluster Health Workflow](../workflows/cluster-health.md)
- [Must-Gather Analysis](../advanced/must-gather.md)
