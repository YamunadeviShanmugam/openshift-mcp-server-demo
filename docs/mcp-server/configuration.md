# Configuration

Configure **kubernetes-mcp-server** via CLI flags, TOML, or environment variables.

Full upstream reference:
[containers/kubernetes-mcp-server — configuration.md](https://github.com/containers/kubernetes-mcp-server/blob/main/docs/configuration.md)

## CLI flags

Configure via `~/.cursor/mcp.json`:

```json
"args": [
  "--port", "",
  "--kubeconfig", "/ABSOLUTE/PATH/to/kubeconfig",
  "--toolsets", "core,config,openshift,cluster-diagnostics,cni-diagnostics,ovn-kubernetes",
  "--list-output", "yaml",
  "--log-file", "/tmp/kubernetes-mcp-server.log"
]
```

### What flags cover

| Setting | Flag |
|---------|------|
| Toolsets | `--toolsets` (comma-separated) |
| Kubeconfig | `--kubeconfig` |
| List output format | `--list-output yaml\|table` |
| Read-only mode | `--read-only` |
| Disable destructive tools | `--disable-destructive` |
| Cluster provider | `--cluster-provider kubeconfig` |

### What flags do **not** cover

| Setting | Requires TOML |
|---------|---------------|
| `denied_resources` | Yes |
| `disabled_tools` | Yes |
| MCP prompts | Yes (`[[prompts]]` in TOML) |
| Server instructions | Yes |

## TOML configuration

Create a config file (e.g. `~/.kube/mcp-config.toml`):

```toml
read_only = false
list_output = "yaml"

toolsets = [
  "core",
  "config",
  "openshift",
  "cluster-diagnostics",
]

[[denied_resources]]
group = ""
version = "v1"
kind = "Secret"
```

Cursor `mcp.json`:

```json
"args": [
  "--port", "",
  "--kubeconfig", "/ABSOLUTE/PATH/to/kubeconfig",
  "--config", "/ABSOLUTE/PATH/to/mcp-config.toml",
  "--log-file", "/tmp/kubernetes-mcp-server.log"
]
```

!!! warning "Kubeconfig paths in TOML"
    Use **absolute paths** in TOML (`kubeconfig = "/home/user/.kube/config"`).
    The server does **not** expand `~` in TOML values.
    When both TOML and `--kubeconfig` are set, the **CLI flag wins**.

## Kubeconfig resolution order

1. `--kubeconfig` flag (if passed on CLI)
2. `kubeconfig = "..."` in TOML (if `--config` is used)
3. `KUBECONFIG` environment variable (if no explicit path above)
4. Default: `~/.kube/config` (expanded by client-go when using defaults)

## Toolsets

Default without configuration: `core`, `config`.

List all available toolsets:

```bash
./kubernetes-mcp-server --help | grep toolsets
```

Example SRE-oriented set:

```
core,config,openshift,cluster-diagnostics,openshift/mustgather,cni-diagnostics,ovn-kubernetes
```

See [Toolsets Reference](../reference/toolsets.md).

## Environment variables

| Variable | Purpose |
|----------|---------|
| `KUBECONFIG` | Kubeconfig path (when no `--kubeconfig` / TOML path) |
| `K8S_MCP_CONFIG_PATH` | Default config file if `--config` omitted |

## SRE agent TOML

The team demo uses `agents/sre/sre-agent.toml` plus `conf.d/` drop-ins for prompts and
report instructions. See the **[SRE Agent](../sre-agent/index.md)** tab.
