# Prompt Examples

Copy-paste examples for Cursor chat with the **SRE agent** configured (`sre-agent.toml` + MCP connected).

!!! important "MCP tools only"
    Every prompt on this page must be answerable using **kubernetes-mcp-server tools**
    from the enabled toolsets in [`sre-agent.toml`](../../agents/sre/sre-agent.toml).
    Prompts that require external HTTP fetch (Prow build logs, GCS artifacts, web URLs) are
    **not** included — download must-gather locally first, then use `/must-gather-rca`.

Reports save under **`agents/sre/reports/`** — open **openshift-mcp-server-demo** as the Cursor workspace.

---

## Enabled toolsets

From `sre-agent.toml`: `core`, `config`, `openshift`, `cluster-diagnostics`,
`openshift/mustgather`, `cni-diagnostics`, `ovn-kubernetes`.

| Toolset | Example MCP tools |
|---------|-------------------|
| **core** | `resources_list`, `resources_get`, `pods_list`, `pods_log`, `pods_exec`, `pods_top`, `events_list`, `nodes_top` |
| **openshift** | `projects_list`, `namespaces_list` |
| **config** | `configuration_contexts_list`, `targets_list` |
| **cluster-diagnostics** | `nodes_debug_exec`, `nodes_log`, `nodes_stats_summary` |
| **openshift/mustgather** | `mustgather_use`, `mustgather_events_list`, `mustgather_etcd_health`, `mustgather_pod_logs_grep`, … |
| **cni-diagnostics** | `get-conntrack`, `get-ip`, `get-iptables`, `get-nft`, `tcpdump`, `pwru` |
| **ovn-kubernetes** | `ovn_get`, `ovn_show`, `ovn_trace`, `ovn_lflow_list`, `ovs_appctl`, `ovs_ofctl`, `ovs_vsctl` |

---

## MCP slash prompts

Registered in [`agents/sre/conf.d/20-prompts.toml`](../../agents/sre/conf.d/20-prompts.toml).

| Prompt | MCP tools used | Report pattern |
|--------|----------------|----------------|
| `/live-cluster-rca` | `resources_list`, `nodes_top`, `pods_list`, `events_list`, `pods_log`, `pods_exec` | `live-rca-{cluster}-{date}.md` |
| `/live-etcd-analysis` | `resources_get`, `pods_list_in_namespace`, `pods_exec`, `pods_log`, `events_list` | `live-rca-{cluster}-etcd-{date}.md` |
| `/live-component-rca <name>` | Component-specific subset of core + diagnostics tools | `live-rca-{cluster}-{component}-{date}.md` |
| `/must-gather-rca <dir>` | `mustgather_use` + all `mustgather_*` tools | `mustgather-rca-{cluster}-{date}.md` |

### Examples

```
/live-cluster-rca API 503 errors after worker node reboot
```

```
/live-etcd-analysis slow API responses and etcd operator Degraded
```

```
/live-component-rca ingress
```

```
/live-component-rca network
```

```
/must-gather-rca /tmp/mg-extracted/must-gather.local.123456789/registry-sha256-abc/
```

---

## Live cluster — health & triage

Uses: `resources_list`, `nodes_top`, `pods_list`, `pods_top`, `events_list`

### Quick health check

```
Give me a quick health check of my OpenShift cluster:
1. How many nodes and what's their status?
2. Are there nodes with memory or CPU pressure?
3. Which namespaces have the most pod activity?
4. Show me any pods in CrashLoopBackOff or Error states
```

### Operator sweep

```
Use resources_list for ClusterOperator and ClusterVersion.
List operators where Available is not True.
For each degraded operator, use events_list (Warning) and pods_log on related pods.
```

### Node pressure

```
Use nodes_top and resources_list (Node) to find nodes under pressure.
Use pods_top on affected nodes and events_list for eviction or scheduling failures.
```

### API / control plane

```
Use pods_list_in_namespace (openshift-etcd), pods_exec (etcdctl endpoint health),
events_list in openshift-etcd and openshift-kube-apiserver, and pods_log on apiserver pods.
Save a scoped RCA report.
```

---

## Live cluster — incident response

Uses: `pods_list`, `pods_log`, `events_list`, `resources_get`, `nodes_log`, `nodes_debug_exec`

### CrashLoopBackOff

```
Pods in namespace openshift-ingress are CrashLoopBackOff.
Use pods_list, pods_log (previous=true), events_list, and resources_get on the Deployment.
```

### Node NotReady

```
Node worker-3 is NotReady. Use events_list filtered to that node, nodes_log,
and nodes_stats_summary for worker-3.
```

### Image pull failures

```
Use pods_list to find ImagePullBackOff or ErrImagePull pods cluster-wide.
Use events_list on failing pods to identify registry vs quota vs RBAC causes.
```

### Recent change correlation

```
Use events_list (type=Warning) and resources_list (ClusterOperator)
to build a timeline of the last 30 minutes. Correlate with pods_list for restarts.
```

---

## Network diagnostics (CNI / OVN)

Uses: `ovn-kubernetes` + `cni-diagnostics` toolsets (live cluster only)

```
Use pods_list in openshift-ovn-kubernetes. Check Warning events for OVN/CNI.
Use ovn_show and ovn_trace for pod-to-service connectivity in namespace X.
```

```
Pod cannot reach a Service — use get-ip on the source node, get-conntrack for the flow,
and ovn_trace from source pod to destination.
```

---

## Multi-cluster

Uses: `mustgather_use` then `mustgather_*` tools only. **Extract the tar locally first** (shell — not an MCP tool):

```bash
tar -xzf must-gather.tar -C /tmp/mg-extracted/
# Inner path: /tmp/mg-extracted/<outer>/<registry-sha-dir>/
```

Then in Cursor:

```
/must-gather-rca /tmp/mg-extracted/must-gather.local.1723456789/registry-sha256-deadbeef/
```

```
Using mustgather tools on /path/to/extracted/dir:
mustgather_resources_list (ClusterOperator), mustgather_etcd_health,
mustgather_events_list (Warning), mustgather_monitoring_prometheus_alerts.
Save mustgather-rca report.
```

```
mustgather_events_by_time for the incident window, then mustgather_pod_logs_grep
on degraded operator pods with pattern error|failed|timeout.
```

!!! tip "CI job must-gather"
    Download the bundle from Prow/GCS yourself (`curl`, `gcloud storage cp`, or browser),
    extract it, then run `/must-gather-rca` with the **directory path**. The MCP server
    cannot fetch URLs.

---

## Multi-cluster

Uses: `configuration_contexts_list`, `targets_list` (config toolset)

```
Use configuration_contexts_list to show available contexts, then run a health check
on the production context using pods_list and events_list.
```

---

## Ad-hoc prompts (no slash)

Direct tool invocation — no registered MCP prompt required:

```
List all namespaces with namespaces_list and count non-Running pods with pods_list.
```

```
Show Warning events in openshift-etcd using events_list.
```

```
Get etcd endpoint health with pods_exec in openshift-etcd (etcdctl endpoint health).
```

```
Run nodes_stats_summary on node worker-1.
```

---

## Report output

| Source | File pattern |
|--------|--------------|
| Live (full) | `live-rca-{cluster}-{date}.md` |
| Live (scoped) | `live-rca-{cluster}-{scope}-{date}.md` |
| Must-gather | `mustgather-rca-{cluster}-{date}.md` |

Template: [`agents/sre/reports/rca-report-template.md`](../../agents/sre/reports/rca-report-template.md)

Example: [`agents/sre/reports/mustgather-rca-pkhblocphcprod-2025-08-12.md`](../../agents/sre/reports/mustgather-rca-pkhblocphcprod-2025-08-12.md)

---

## Tips

- **MCP tools only** — if a step needs `curl`, browser download, or Prow API, do that outside Cursor first.
- **Must-gather** — pass the **extracted directory** to `mustgather_use`, never the `.tar` file.
- **Open demo repo** as workspace so reports land in `agents/sre/reports/`.
- **Verify MCP** — Settings → MCP → `kubernetes-mcp-server` connected.

## Related

- [Quick Start](../quickstart.md) — MCP server with `--toolsets` first
- [Must-Gather Analysis](../advanced/must-gather.md)
- [Toolsets Reference](../reference/toolsets.md)
