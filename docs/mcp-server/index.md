# Kubernetes / OpenShift MCP Server

Generic setup for **[kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)**
(the binary built from [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)).

Use this section when you want **cluster tools only** — no SRE prompts, no mandatory RCA reports.

!!! tip "Need the sample agent?"
    See **[SRE Agent](../sre-agent/index.md)** for prompts, report templates, and `sre-agent.toml`.
    Setup: **[Quick Start](../quickstart.md)** first, then **[SRE Agent](../sre-agent/index.md)** for `--config`.

## What you get

- Native Go MCP server (not a `kubectl` wrapper)
- Toolsets selected via `--toolsets` or TOML
- stdio mode for Cursor (`--port ""`)
- Optional TOML for denied resources, prompts, drop-ins

## Setup paths

| Goal | Start here |
|------|------------|
| Sample SRE agent (prompts + RCA) | [Quick Start](../quickstart.md) → [SRE Agent](../sre-agent/index.md) |
| Generic MCP server in Cursor | [Cursor Integration](cursor-integration.md) |
| Build OpenShift fork | [Build from Source](build-from-source.md) |
| npm / binary / Docker | [Installation](installation.md) |
| Flags and TOML | [Configuration](configuration.md) |

## Common issues

| Symptom | Fix |
|---------|-----|
| MCP shows help text then disconnects | Startup failed — check `/tmp/kubernetes-mcp-server.log` |
| `stat ~/.kube/config: no such file` | Add `--kubeconfig` with absolute path |
| Only `core` + `config` tools | Add `--toolsets` — see [Configuration](configuration.md) |
| TOML `kubeconfig = "~/.kube/config"` fails | Tilde is **not** expanded — use absolute path or `--kubeconfig` |

## Next steps

- [Build from Source](build-from-source.md)
- [Cursor Integration](cursor-integration.md)
- [Configuration](configuration.md)
- [Toolsets Reference](../reference/toolsets.md)
