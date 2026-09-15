# Cursor Integration (SRE Agent)

Use Cursor with the **SRE agent** — `sre-agent.toml` + `conf.d/` loaded via `--config`.

## Prerequisites

- Cursor IDE
- Completed [Quick Start Steps 1–3](quickstart.md)
- `oc get nodes` works with your kubeconfig

## MCP configuration

Copy [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example) into `~/.cursor/mcp.json`:

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

| Loads from `sre-agent.toml` | Loads from `conf.d/` |
|-----------------------------|----------------------|
| Toolsets, `list_output`, security | `10-server-instructions.toml`, `20-prompts.toml` |

Restart Cursor. Open **openshift-mcp-server-demo** as the workspace.

## Verify

1. **Settings → MCP** — `openshift-mcp-server` connected
2. Chat: `List cluster namespaces using MCP`
3. Slash prompt: `/live-cluster-rca`
4. Logs: `tail -f /tmp/kubernetes-mcp-server.log`

## Example queries

**[Full prompt catalog →](../sre-agent/prompt-examples.md)**

### MCP slash prompts

```
/live-cluster-rca API 503 after worker node reboot
/live-etcd-analysis slow API responses
/live-component-rca ingress
/must-gather-rca /tmp/mg-extracted/.../registry-sha256-abc/
```

Reports save to `agents/sre/reports/`.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Help text then disconnect | Add `--kubeconfig` absolute path; check log file |
| MCP not connecting | Valid JSON; restart Cursor |
| Slash prompts missing | `--config` must point to `agents/sre/sre-agent.toml` |
| No report saved | Open demo repo as workspace |

## Next steps

- [Prompt Examples](../sre-agent/prompt-examples.md)
- [Quick Start](quickstart.md)
- [SRE Agent Setup](sre-agent.md)
