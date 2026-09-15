# Configuration Reference

This page replaces the outdated `config.yaml` examples. Use the guides below instead.

## MCP server configuration

**[MCP Server — Configuration](../mcp-server/configuration.md)**

Covers:

- CLI flags in `~/.cursor/mcp.json` (`--port`, `--kubeconfig`, `--toolsets`, …)
- TOML config files and drop-in directories
- Kubeconfig path rules (use absolute paths; `--kubeconfig` wins over TOML)
- Environment variables (`KUBECONFIG`, `K8S_MCP_CONFIG_PATH`)

Upstream full reference:
[containers/kubernetes-mcp-server — configuration.md](https://github.com/containers/kubernetes-mcp-server/blob/main/docs/configuration.md)

## SRE agent configuration

**[SRE Agent Setup](../getting-started/sre-agent.md)**

| File | Purpose |
|------|---------|
| [`agents/sre/sre-agent.toml`](../../agents/sre/sre-agent.toml) | Toolsets, security, loads `conf.d/` |
| [`agents/sre/conf.d/10-server-instructions.toml`](../../agents/sre/conf.d/10-server-instructions.toml) | RCA report rules |
| [`agents/sre/conf.d/20-prompts.toml`](../../agents/sre/conf.d/20-prompts.toml) | MCP slash prompts |
| [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example) | Cursor `mcp.json` template |

### Example Cursor config (SRE agent)

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
      ]
    }
  }
}
```

### Toolsets in `sre-agent.toml`

```
core, config, openshift, cluster-diagnostics, openshift/mustgather, cni-diagnostics, ovn-kubernetes
```

Edit `sre-agent.toml` to change toolsets — do not duplicate in `--toolsets` when using `--config`.

## Toolsets reference

**[Toolsets Guide](toolsets.md)**
