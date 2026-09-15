# Quick Start — SRE Agent Demo

Single setup path: **clone MCP server → clone demo → apply config**.

!!! tip "Generic MCP server only?"
    See the **[MCP Server](../mcp-server/index.md)** tab for CLI-based setup.

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

Build details: [MCP Server — Build from Source](../mcp-server/build-from-source.md)

## Step 2 — Clone the demo repo

```bash
cd ..
git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cd openshift-mcp-server-demo
```

Demo layout:

```text
agents/sre/
├── sre-agent.toml          # MCP toolsets, security, loads conf.d/
├── mcp.json.example        # Cursor template
├── conf.d/                 # prompts + server instructions
└── reports/                # RCA reports written here
```

Optional — personal overrides (gitignored):

```bash
cp agents/sre/conf.d/99-local.toml.example agents/sre/conf.d/99-local.toml
# edit kubeconfig or other settings
```

## Step 3 — Apply MCP config (Cursor)

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
        "--config",
        "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml",
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
| `--config` | **`sre-agent.toml`** — toolsets, prompts (`conf.d/`), security |
| `--log-file` | Required in stdio mode |

Toolsets, `list_output`, denied resources, and MCP prompts load from **`sre-agent.toml`** and **`conf.d/`** — do not duplicate in `--toolsets`.

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

Full catalog: **[Prompt Examples](../sre-agent/prompt-examples.md)**

| Prompt | Example |
|--------|---------|
| `/live-cluster-rca` | `/live-cluster-rca API 503 after worker reboot` |
| `/live-etcd-analysis` | `/live-etcd-analysis slow API and etcd operator Degraded` |
| `/live-component-rca <name>` | `/live-component-rca ingress` |
| `/must-gather-rca <dir>` | `/must-gather-rca /tmp/mg-extracted/.../registry-sha-dir/` |

### More natural-language examples

```
List ClusterOperators that are not Available=True and summarize impact.
```

```
Node worker-2 is NotReady — build a timeline from events and kubelet logs.
```

```
Find all pods in ImagePullBackOff cluster-wide and group by likely cause.
```

## ✅ Checklist

- [ ] `openshift-mcp-server/kubernetes-mcp-server` exists (`make build`)
- [ ] `openshift-mcp-server-demo/agents/sre/sre-agent.toml` exists
- [ ] `~/.cursor/mcp.json` has absolute paths for binary, kubeconfig, and config
- [ ] Cursor workspace = **openshift-mcp-server-demo**
- [ ] MCP connected in Cursor settings
- [ ] Report saved under `agents/sre/reports/`

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Help text in MCP log, then disconnect | Startup crash — `tail -20 /tmp/kubernetes-mcp-server.log` |
| `stat ~/.kube/config: no such file` | Add `--kubeconfig` with absolute path to `mcp.json` |
| MCP not connected | Valid JSON; restart Cursor |
| Wrong cluster | `oc config current-context`; fix `--kubeconfig` |
| No report file | Open demo repo as Cursor workspace |
| Build fails | Match Go version in `openshift-mcp-server/go.mod` |

## Next steps

- [Prompt Examples](../sre-agent/prompt-examples.md) — full copy-paste catalog
- [SRE Agent Setup](sre-agent.md) — prompts, reports, security
- [Cursor Integration](cursor-integration.md)
- [Cluster Health Workflow](../workflows/cluster-health.md)
