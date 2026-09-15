# Team Onboarding Guide

Welcome! Two-step setup: **Quick Start** (MCP server) → **SRE Agent** (`--config`).

## MCP server (everyone)

| Step | Action | Doc |
|------|--------|-----|
| **1** | Clone [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server) → `make build` | [Quick Start Step 1](docs/quickstart.md#step-1--clone-and-build-openshift-mcp-server) |
| **2** | Add `~/.cursor/mcp.json` with **`--toolsets`** (no TOML) | [Quick Start Step 2](docs/quickstart.md#step-2--apply-mcp-config-cursor) |
| **3** | Verify: `List all namespaces using MCP` | [Quick Start Step 3](docs/quickstart.md#step-3--verify) |

## Sample SRE agent (prompts + RCA)

| Step | Action | Doc |
|------|--------|-----|
| **1** | Clone this demo repo | [SRE Agent Step 1](docs/sre-agent/index.md#step-1--clone-the-demo-repo) |
| **2** | Switch to **`--config`** + `sre-agent.toml` in `mcp.json` | [SRE Agent Step 2](docs/sre-agent/index.md#step-2--switch-to-sre-agent-config-cursor) |
| **3** | Open demo repo in Cursor → `/live-cluster-rca` | [SRE Agent Step 3](docs/sre-agent/index.md#step-3--run-the-sample-agent) |

Template: [`agents/sre/mcp.json.example`](agents/sre/mcp.json.example)

## Day 1 (30 minutes)

1. Complete **Quick Start** + **SRE Agent** steps (15 min)
2. Run `/live-cluster-rca` (10 min)
3. Open saved report in `agents/sre/reports/` (5 min)

Optional: [Prompt Examples](docs/sre-agent/prompt-examples.md)

## Day 2 — Pick a workflow

- [Cluster Health Monitoring](docs/workflows/cluster-health.md)
- [Troubleshooting & Debugging](docs/workflows/troubleshooting.md)
- [Must-Gather Analysis](docs/advanced/must-gather.md)
- [Incident Response](docs/workflows/incident-response.md)

## Kickoff demo script (team leads)

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build

cd .. && git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cat openshift-mcp-server-demo/agents/sre/mcp.json.example
```

In Cursor: show MCP connected → run `/live-cluster-rca` → open new file in `agents/sre/reports/`.

## Checklist

- [ ] `kubernetes-mcp-server` binary built
- [ ] `~/.cursor/mcp.json` with `--toolsets` (Quick Start) or `--config` (SRE Agent)
- [ ] `--kubeconfig` absolute path set
- [ ] Demo repo cloned (for SRE agent)
- [ ] Cursor workspace = demo repo (for reports)
- [ ] At least one report in `agents/sre/reports/`

## Common questions

**Do we use npm?**  
No for this demo — build from the OpenShift fork with `make build`.

**Where is the TOML config?**  
Only for the SRE agent: `agents/sre/sre-agent.toml` via `--config` in [SRE Agent docs](docs/sre-agent/index.md). Quick Start uses `--toolsets` only.

**MCP shows help text then disconnects?**  
Startup failed — usually missing kubeconfig. Check `tail -20 /tmp/kubernetes-mcp-server.log`.

**Generic MCP server without SRE agent?**  
Stop at [Quick Start](docs/quickstart.md) — no `--config` needed.

## Resources

- [Quick Start](docs/quickstart.md)
- [SRE Agent](docs/sre-agent/index.md)
- [MCP Server Overview](docs/mcp-server/index.md)
- [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)

---

**Welcome!** Start with [Quick Start](docs/quickstart.md), then [SRE Agent](docs/sre-agent/index.md).
