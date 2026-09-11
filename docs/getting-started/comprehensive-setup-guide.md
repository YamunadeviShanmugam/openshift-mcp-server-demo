# OpenShift MCP Server Configuration Guide

A comprehensive guide to configure the **Kubernetes/OpenShift MCP Server** for use with Cursor, Claude, VS Code, and other AI tools.

## 📚 Table of Contents

1. [What is the Kubernetes MCP Server?](#what-is-the-kubernetes-mcp-server)
2. [Quick Start](#quick-start)
3. [Installation Methods](#installation-methods)
4. [Client Configuration](#client-configuration)
5. [Security & Best Practices](#security--best-practices)
6. [Advanced Configuration](#advanced-configuration)
7. [Troubleshooting](#troubleshooting)
8. [Resources](#resources)

---

## What is the Kubernetes MCP Server?

The **Kubernetes MCP Server** is a native Go-based Model Context Protocol server that enables AI assistants to interact with Kubernetes and OpenShift clusters directly through natural language.

### Key Benefits

- **Native Go Implementation**: Directly interacts with Kubernetes API (no kubectl wrapper)
- **No External Dependencies**: Single binary or npm package, no need for kubectl, helm, etc.
- **Multi-Cluster Support**: Manage multiple clusters simultaneously using kubeconfig contexts
- **Comprehensive Tools**: Pods, namespaces, resources, Helm, Tekton, observability, and more
- **High Performance**: Direct API calls without subprocess overhead
- **Enterprise Ready**: Security, TLS, OAuth, read-only mode, resource access control

### Repository

- **GitHub**: https://github.com/containers/kubernetes-mcp-server
- **Documentation**: https://github.com/containers/kubernetes-mcp-server/blob/main/docs/

---

## Quick Start

### 1. Prerequisites

You need:
- Access to a Kubernetes or OpenShift cluster
- A `kubeconfig` file (typically at `~/.kube/config`)
- OR in-cluster ServiceAccount (if running inside a pod)

### 2. Choose Installation Method

**Fastest (npm):**
```bash
npx -y kubernetes-mcp-server@latest --read-only
```

**Python alternative (uvx):**
```bash
uvx kubernetes-mcp-server --read-only
```

**Or download native binary** from [GitHub Releases](https://github.com/containers/kubernetes-mcp-server/releases)

### 3. Configure Your Client

#### For Cursor IDE

Edit `~/.cursor/mcp.json`:

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

#### For Claude Desktop

Edit `~/.config/claude.json`:

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

#### For VS Code Insiders

Run:
```bash
code-insiders --add-mcp '{"name":"kubernetes","command":"npx","args":["kubernetes-mcp-server@latest"]}'
```

### 4. Verify Connection

```bash
# Test the MCP server
npx kubernetes-mcp-server --help

# Or with your client
# In Cursor: Check that the MCP server appears in the MCP servers list
# In Claude Desktop: Try asking "List all namespaces in my cluster"
```

---

## Installation Methods

### npm (Recommended)

**Best for:** Most users, easiest setup

```bash
# Install globally (optional)
npm install -g kubernetes-mcp-server

# Or use npx (recommended - always runs latest)
npx -y kubernetes-mcp-server@latest --read-only
```

**Pros:**
- Automatic updates
- No local installation needed
- Works on macOS, Linux, Windows
- Easy to add to config files

**Cons:**
- Requires Node.js and npm

### uvx (Python)

**Best for:** Python-focused environments

```bash
# Requires 'uv' - Fast Python package installer
uv pip install kubernetes-mcp-server
uvx kubernetes-mcp-server --read-only
```

**Pros:**
- Python ecosystem
- Similar to npx but for Python
- Isolated environment

**Cons:**
- Requires Python and uv

### Native Binary

**Best for:** Minimal dependencies, production deployment

```bash
# Download from releases
wget https://github.com/containers/kubernetes-mcp-server/releases/download/v1.0.0/kubernetes-mcp-server-linux-x86_64
chmod +x kubernetes-mcp-server-linux-x86_64

# Run
./kubernetes-mcp-server-linux-x86_64 --read-only
```

**Pros:**
- No dependencies
- Fast startup
- Available for Linux, macOS, Windows

**Cons:**
- Manual updates needed

### Docker Container

**Best for:** Isolated, consistent environments, CI/CD

```bash
docker run -v ~/.kube/config:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  -p 8080:8080 \
  ghcr.io/containers/kubernetes-mcp-server:latest \
  --port 8080 --read-only
```

**Pros:**
- Complete isolation
- Consistent across machines
- Easy to deploy

**Cons:**
- Docker overhead
- Port exposure needed

---

## Client Configuration

### Cursor IDE Configuration

**File:** `~/.cursor/mcp.json`

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

### Claude Desktop Configuration

**File:** `~/.config/claude.json`

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

### VS Code Configuration

**Option 1: Command Line**
```bash
code-insiders --add-mcp '{"name":"kubernetes","command":"npx","args":["kubernetes-mcp-server@latest"]}'
```

**Option 2: Manual Edit** (VS Code Insiders config file location varies by OS)

### Cursor Code CLI Configuration

**File:** `~/.config/claude-code/config.toml`

```toml
[[mcp_servers]]
name = "kubernetes-mcp-server"
command = "npx"
args = [
    "-y",
    "kubernetes-mcp-server@latest",
    "--read-only"
]

[mcp_servers.env]
KUBECONFIG = "/home/USERNAME/.kube/config"
```

Then test with:
```bash
claude mcp list
```

---

## Security & Best Practices

### 1. Read-Only Mode (Recommended)

Prevents accidental modifications:

```bash
npx kubernetes-mcp-server@latest --read-only
```

Or in config:
```json
{
  "args": ["-y", "kubernetes-mcp-server@latest", "--read-only"]
}
```

### 2. Restrict Sensitive Resources

Create a TOML config file to deny access to Secrets and ConfigMaps:

**`~/.kube/mcp-config.toml`:**
```toml
read_only = true

[[denied_resources]]
group = ""
version = "v1"
kind = "Secret"

[[denied_resources]]
group = ""
version = "v1"
kind = "ConfigMap"
```

Then run with:
```bash
npx kubernetes-mcp-server@latest --config ~/.kube/mcp-config.toml
```

### 3. Enable TLS/HTTPS

For production deployments:

```toml
# config.toml
port = "8080"
tls_cert = "/path/to/cert.pem"
tls_key = "/path/to/key.pem"
require_tls = true
```

### 4. Use ServiceAccount for In-Cluster

If running inside a pod, use a dedicated ServiceAccount:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: mcp-viewer
  namespace: default

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: mcp-viewer
rules:
- apiGroups: [""]
  resources: ["pods", "namespaces", "events"]
  verbs: ["get", "list", "watch"]
- apiGroups: [""]
  resources: ["services", "configmaps"]
  verbs: ["get", "list"]
- apiGroups: ["apps"]
  resources: ["deployments", "statefulsets", "daemonsets"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: mcp-viewer-binding
subjects:
- kind: ServiceAccount
  name: mcp-viewer
  namespace: default
roleRef:
  kind: ClusterRole
  name: mcp-viewer
  apiGroup: rbac.authorization.k8s.io
```

### 5. Rate Limiting

Prevent abuse:

```toml
[http]
rate_limit_rps = 10  # 10 requests per second per session
rate_limit_burst = 20
```

---

## Advanced Configuration

### Enable Specific Toolsets

By default, only `core` and `config` toolsets are enabled. Enable additional features:

```bash
# Via CLI
npx kubernetes-mcp-server@latest \
  --toolsets core,config,helm,tekton,openshift,observability/metrics

# Via TOML
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

### Available Toolsets

| Toolset | Description | Default |
|---------|-------------|---------|
| `core` | Pods, namespaces, events, nodes | ✓ |
| `config` | kubeconfig management | ✓ |
| `helm` | Helm charts and releases | |
| `tekton` | Tekton pipelines | |
| `openshift` | OpenShift-specific tools | |
| `openshift/mustgather` | Analyze must-gather archives | |
| `kubevirt` | Virtual machines | |
| `observability/metrics` | Prometheus queries | |
| `observability/logs` | Loki queries | |
| `observability/traces` | Distributed tracing (Tempo) | |
| `kiali` | Istio/Kiali integration | |

### TOML Configuration File

Create `~/.kube/mcp-config.toml`:

```toml
# Server settings
log_level = 1
kubeconfig = "/home/user/.kube/config"
cluster_provider_strategy = "kubeconfig"

# Security
read_only = true
list_output = "yaml"

# Features
toolsets = ["core", "config", "helm", "tekton", "openshift"]

# Resource restrictions
[[denied_resources]]
group = ""
version = "v1"
kind = "Secret"

# Observability (optional)
[telemetry]
enabled = true
endpoint = "http://localhost:4317"

# HTTP Security (for HTTP mode)
[http]
read_header_timeout = "10s"
max_body_bytes = 16777216
rate_limit_rps = 10
rate_limit_burst = 20
```

Run with:
```bash
npx kubernetes-mcp-server@latest --config ~/.kube/mcp-config.toml
```

### Multi-Cluster Support

The server auto-detects and supports multiple clusters from your kubeconfig. When you have multiple contexts, tools will include a `context` parameter to specify which cluster to use.

```bash
# List all available contexts
kubectl config get-contexts

# The MCP server will automatically support all of them
npx kubernetes-mcp-server@latest --read-only
```

### Dynamic Configuration Reload

Update configuration without restarting:

```bash
# Make changes to your config file
# Then send SIGHUP to reload
kill -HUP $(pgrep kubernetes-mcp-server)

# Or with pkill
pkill -HUP kubernetes-mcp-server
```

**Note:** Some settings like `port`, `kubeconfig`, and TLS require a restart.

---

## Troubleshooting

### MCP Server Not Connecting

**Check 1: Verify Installation**
```bash
npx kubernetes-mcp-server@latest --help
```

Should show help text without errors.

**Check 2: Test kubeconfig Access**
```bash
kubectl get namespaces
```

If this fails, your kubeconfig is not working.

**Check 3: Check MCP Server Health**

In Cursor, go to the MCP Servers view and check the connection status. Look for any error messages.

### "Connection Refused" Error

The MCP server is not running. Ensure you:
1. Haven't exited the terminal running the server
2. Are using the correct configuration file
3. Have the correct port specified (if using HTTP mode)

### "Permission Denied" Error

You need the correct permissions to access the cluster. Check:

```bash
# Can you access your cluster?
kubectl get pods --all-namespaces

# Do you have permission for the resource?
kubectl auth can-i get pods
```

### High Memory/CPU Usage

The MCP server should be lightweight. If you're experiencing high usage:

1. Disable unused toolsets:
   ```bash
   npx kubernetes-mcp-server@latest --toolsets core,config
   ```

2. Enable read-only mode if not already:
   ```bash
   npx kubernetes-mcp-server@latest --read-only
   ```

3. Check your cluster size - very large clusters with thousands of resources may take more memory

### "Unknown Flag" Error

You're using an old installation. Update:

```bash
# npm will auto-update with npx
npx -y kubernetes-mcp-server@latest --help

# If installed locally, update
npm update -g kubernetes-mcp-server

# Or reinstall
npm uninstall -g kubernetes-mcp-server
npm install -g kubernetes-mcp-server@latest
```

---

## Resources

### Official Documentation
- **GitHub Repository**: https://github.com/containers/kubernetes-mcp-server
- **Configuration Reference**: https://github.com/containers/kubernetes-mcp-server/blob/main/docs/configuration.md
- **Getting Started**: https://github.com/containers/kubernetes-mcp-server/blob/main/docs/getting-started-kubernetes.md
- **OpenShift Guide**: https://github.com/containers/kubernetes-mcp-server/blob/main/docs/openshift/user-guide.md

### Feature-Specific Guides
- **Helm Integration**: See `docs/` folder in repository
- **Tekton Integration**: See `docs/tekton.md`
- **Observability**: See `docs/observability/`
- **OpenShift Virtualization (KubeVirt)**: See `docs/kubevirt.md`
- **Service Mesh (OSSM)**: See `docs/OSSM.md`

### External Resources
- **Kubernetes Documentation**: https://kubernetes.io/docs/
- **OpenShift Documentation**: https://docs.openshift.com/
- **Model Context Protocol**: https://modelcontextprotocol.io/

---

## Example Use Cases

### 1. List Namespaces and Pods

Ask your AI assistant:
> "List all namespaces in my cluster, then show me pods in the kube-system namespace"

### 2. Troubleshoot Deployment

> "I have a deployment called 'my-app' in the default namespace that's not starting. Get me the pod logs and events to help diagnose the issue"

### 3. Get Resource Information

> "Show me all services in the default namespace and their endpoints"

### 4. Manage Helm Releases

> "List all Helm releases in all namespaces and show their versions"

### 5. Check Cluster Health

> "Show me node resource usage and any warning/error events across the cluster"

---

## Next Steps

1. **Choose your installation method** from the [Installation Methods](#installation-methods) section
2. **Configure your client** using the appropriate configuration for Cursor, Claude, or VS Code
3. **Verify the connection** by checking the MCP Servers panel
4. **Start using it** with natural language queries in your AI tool
5. **Review security options** and enable read-only mode or resource restrictions as needed
6. **Explore advanced features** like specific toolsets based on your needs

---

## Getting Help

If you encounter issues:

1. Check the [Troubleshooting](#troubleshooting) section
2. Review the [official documentation](https://github.com/containers/kubernetes-mcp-server/blob/main/docs/)
3. Open an issue on [GitHub](https://github.com/containers/kubernetes-mcp-server/issues)
4. Check existing GitHub discussions

---

**Happy Kubernetes managing with AI! 🚀**
