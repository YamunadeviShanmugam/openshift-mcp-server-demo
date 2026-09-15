# OpenShift SRE Agent

**Sample agent leveraging MCP server** — MCP prompts, report templates, security defaults, and onboarding on top of **kubernetes-mcp-server**.

Uses **`--config`** with `sre-agent.toml` (TOML + `conf.d/` drop-ins). Do not use this on the [Quick Start](../quickstart.md) or [MCP Server](../mcp-server/index.md) pages — those use **`--toolsets`** only.

!!! info "MCP server only?"
    For cluster tools without prompts or RCA reports, see **[MCP Server](../mcp-server/index.md)** or **[Quick Start](../quickstart.md)**.

## Prerequisites

- Completed **[Quick Start](../quickstart.md)** — built binary, MCP connected with `--toolsets`
- **Cursor IDE**

## Step 1 — Clone the demo repo

```bash
git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cd openshift-mcp-server-demo
```

```text
agents/sre/
├── sre-agent.toml          # Toolsets, security, loads conf.d/
├── mcp.json.example        # Cursor template (--config)
├── conf.d/                 # prompts + server instructions
└── reports/                # RCA reports written here
```

Optional — personal overrides (gitignored):

```bash
cp agents/sre/conf.d/99-local.toml.example agents/sre/conf.d/99-local.toml
```

## Step 2 — Switch to SRE agent config (Cursor)

Replace **`--toolsets`** in `~/.cursor/mcp.json` with **`--config`** pointing at `sre-agent.toml`.

Copy from [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example):

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
| `--config` | Loads `sre-agent.toml` + `conf.d/` (toolsets, prompts, security) |
| `--kubeconfig` | **Absolute path** — required; tilde in TOML is not expanded |

Do **not** pass `--toolsets` when using `--config` — toolsets come from `sre-agent.toml`.

**Restart Cursor**, then open **`openshift-mcp-server-demo`** as the workspace (reports save under `agents/sre/reports/`).

## Step 3 — Run the sample agent

In Cursor chat:

```
/live-cluster-rca
```

Or:

```
Give me a quick health check of my OpenShift cluster:
1. How many nodes and what's their status?
2. Are there nodes with memory or CPU pressure?
3. Which namespaces have the most pod activity?
4. Show me any pods in CrashLoopBackOff or Error states
```

Expected: analysis + file such as `agents/sre/reports/live-rca-{cluster}-{date}.md`

## What the sample agent adds

| Feature | Location |
|---------|----------|
| MCP prompts (`/live-cluster-rca`, `/must-gather-rca`, …) | `agents/sre/conf.d/20-prompts.toml` |
| Mandatory RCA report output rules | `agents/sre/conf.d/10-server-instructions.toml` |
| Toolsets + security denials | `agents/sre/sre-agent.toml` |
| Report templates + examples | `agents/sre/reports/` |
| Cursor skill | `.agents/skills/sre-rca-report/` |

## Toolsets (in `sre-agent.toml`)

```
core, config, openshift, cluster-diagnostics, openshift/mustgather, cni-diagnostics, ovn-kubernetes
```

Edit `sre-agent.toml` to add/remove toolsets.

## MCP prompts

**[Prompt Examples →](prompt-examples.md)** — full copy-paste catalog.

| Prompt | Example |
|--------|---------|
| `/live-cluster-rca` | `/live-cluster-rca API 503 after worker reboot` |
| `/live-etcd-analysis` | `/live-etcd-analysis slow API and etcd Degraded` |
| `/live-component-rca <name>` | `/live-component-rca ingress` |
| `/must-gather-rca <dir>` | `/must-gather-rca /tmp/mg-extracted/.../registry-sha-dir/` |

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Slash prompts missing | `--config` must point to `agents/sre/sre-agent.toml` |
| No report file | Open demo repo as Cursor workspace |
| Help text then disconnect | Check `--kubeconfig` absolute path; read log file |

## Next steps

- [Prompt Examples](prompt-examples.md)
- [Team Onboarding](../../TEAM_ONBOARDING.md)
- [Cluster Health Workflow](../workflows/cluster-health.md)
