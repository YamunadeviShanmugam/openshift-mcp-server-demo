# OpenShift SRE Agent

The **SRE agent** bundles MCP prompts, report templates, security defaults, and team onboarding
on top of **kubernetes-mcp-server**.

!!! info "MCP server only?"
    For generic cluster tools without prompts or RCA reports, use the
    **[MCP Server](../mcp-server/index.md)** tab.

## What the SRE agent adds

| Feature | Location |
|---------|----------|
| MCP prompts (`/live-cluster-rca`, `/must-gather-rca`, …) | `agents/sre/conf.d/20-prompts.toml` |
| Mandatory RCA report output rules | `agents/sre/conf.d/10-server-instructions.toml` |
| Toolsets + security denials | `agents/sre/sre-agent.toml` |
| Report templates + examples | `agents/sre/reports/` |
| Cursor skill | `.agents/skills/sre-rca-report/` |

## Team setup (3 steps)

| Step | Action |
|------|--------|
| **1** | Clone [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server) → `make build` |
| **2** | Clone this demo repo |
| **3** | Apply [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example) → `~/.cursor/mcp.json` |

Full walkthrough: **[Quick Start](../getting-started/quickstart.md)**

Open **openshift-mcp-server-demo** in Cursor → run `/live-cluster-rca` → report in `agents/sre/reports/`.

## MCP config template

[`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example):

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

!!! warning "Always set `--kubeconfig`"
    Use an **absolute path**. Do not rely on `kubeconfig = "~/.kube/config"` in TOML — tilde is not expanded.

## CLI flags alternative

Without `--config`, you get MCP tools only — no slash prompts, server instructions, or
`denied_resources` from TOML. See [MCP Server Configuration](../mcp-server/configuration.md#cli-flags).

## Directory layout

```text
agents/sre/
├── sre-agent.toml           # Toolsets, security, loads conf.d/
├── mcp.json.example         # Cursor template
├── conf.d/
│   ├── 10-server-instructions.toml
│   ├── 20-prompts.toml
│   └── 99-local.toml.example   # optional personal overrides (gitignored)
├── skills/sre-rca-report/
└── reports/                 # Generated RCA markdown files
```

## MCP prompts

**[Prompt Examples →](prompt-examples.md)** — full copy-paste catalog.

| Prompt | Example |
|--------|---------|
| `/live-cluster-rca` | `/live-cluster-rca API 503 after worker reboot` |
| `/live-etcd-analysis` | `/live-etcd-analysis slow API and etcd Degraded` |
| `/live-component-rca <name>` | `/live-component-rca ingress` |
| `/must-gather-rca <dir>` | `/must-gather-rca /tmp/mg-extracted/.../registry-sha-dir/` |

```
Give me a quick health check: nodes, pressure, top namespaces, CrashLoopBackOff pods
```

## Next steps

- [Prompt Examples](prompt-examples.md)
- [Quick Start](../getting-started/quickstart.md)
- [Setup Guide](../getting-started/sre-agent.md)
- [Cursor Integration](../getting-started/cursor-integration.md)
- [Team Onboarding](../../TEAM_ONBOARDING.md)
- [Cluster Health Workflow](../workflows/cluster-health.md)
