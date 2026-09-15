# Quick Start

Get **kubernetes-mcp-server** running in Cursor with CLI flags — no TOML config file.

!!! tip "Sample SRE agent (prompts + RCA reports)?"
    Complete this Quick Start first, then **[SRE Agent](sre-agent/index.md)** — add `--config` with `sre-agent.toml` there only.

## Prerequisites

- **Go** and **make** (to build the MCP server)
- **Git**
- OpenShift/Kubernetes **kubeconfig** (absolute path) and **`oc` CLI**
- **Cursor IDE**

Verify cluster access:

```bash
oc get nodes
```

## Step 1 — Clone and build openshift-mcp-server

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server
make build
```

You get **`./kubernetes-mcp-server`** in that directory.

Smoke test (optional):

```bash
./kubernetes-mcp-server --version
npx @modelcontextprotocol/inspector@latest $(pwd)/kubernetes-mcp-server
```

Build details: [MCP Server — Build from Source](mcp-server/build-from-source.md)

## Step 2 — Apply MCP config (Cursor)

Edit **`~/.cursor/mcp.json`**. Use **absolute paths** and **`--toolsets`** (not `--config`):

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
      ],
      "env": {
        "KUBECONFIG": "/ABSOLUTE/PATH/to/your/kubeconfig"
      }
    }
  }
}
```

| Flag | Why |
|------|-----|
| `command` | Binary from **Step 1** (`make build`) |
| `--port ""` | stdio mode for Cursor |
| `--kubeconfig` | **Required** — absolute path to your kubeconfig |
| `--toolsets` | Enables cluster tools (comma-separated list) |
| `--log-file` | Required in stdio mode (keeps logs off stdout) |

More options: [MCP Server — Cursor Integration](mcp-server/cursor-integration.md)

**Restart Cursor** → **Settings → MCP** → `openshift-mcp-server` connected.

## Step 3 — Verify

In Cursor chat:

```
List all namespaces using MCP
```

Or:

```
Show me pods not in Running or Completed state across the cluster
```

Check logs if needed: `tail -f /tmp/kubernetes-mcp-server.log`

## ✅ Checklist

- [ ] `openshift-mcp-server/kubernetes-mcp-server` exists (`make build`)
- [ ] `~/.cursor/mcp.json` has absolute paths for binary and kubeconfig
- [ ] `--toolsets` set (not bare defaults)
- [ ] MCP connected in Cursor settings

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Help text in MCP log, then disconnect | Startup crash — `tail -20 /tmp/kubernetes-mcp-server.log` |
| `stat ~/.kube/config: no such file` | Add `--kubeconfig` with absolute path to `mcp.json` |
| MCP not connected | Valid JSON; restart Cursor |
| Only basic tools | Add `--toolsets` — see [Toolsets Guide](reference/toolsets.md) |
| Build fails | Match Go version in `openshift-mcp-server/go.mod` |

## Next steps

- [MCP Server](mcp-server/index.md) — install, configuration, all flags
- [SRE Agent](sre-agent/index.md) — add `sre-agent.toml` via `--config`, MCP prompts, RCA reports
- [Cluster Health Workflow](workflows/cluster-health.md)
