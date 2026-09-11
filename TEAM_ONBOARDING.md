# Team Onboarding Guide

Welcome! Follow **one setup flow** — no npm vs binary vs Docker choices for the demo.

## Demo setup (everyone)

| Step | Action | Doc |
|------|--------|-----|
| **1** | Clone [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server) → `make build` | [Quick Start Step 1](docs/getting-started/quickstart.md#step-1--clone-and-build-openshift-mcp-server) |
| **2** | Clone this demo repo | [Quick Start Step 2](docs/getting-started/quickstart.md#step-2--clone-the-demo-repo) |
| **3** | Apply [`agents/sre/mcp.json.example`](agents/sre/mcp.json.example) to `~/.cursor/mcp.json` | [Quick Start Step 3](docs/getting-started/quickstart.md#step-3--apply-mcp-config-cursor) |

Then: open **openshift-mcp-server-demo** in Cursor → `/live-cluster-rca`

## Day 1 (30 minutes)

1. Complete the **3 steps** above (15 min)
2. Run health-check prompt or `/live-cluster-rca` (10 min)
3. Open saved report in `agents/sre/reports/` (5 min)

Optional reading: [SRE Agent Setup](docs/getting-started/sre-agent.md)

## Day 2 — Pick a workflow

- [Cluster Health Monitoring](docs/workflows/cluster-health.md)
- [Troubleshooting & Debugging](docs/workflows/troubleshooting.md)
- [Must-Gather Analysis](docs/advanced/must-gather.md)
- [Incident Response](docs/workflows/incident-response.md)

## Kickoff demo script (team leads)

```bash
# Terminal — show live
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build
ls -la kubernetes-mcp-server

cd .. && git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
cat openshift-mcp-server-demo/agents/sre/mcp.json.example
```

In Cursor: show MCP connected → run `/live-cluster-rca` → open new file in `agents/sre/reports/`.

## Checklist

- [ ] `kubernetes-mcp-server` binary built
- [ ] Demo repo cloned
- [ ] `~/.cursor/mcp.json` configured from `mcp.json.example`
- [ ] Cursor workspace = demo repo
- [ ] At least one report in `agents/sre/reports/`

## Common questions

**Do we use npm?**  
No for the demo — build from the OpenShift fork with `make build`.

**Where is the config?**  
`agents/sre/sre-agent.toml` in this repo + `~/.cursor/mcp.json` pointing to the binary and that file.

**Different kubeconfig?**  
Copy `agents/sre/conf.d/99-local.toml.example` → `99-local.toml` (gitignored).

## Resources

- [Quick Start](docs/getting-started/quickstart.md)
- [Build from Source](docs/getting-started/build-from-source.md)
- [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)

---

**Welcome!** Start with [Quick Start](docs/getting-started/quickstart.md).
