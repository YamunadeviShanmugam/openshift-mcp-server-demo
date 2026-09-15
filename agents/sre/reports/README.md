# SRE RCA Report Output

The OpenShift SRE agent (`agents/sre/sre-agent.toml`) **always writes** RCA reports here as
markdown files — for full cluster RCAs, component drill-downs (etcd, ingress, …), and must-gather
analysis using **kubernetes-mcp-server MCP tools only**. Chat-only summaries are not acceptable; the agent must write the file before replying.

## Naming convention

| Analysis type | Filename pattern | Example |
|---|---|---|
| Live cluster (full) | `live-rca-{cluster}-{YYYY-MM-DD}.md` | `live-rca-rg-070901-2026-09-11.md` |
| Live cluster (scoped) | `live-rca-{cluster}-{scope}-{YYYY-MM-DD}.md` | `live-rca-rg-070901-etcd-2026-09-11.md` |
| Must-gather | `mustgather-rca-{cluster}-{YYYY-MM-DD}.md` | `mustgather-rca-pkhblocphcprod-2025-08-12.md` |

- `{cluster}` — short name from ClusterVersion or API server hostname (lowercase, dots → hyphens)
- Same cluster + same date → append `-HHMM` (UTC) to avoid overwrite

## Templates and examples (committed)

| File | Purpose |
|---|---|
| `rca-report-template.md` | Blank template (At a Glance, TOC, collapsible sections) |
| `mustgather-rca-pkhblocphcprod-2025-08-12.md` | Example must-gather RCA |

## How to trigger

1. Configure MCP with `agents/sre/sre-agent.toml` and `--port ""` (stdio)
2. Open this repository as your Cursor workspace
3. Ask for analysis — e.g. `/live-cluster-rca`, `/live-etcd-analysis`, "analyze etcd pods", `/must-gather-rca <path>`
4. Agent writes report here and replies with the absolute path

Skill: `agents/sre/skills/sre-rca-report/SKILL.md`
