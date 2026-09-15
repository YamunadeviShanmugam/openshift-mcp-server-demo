# SRE Agent Setup

The demo uses **one setup flow**:

1. Clone and build [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
2. Clone this demo repo
3. Apply [`mcp.json.example`](mcp.json.example) to `~/.cursor/mcp.json`

**Start here:** [Quick Start](../../docs/getting-started/quickstart.md)  
**Docs:** [SRE Agent tab](../../docs/sre-agent/index.md) in MkDocs

## What's included

```
agents/sre/
├── sre-agent.toml           # MCP config (toolsets, security)
├── mcp.json.example         # Cursor MCP template — absolute paths required
├── conf.d/
│   ├── 10-server-instructions.toml
│   └── 20-prompts.toml      # /live-cluster-rca, /must-gather-rca, …
├── skills/sre-rca-report/
└── reports/                 # Generated RCA markdown files
```

## Cursor config (summary)

Merge [`mcp.json.example`](mcp.json.example) into `~/.cursor/mcp.json`:

- Server name → **`openshift-mcp-server`**
- `command` → built `kubernetes-mcp-server` binary
- `--kubeconfig` → **absolute path** to your kubeconfig
- `--config` → this directory's `sre-agent.toml`
- `--port ""` and `--log-file` → required for Cursor stdio

Do **not** set `kubeconfig = "~/.kube/config"` in TOML — tilde is not expanded.

## Run the demo

```
/live-cluster-rca
```

Reports → `agents/sre/reports/live-rca-*.md`

## MCP prompts

**[Full prompt examples →](../../docs/sre-agent/prompt-examples.md)**

| Prompt | Example |
|--------|---------|
| `/live-cluster-rca` | `/live-cluster-rca API 503 after node reboot` |
| `/live-etcd-analysis` | `/live-etcd-analysis` |
| `/live-component-rca <name>` | `/live-component-rca ingress` |
| `/must-gather-rca <path>` | `/must-gather-rca /tmp/mg-extracted/.../registry-sha-dir/` |

## Security defaults (`sre-agent.toml`)

- Denies Secret, ConfigMap, ClusterRole, ClusterRoleBinding reads
- Toolsets: core, openshift, cluster-diagnostics, must-gather, CNI, OVN

## Reference

- [Prompt Examples](../../docs/sre-agent/prompt-examples.md)
- [Quick Start](../../docs/getting-started/quickstart.md)
- [MCP Server tab](../../docs/mcp-server/index.md) — generic setup without SRE agent
- [Cursor Integration](../../docs/getting-started/cursor-integration.md)
