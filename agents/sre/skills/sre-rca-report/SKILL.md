---
name: sre-rca-report
description: >
  Produce a well-formatted OpenShift SRE incident/health RCA report with chain of events,
  primary root cause, secondary causes, impact, remediation, and shareable workflow.
  Use for live cluster diagnostics, must-gather offline analysis, or combined investigations.
when_to_use: >
  Trigger when: user asks for cluster health check, incident analysis, RCA, root cause,
  must-gather analysis, chain of events, component analysis (etcd, ingress, nodes, operators),
  "analyze X pods", or any diagnostic that uses kubernetes-mcp-server tools on live or offline data.
  ALWAYS save report to agents/sre/reports/ — never chat-only.
---

# SRE RCA Report Skill

Always produce reports using **kubernetes-mcp-server tools only** (no oc/kubectl). Follow the
structure in `agents/sre/reports/rca-report-template.md`. Example: `agents/sre/reports/mustgather-rca-*.md`.

## Save report as a document file (mandatory)

Every RCA must be written to disk — not chat-only.

| Source | Default output path |
|---|---|
| Live cluster (full) | `agents/sre/reports/live-rca-{cluster}-{YYYY-MM-DD}.md` |
| Live cluster (scoped) | `agents/sre/reports/live-rca-{cluster}-{scope}-{YYYY-MM-DD}.md` |
| Must-gather | `agents/sre/reports/mustgather-rca-{cluster}-{YYYY-MM-DD}.md` |

Rules:
- Create `agents/sre/reports/` if it does not exist
- `{cluster}` = short cluster name from ClusterVersion or API hostname (lowercase, hyphens)
- If same path exists on the same day, append `-HHMM` (UTC)
- Use the host agent's file-write tool; paste the **full** report (all sections)
- In chat: give absolute path + 2–3 sentence summary (primary cause, status, top P0)

See `agents/sre/reports/README.md` for naming details.

## Readability rules (mandatory)

Follow `agents/sre/reports/rca-report-template.md` layout exactly:

1. **Status line** under title: `> Status · Severity · Source · OCP version`
2. **At a Glance** — 2 compact key-value tables (cluster, cause, impact, P0 + ruled-out/etcd one-liners)
3. **Contents** — linked table of contents for all 9 sections
4. **Shorter tables** — use node aliases (`m1`, `w2`) with full hostname in parentheses; ISO times as `HH:MM UTC` in timelines when same day
5. **One-line chain** — blockquote (`>`) not buried in prose
6. **Critical vs full timeline** — show 5–8 critical rows; put earlier events in `<details>` collapsible
7. **Remediation** — numbered tables (`| # | Action | Why |`) not long bullet lists
8. **Appendix** — wrap each evidence snippet in `<details><summary>…</summary>` so the doc stays scannable
9. **Avoid repetition** — state primary cause once in At a Glance, once in Executive Summary table, once in §3

## Mandatory report sections (in order)

1. **At a Glance + metadata** — status line, glance tables, report metadata table
2. **Executive Summary** — 3 short paragraphs + summary key-value table
3. **Chain of Events** — mermaid + blockquote one-liner + critical timeline + optional `<details>` full timeline
4. **Primary Root Cause** — 4-row key table + ruled-out table
5. **Secondary Causes** — single table with Category column
6. **Impact Assessment** — area / impact / duration table
7. **Cluster State** — nodes, etcd, operators, top-5 problem pods (short names)
8. **Remediation** — P0/P1/P2 numbered action tables
9. **Shareable Workflow** — tools table + reproduce commands + audience matrix
10. **Appendix** — collapsible `<details>` evidence blocks

## Chain of events rules

- Order events **chronologically** (earliest trigger first)
- Each step must show **cause → effect** (what enabled the next step)
- Identify the **trigger event** (reboot, upgrade, config change, network partition, etc.)
- Distinguish **symptoms** (503, NodeNotReady) from **root cause** (CP reboot)
- Use mermaid `flowchart TD` for the causal chain

## Primary vs secondary cause rules

| Type | Definition | Example |
|---|---|---|
| **Primary** | Initiating event without which the incident would not have occurred | CP node reboot |
| **Secondary** | Pre-existing or amplifying issues that worsened impact or delayed recovery | ingress-operator crash loop, ImagePolicy blocking monitoring |

## Live cluster workflow

Run tools in this order, then write the report:

| Step | MCP tools |
|---|---|
| 1. Nodes | `resources_list` (v1 Node), `nodes_top` |
| 2. Workloads | `pods_list`, `pods_top`, filter CrashLoopBackOff/Failed in analysis |
| 3. Events | `events_list` (Warning), grep reasons: Rebooted, NodeNotReady, BackOff, FailedMount |
| 4. Operators | `resources_list` (config.openshift.io ClusterOperator, ClusterVersion) |
| 5. etcd | `pods_list_in_namespace` (openshift-etcd), `pods_exec` etcdctl endpoint health/status |
| 6. Logs | `pods_log`, `pods_log` + grep for degraded operators |
| 7. Network (if needed) | cni-diagnostics / ovn-kubernetes toolsets |

Built-in prompts: `/live-cluster-rca`, `/live-etcd-analysis`, `/live-component-rca` (from `agents/sre/conf.d/20-prompts.toml`)

Ad-hoc requests ("analyze etcd pods") use the same report workflow — save scoped file before replying.

## Must-gather workflow

| Step | MCP tools | Notes |
|---|---|---|
| 1. Prepare | Extract `.tar.gz` to directory | **Never** pass `.tar.gz` directly to `mustgather_use` |
| 2. Load | `mustgather_use` | Verify `Resources indexed > 0` |
| 3. Nodes | `mustgather_resources_list` (v1 Node) | |
| 4. Events | `mustgather_events_list` (Warning, Rebooted, BackOff, NodeNotReady) | Build timeline |
| 5. etcd | `mustgather_etcd_health`, `mustgather_etcd_object_count` | Raw JSON fallback in bundle if tool parse fails |
| 6. Monitoring | `mustgather_monitoring_prometheus_alerts`, `mustgather_monitoring_prometheus_targets` | |
| 7. Logs | `mustgather_pod_logs_grep`, `mustgather_node_kubelet_logs_grep` | |
| 8. Report | Use template sections above | |

Built-in prompt: `/must-gather-rca` (from `agents/sre/conf.d/20-prompts.toml`)

## Share guidance (section 9)

| Audience | Include |
|---|---|
| SRE / Platform | Full report + mermaid + workflow |
| Management | Executive Summary + Impact + P0 actions only |
| Red Hat support | Full report + must-gather path + cluster version |

## Quality checklist before delivering

- [ ] Report saved to `agents/sre/reports/*.md` with correct naming convention
- [ ] Chat reply includes absolute path to the saved file
- [ ] Chain of events has at least 5 numbered steps with timestamps
- [ ] Primary cause is ONE sentence with evidence citation
- [ ] At least 2 secondary causes identified (or explicit "none found")
- [ ] Impact table filled in
- [ ] P0/P1/P2 remediation items are actionable
- [ ] Workflow section lists exact MCP tool names used
