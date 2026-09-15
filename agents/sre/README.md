# SRE Agent Setup

The demo uses **one setup flow**:

1. Clone and build [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
2. Clone this demo repo
3. Apply [`mcp.json.example`](mcp.json.example) to `~/.cursor/mcp.json`

**Start here:** [Quick Start](../../docs/getting-started/quickstart.md)

## What's included

```
agents/sre/
├── sre-agent.toml           # MCP config (toolsets, security)
├── mcp.json.example         # Cursor MCP template — copy paths into ~/.cursor/mcp.json
├── conf.d/
│   ├── 00-local.toml        # kubeconfig = ~/.kube/config
│   └── 20-prompts.toml      # /live-cluster-rca, /must-gather-rca, …
├── skills/sre-rca-report/
└── reports/                 # Generated RCA markdown files
```

## The three steps (summary)

### 1. MCP server

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build
```

Binary: `./kubernetes-mcp-server`

### 2. Demo repo

```bash
git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
```

### 3. Cursor config

Merge [`mcp.json.example`](mcp.json.example) into `~/.cursor/mcp.json` with your absolute paths.
Open **openshift-mcp-server-demo** in Cursor. Restart Cursor.

## Run the demo

```
/live-cluster-rca
```

Reports → `agents/sre/reports/live-rca-*.md`

## MCP prompts

| Prompt | Use case |
|---|---|
| `/live-cluster-rca` | Full cluster RCA |
| `/live-etcd-analysis` | etcd pods / quorum |
| `/live-component-rca <name>` | Ingress, nodes, operators, … |
| `/must-gather-rca <path>` | Offline must-gather directory |
| `/prow-job-analysis <url>` | Prow CI job |

## Report format

Template: `reports/rca-report-template.md`  
Example: `reports/mustgather-rca-pkhblocphcprod-2025-08-12.md`

## Security defaults (`sre-agent.toml`)

- Denies Secret, ConfigMap, ClusterRole, ClusterRoleBinding reads
- Toolsets: core, openshift, cluster-diagnostics, must-gather, helm, CNI, OVN

## Reference

- [Quick Start](../../docs/getting-started/quickstart.md)
- [Build from Source](../../docs/getting-started/build-from-source.md)
- [Cursor Integration](../../docs/getting-started/cursor-integration.md)
