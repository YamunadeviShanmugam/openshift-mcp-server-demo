# SRE Agent Setup

Sample agent leveraging MCP server.

1. Complete **[Quick Start](../../docs/quickstart.md)** — MCP server with **`--toolsets`** (no TOML)
2. Clone this demo repo
3. Switch to **`--config`** → [`mcp.json.example`](mcp.json.example)

**SRE agent docs:** [SRE Agent overview](../../docs/sre-agent/index.md)

## What's included

```
agents/sre/
├── sre-agent.toml           # MCP config (toolsets, security) — used with --config only
├── mcp.json.example         # Cursor template (--config, not --toolsets)
├── conf.d/                  # prompts + server instructions
├── skills/sre-rca-report/
└── reports/                 # RCA markdown output
```

## Cursor config (SRE agent only)

See **[SRE Agent — Step 2](../../docs/sre-agent/index.md#step-2--switch-to-sre-agent-config-cursor)** for the full `mcp.json` with `--config`.

Key change from Quick Start: replace `--toolsets` with:

```
"--config", "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml"
```

## Security defaults (`sre-agent.toml`)

- Denies Secret, ConfigMap, ClusterRole, ClusterRoleBinding reads
- Disables `configuration_view` tool

## Run the demo

Open **openshift-mcp-server-demo** in Cursor → `/live-cluster-rca` → report in `reports/`.

## Reference

- [Prompt Examples](../../docs/sre-agent/prompt-examples.md)
- [Quick Start](../../docs/quickstart.md)
- [Quick Start](../../docs/quickstart.md)
