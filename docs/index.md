# OpenShift MCP Server

Native Go **Model Context Protocol (MCP) server** for Kubernetes and OpenShift — built from
[openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
(upstream: [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)).

Use the tabs above:

| Tab | Use when |
|-----|----------|
| **Home** (this page) | Build, configure, and run the MCP server in Cursor |
| **[MCP Server](mcp-server/index.md)** | Detailed guides — build, Cursor, configuration, install |
| **[SRE Agent](sre-agent/index.md)** | Team demo — `sre-agent.toml`, prompts, RCA reports |

---

## What you get

- Direct Kubernetes/OpenShift API access (not a `kubectl` wrapper)
- Toolsets via `--toolsets` or TOML config
- stdio mode for Cursor (`--port ""`)
- Multi-cluster kubeconfig support
- Optional must-gather offline analysis (`openshift/mustgather` toolset)

---

## Quick start (3 steps)

### 1. Build the binary

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server
make build
./kubernetes-mcp-server --version
```

### 2. Verify cluster access

```bash
oc get nodes
```

### 3. Configure Cursor

Add to **`~/.cursor/mcp.json`** (use **absolute paths**):

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
        "--toolsets",
        "core,config,openshift,cluster-diagnostics,openshift/mustgather,cni-diagnostics,ovn-kubernetes",
        "--list-output",
        "yaml",
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
| `--port ""` | stdio transport for Cursor |
| `--kubeconfig` | **Absolute path** — required when not using `~/.kube/config` |
| `--toolsets` | Default is only `core` + `config` without this flag |
| `--log-file` | Required in stdio mode (keeps logs off stdout) |

Restart Cursor → **Settings → MCP** → `openshift-mcp-server` connected.

Test in chat: `List all namespaces using MCP`

---

## Configuration

| Approach | Best for | Docs |
|----------|----------|------|
| **CLI flags** | Quick setup in `mcp.json` | [Configuration](mcp-server/configuration.md#cli-flags) |
| **TOML file** | Persistent settings, drop-ins | [Configuration](mcp-server/configuration.md#toml-configuration) |
| **npx / releases** | No build step | [Installation](mcp-server/installation.md) |

List available toolsets:

```bash
./kubernetes-mcp-server --help | grep toolsets
```

---

## Example chat prompts (MCP tools)

Natural-language queries the server can answer with its tools:

```
List all pods not in Running or Completed state across the cluster.
```

```
Show Warning events from the last hour in openshift-etcd.
```

```
What is the status of all ClusterOperators?
```

```
Use mustgather tools on /path/to/extracted/must-gather-directory — show etcd health and firing alerts.
```

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Help text in MCP log, then disconnect | Startup failed — `tail -20 /tmp/kubernetes-mcp-server.log` |
| `stat ~/.kube/config: no such file` | Set `--kubeconfig` to an absolute path |
| Only basic tools | Add `--toolsets` with the toolsets you need |
| Permission errors | `oc auth can-i get pods -A` |

---

## Documentation

- [Build from Source](mcp-server/build-from-source.md)
- [Cursor Integration](mcp-server/cursor-integration.md)
- [Configuration](mcp-server/configuration.md)
- [Toolsets Reference](reference/toolsets.md)
- [FAQ](reference/faq.md)

---

## SRE agent demo

For MCP slash prompts (`/live-cluster-rca`), report templates, and team onboarding, switch to the
**[SRE Agent](sre-agent/index.md)** tab.

## Resources

- **Source:** [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Upstream:** [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- **Protocol:** [modelcontextprotocol.io](https://modelcontextprotocol.io)
