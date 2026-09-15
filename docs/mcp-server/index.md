# Kubernetes / OpenShift MCP Server

Generic setup for **[kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)**
(the binary built from [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)).

Use this tab when you want **cluster tools only** — no SRE prompts, no mandatory RCA reports.

!!! tip "Need the SRE agent demo?"
    Switch to the **[SRE Agent](../sre-agent/index.md)** tab for prompts, report templates,
    and the team onboarding flow.

## What you get

- Native Go MCP server (not a `kubectl` wrapper)
- Toolsets selected via `--toolsets` or TOML
- stdio mode for Cursor (`--port ""`)
- Optional TOML for advanced settings (denied resources, prompts)

## Quick setup (Cursor)

Configure toolsets and kubeconfig via CLI flags in `~/.cursor/mcp.json`:

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build
```

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
        "core,config,openshift,cluster-diagnostics,openshift/mustgather,cni-diagnostics,ovn-kubernetes",
        "--list-output",
        "yaml",
        "--log-file",
        "/tmp/kubernetes-mcp-server.log"
      ]
    }
  }
}
```

| Requirement | Why |
|-------------|-----|
| `--port ""` | stdio transport for Cursor |
| `--kubeconfig` with **absolute path** | Required when kubeconfig is not `~/.kube/config` |
| `--log-file` | Required in stdio mode (keeps logs off stdout) |

Restart Cursor. Verify: **Settings → MCP** → connected.

## Configuration options

| Approach | Best for | Docs |
|----------|----------|------|
| **CLI flags** | Quick start, minimal config | [Configuration](configuration.md#cli-flags) |
| **TOML file** | Denied resources, prompts, drop-ins | [Configuration](configuration.md#toml-configuration) |
| **Built binary** | Demos, unreleased toolsets | [Build from Source](build-from-source.md) |
| **npx / releases** | Fastest install | [Installation](installation.md) |

## Common issues

| Symptom | Fix |
|---------|-----|
| MCP shows help text then disconnects | Startup failed — check `/tmp/kubernetes-mcp-server.log` |
| `stat ~/.kube/config: no such file` | Add `--kubeconfig` with absolute path |
| Only `core` + `config` tools | Add `--toolsets` or `--config` with toolsets list |
| TOML `kubeconfig = "~/.kube/config"` fails | Tilde is **not** expanded — use absolute path or `--kubeconfig` |

## Next steps

- [Build from Source](build-from-source.md)
- [Cursor Integration](cursor-integration.md)
- [Configuration](configuration.md)
- [Toolsets Reference](../reference/toolsets.md)
