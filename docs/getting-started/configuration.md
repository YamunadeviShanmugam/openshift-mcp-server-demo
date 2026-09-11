# Configuration Guide

Configure OpenShift MCP Server for your specific client and requirements.

## Configuration Files

### Cursor IDE

File: `~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "npx",
      "args": ["-y", "kubernetes-mcp-server@latest", "--read-only"]
    }
  }
}
```

### Claude Desktop

File: `~/.config/claude.json`

```json
{
  "mcpServers": {
    "kubernetes": {
      "command": "npx",
      "args": ["-y", "kubernetes-mcp-server@latest", "--read-only"]
    }
  }
}
```

### VS Code Insiders

```bash
code-insiders --add-mcp '{"name":"kubernetes","command":"npx","args":["kubernetes-mcp-server@latest"]}'
```

## Advanced Configuration

### TOML Configuration File

Create `~/.kube/mcp-config.toml`:

```toml
log_level = 1
read_only = true
kubeconfig = "/home/user/.kube/config"
list_output = "yaml"
toolsets = ["core", "config", "helm", "tekton", "openshift"]

# Security
[[denied_resources]]
group = ""
version = "v1"
kind = "Secret"

[[denied_resources]]
group = ""
version = "v1"
kind = "ConfigMap"

# Observability
[telemetry]
enabled = true
endpoint = "http://localhost:4317"
```

Run with:
```bash
npx kubernetes-mcp-server@latest --config ~/.kube/mcp-config.toml
```

### Environment Variables

```bash
export KUBECONFIG="/home/user/.kube/config"
export K8S_MCP_LOG_LEVEL="2"
export K8S_MCP_READ_ONLY="true"
```

## Security Configuration

### Read-Only Mode (Recommended)

Prevents accidental modifications:

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "npx",
      "args": ["-y", "kubernetes-mcp-server@latest", "--read-only"]
    }
  }
}
```

### TLS/HTTPS (Production)

```toml
port = "8080"
tls_cert = "/etc/tls/mcp-server.crt"
tls_key = "/etc/tls/mcp-server.key"
require_tls = true
```

### Resource Restrictions

Deny access to sensitive resources:

```toml
[[denied_resources]]
group = ""
version = "v1"
kind = "Secret"

[[denied_resources]]
group = ""
version = "v1"
kind = "ConfigMap"

[[denied_resources]]
group = "rbac.authorization.k8s.io"
version = "v1"
kind = "ClusterRole"
```

## Toolset Configuration

Enable specific features:

```toml
toolsets = [
  "core",
  "config",
  "helm",
  "tekton",
  "openshift",
  "observability/metrics",
  "observability/logs",
  "observability/traces"
]
```

## Next Steps

- [Quick Start](quickstart.md)
- [Installation Guide](installation.md)
- [Cursor Integration](cursor-integration.md)

---

**Tips:** Start with `--read-only` and add other flags as needed!
