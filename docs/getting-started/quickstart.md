# Quick Start — SRE Agent Demo (one flow)

Single setup path for the demo: **clone MCP server → clone demo → build & apply config**.

## Prerequisites

- **Go** and **make** (to build the MCP server)
- **Git**
- OpenShift/Kubernetes **kubeconfig** (default: `~/.kube/config`)
- **Cursor IDE**

Verify cluster access:

```bash
kubectl get nodes
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

More build details: [Build from Source](build-from-source.md)

## Step 2 — Clone the demo repo

In a sibling directory (or anywhere you prefer):

```bash
cd ..
git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cd openshift-mcp-server-demo
```

Demo layout:

```text
agents/sre/
├── sre-agent.toml          # MCP toolsets, prompts, security
├── conf.d/00-local.toml    # kubeconfig = ~/.kube/config
└── reports/                # RCA reports written here
```

Optional — different kubeconfig on your machine only:

```bash
cp agents/sre/conf.d/99-local.toml.example agents/sre/conf.d/99-local.toml
# edit path — gitignored
```

## Step 3 — Apply MCP config (Cursor)

Edit **`~/.cursor/mcp.json`**. Use **absolute paths** for your machine:

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "/ABSOLUTE/PATH/openshift-mcp-server/kubernetes-mcp-server",
      "args": [
        "--port",
        "",
        "--config",
        "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml",
        "--log-file",
        "/tmp/kubernetes-mcp-server.log"
      ],
      "env": {
        "KUBECONFIG": "/Users/YOU/.kube/config"
      }
    }
  }
}
```

| Flag | Why |
|------|-----|
| `command` | Binary from **Step 1** (`make build`) |
| `--port ""` | stdio mode for Cursor |
| `--config` | SRE agent TOML from **Step 2** |

Copy-paste template: [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example)

**Restart Cursor**, then open **`openshift-mcp-server-demo`** as the workspace (reports save under `agents/sre/reports/`).

## Step 4 — Run the demo

In Cursor chat:

```
/live-cluster-rca
```

Or the health-check prompt:

```
Give me a quick health check of my OpenShift cluster:
1. How many nodes and what's their status?
2. Are there nodes with memory or CPU pressure?
3. Which namespaces have the most pod activity?
4. Show me any pods in CrashLoopBackOff or Error states
```

Expected: analysis + file such as  
`agents/sre/reports/live-rca-{cluster}-{date}.md`

## MCP prompts

| Prompt | Purpose |
|--------|---------|
| `/live-cluster-rca` | Full live cluster RCA |
| `/live-etcd-analysis` | etcd health |
| `/live-component-rca <name>` | Scoped analysis |
| `/must-gather-rca <dir>` | Offline bundle |

## ✅ Checklist

- [ ] `openshift-mcp-server/kubernetes-mcp-server` exists (`make build`)
- [ ] `openshift-mcp-server-demo/agents/sre/sre-agent.toml` exists
- [ ] `~/.cursor/mcp.json` points to both paths above
- [ ] Cursor workspace = **openshift-mcp-server-demo**
- [ ] MCP connected in Cursor settings
- [ ] Report saved under `agents/sre/reports/`

## Troubleshooting

| Problem | Fix |
|---------|-----|
| MCP not connected | Valid JSON; restart Cursor; `tail -f /tmp/kubernetes-mcp-server.log` |
| Wrong cluster | `kubectl config current-context`; set `KUBECONFIG` in `mcp.json` |
| No report file | Open demo repo as Cursor workspace |
| Build fails | Match Go version in `openshift-mcp-server/go.mod` |

## Next steps

- [SRE Agent Setup](sre-agent.md) — prompts, reports, security
- [Build from Source](build-from-source.md) — multi-platform builds, mcp-inspector
- [Cluster Health Workflow](../workflows/cluster-health.md)
