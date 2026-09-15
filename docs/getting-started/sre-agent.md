# SRE Agent Setup

Supplements the **[Quick Start](quickstart.md)** — follow that flow first.

## One demo flow

| Step | Action |
|------|--------|
| **1** | `git clone https://github.com/openshift/openshift-mcp-server.git` → `make build` |
| **2** | `git clone` this demo repo |
| **3** | Apply [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example) to `~/.cursor/mcp.json` |

## MCP config + `sre-agent.toml`

[`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example):

```json
{
  "mcpServers": {
    "openshift-mcp-server": {
      "command": "/ABSOLUTE/PATH/openshift-mcp-server/kubernetes-mcp-server",
      "args": [
        "--port", "",
        "--kubeconfig", "/ABSOLUTE/PATH/to/your/kubeconfig",
        "--config", "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml",
        "--log-file", "/tmp/kubernetes-mcp-server.log"
      ],
      "env": { "KUBECONFIG": "/ABSOLUTE/PATH/to/your/kubeconfig" }
    }
  }
}
```

!!! warning "Always set `--kubeconfig`"
    Use an **absolute path**. The CLI flag overrides any path in TOML.

## What's in `agents/sre/`

| Path | Purpose |
|------|---------|
| `sre-agent.toml` | Toolsets, denied resources, loads `conf.d/` |
| `conf.d/10-server-instructions.toml` | Mandatory RCA report output |
| `conf.d/20-prompts.toml` | MCP prompts (`/live-cluster-rca`, …) |
| `conf.d/99-local.toml.example` | Optional personal overrides (gitignored) |
| `reports/` | Saved RCA files |

### Toolsets in `sre-agent.toml`

```
core, config, openshift, cluster-diagnostics, openshift/mustgather, cni-diagnostics, ovn-kubernetes
```

Edit `sre-agent.toml` to add/remove toolsets — no need for `--toolsets` in `mcp.json`.

## Next steps

- [Prompt Examples](../sre-agent/prompt-examples.md)
- [Cluster Health Workflow](../workflows/cluster-health.md)
- [Must-Gather Analysis](../advanced/must-gather.md)
