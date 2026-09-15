# Cursor Integration

Use Cursor with the **SRE agent demo** — follow the same single flow as the [Quick Start](quickstart.md).

## Prerequisites

- Cursor IDE
- Completed [Quick Start Steps 1–3](quickstart.md) (MCP server built, demo cloned, config applied)
- `kubectl get nodes` works

## MCP configuration

Copy [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example) into `~/.cursor/mcp.json` with your absolute paths:

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

Restart Cursor. Open **openshift-mcp-server-demo** as the workspace.

## Verify

1. **Settings → MCP** — `kubernetes-mcp-server` connected
2. Chat: `List cluster namespaces using MCP`
3. Optional: `tail -f /tmp/kubernetes-mcp-server.log`

## Example queries

### Health check

```
Give me a quick health check of my OpenShift cluster:
1. How many nodes and what's their status?
2. Are there nodes with memory or CPU pressure?
3. Which namespaces have the most pod activity?
4. Show me any pods in CrashLoopBackOff or Error states
```

### MCP prompts

```
/live-cluster-rca
/live-etcd-analysis
/must-gather-rca /path/to/extracted/must-gather-dir/
```

Reports save to `agents/sre/reports/`.

## Multi-cluster

If your kubeconfig has multiple contexts, specify the cluster in chat:

```
On the production context, run a health check
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| MCP not connecting | Valid JSON; restart Cursor; check log file |
| Permission denied | `kubectl auth can-i get pods -A` |
| No report saved | Open demo repo as workspace |
| Command not found | `command` must point to built `kubernetes-mcp-server` binary |

## Next steps

- [Quick Start](quickstart.md)
- [SRE Agent Setup](sre-agent.md)
- [Cluster Health Workflow](../workflows/cluster-health.md)
