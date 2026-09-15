# Cursor Integration (MCP Server)

Configure Cursor to run **kubernetes-mcp-server** in stdio mode.

## Prerequisites

- Cursor IDE
- Built binary or `npx kubernetes-mcp-server@latest`
- Working kubeconfig (`oc get nodes`)

## Option A — Built binary (recommended for OpenShift fork)

```json
{
  "mcpServers": {
    "openshift-mcp-server": {
      "command": "/ABSOLUTE/PATH/openshift-mcp-server/kubernetes-mcp-server",
      "args": [
        "--port",
        "",
        "--kubeconfig",
        "/ABSOLUTE/PATH/to/your/kubeconfig",
        "--toolsets",
        "core,config,openshift,cluster-diagnostics,openshift/mustgather,helm,cni-diagnostics,ovn-kubernetes",
        "--list-output",
        "yaml",
        "--log-file",
        "/tmp/kubernetes-mcp-server.log"
      ]
    }
  }
}
```

## Option B — npx (upstream release)

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "kubernetes-mcp-server@latest",
        "--port",
        "",
        "--kubeconfig",
        "/ABSOLUTE/PATH/to/your/kubeconfig",
        "--toolsets",
        "core,config,openshift",
        "--log-file",
        "/tmp/kubernetes-mcp-server.log"
      ]
    }
  }
}
```

!!! warning "Do not use `KUBECONFIG=~/.kube/config` in JSON"
    Tilde is not expanded in all environments. Prefer `--kubeconfig` with an absolute path.

## Required flags for Cursor

| Flag | Value | Purpose |
|------|-------|---------|
| `--port` | `""` (empty string) | stdio MCP transport |
| `--log-file` | e.g. `/tmp/kubernetes-mcp-server.log` | Logs must not go to stdout |
| `--kubeconfig` | absolute path | Cluster authentication |

## Verify

1. Restart Cursor after editing `~/.cursor/mcp.json`
2. **Settings → MCP** — `openshift-mcp-server` connected
3. Chat: `List namespaces using MCP`
4. Logs: `tail -f /tmp/kubernetes-mcp-server.log`

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Help text in MCP log, then disconnect | Process crashed on startup — read log file |
| `Connection closed` (-32000) | Usually bad kubeconfig path or invalid JSON in `mcp.json` |
| Permission errors | `oc auth can-i get pods -A` |
| Default toolsets only | Add `--toolsets` — see [Configuration](configuration.md) |

## Sample agent

For MCP prompts (`/live-cluster-rca`) and saved RCA reports, use the
**[SRE Agent](../sre-agent/index.md)** tab instead.

## Next steps

- [Configuration](configuration.md) — TOML, toolsets, security
- [Toolsets Reference](../reference/toolsets.md)
