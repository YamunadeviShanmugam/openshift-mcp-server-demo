# Quick Start

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

## Step 2 — Apply MCP config (Cursor)

Edit **`~/.cursor/mcp.json`**. Use **absolute paths**:

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

- [SRE Workflows](workflows/cluster-health.md) — day-to-day SRE playbooks
- [SRE Agent](sre-agent/index.md) — sample agent, prompts, RCA reports
- [Cluster Health Workflow](workflows/cluster-health.md) — monitor nodes, pods, and operators
