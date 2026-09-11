# Installation Guide

Complete guide to installing OpenShift MCP Server in different environments.

## System Requirements

- **OS**: Linux, macOS, or Windows (with WSL)
- **Kubernetes**: 1.20+ or OpenShift 4.8+
- **Network**: Access to Kubernetes API server
- **Credentials**: Valid kubeconfig file with cluster access

## Installation Methods

### Method 1: npm (Recommended)

Simplest method - works across all platforms.

```bash
# Install globally
npm install -g openshift-mcp-server

# Or run directly
npx -y openshift-mcp-server@latest
```

**Advantages:**
- No system dependencies
- Easy to update
- Works on macOS, Linux, Windows

### Method 2: Native Binary

Download pre-built binaries from GitHub releases.

```bash
# Download latest release
wget https://github.com/openshift/openshift-mcp-server/releases/download/latest/openshift-mcp-server-linux-x86_64
chmod +x openshift-mcp-server-linux-x86_64

# Run
./openshift-mcp-server-linux-x86_64 --toolsets core,openshift
```

**Advantages:**
- No dependencies
- Fastest startup
- Can be placed anywhere

### Method 3: Docker

Run in a container for isolation and consistency.

```bash
docker run -v ~/.kube/config:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  -p 8080:8080 \
  ghcr.io/openshift/openshift-mcp-server:latest \
  --port 8080
```

**Advantages:**
- Isolated environment
- Consistent across machines
- Easy to manage

### Method 4: Python (uvx)

Alternative for Python environments.

```bash
uvx openshift-mcp-server --toolsets core,openshift
```

## Integration with Cursor/Claude

### Cursor IDE Configuration

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "openshift-mcp-server": {
      "command": "npx",
      "args": ["-y", "openshift-mcp-server@latest"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      },
      "description": "OpenShift MCP Server - 18+ toolsets for cluster management"
    }
  }
}
```

### Claude Desktop Configuration

Edit `~/.config/claude.json`:

```json
{
  "mcpServers": {
    "openshift": {
      "command": "npx",
      "args": ["-y", "openshift-mcp-server@latest"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      }
    }
  }
}
```

## Configuring Toolsets

Enable specific toolsets based on your needs.

### Basic Setup

```bash
openshift-mcp-server --toolsets core,openshift
```

### SRE Full Stack

```bash
openshift-mcp-server --toolsets \
  core,openshift,cluster-diagnostics,helm,tekton,oadp,kubevirt, \
  observability/metrics,observability/logs,observability/traces
```

### Network Diagnostics

```bash
openshift-mcp-server --toolsets \
  core,cni-diagnostics,ovn-kubernetes,netobserv,netedge
```

### Virtualization

```bash
openshift-mcp-server --toolsets core,kubevirt,helm
```

### Service Mesh

```bash
openshift-mcp-server --toolsets core,openshift,ossm,helm
```

## Configuration File

Create a config file for persistent settings:

### config.yaml

```yaml
server:
  address: 127.0.0.1
  port: 8080
  sse-base-url: https://example.com:8080

kubeconfig:
  path: ~/.kube/config

toolsets:
  - core
  - openshift
  - cluster-diagnostics
  - helm
  - kubevirt
  - observability/metrics
  - observability/logs
  - oadp

logging:
  level: info
  format: json
```

Run with config:

```bash
openshift-mcp-server --config config.yaml
```

## Environment Variables

Override configuration with environment variables:

```bash
# Kubeconfig location
export KUBECONFIG=~/.kube/config

# Server configuration
export MCP_PORT=8080
export MCP_ADDRESS=127.0.0.1

# Toolsets
export MCP_TOOLSETS="core,openshift,cluster-diagnostics,helm"

# Logging
export MCP_LOG_LEVEL=info

# Start server
openshift-mcp-server
```

## Multi-Cluster Setup

Configure multiple clusters for seamless switching.

### ~/.kube/config

```yaml
apiVersion: v1
clusters:
  - cluster:
      server: https://api.production.example.com:6443
    name: production
  - cluster:
      server: https://api.staging.example.com:6443
    name: staging
contexts:
  - context:
      cluster: production
      user: admin
    name: production-admin
  - context:
      cluster: staging
      user: developer
    name: staging-dev
current-context: production-admin
users:
  - name: admin
    user:
      token: <token>
  - name: developer
    user:
      token: <token>
```

Use in queries:

```
"Show me cluster health for both production and staging clusters"
```

The server automatically detects contexts and allows you to specify which cluster to use.

## Troubleshooting Installation

### "Command not found"

If `openshift-mcp-server` command not found:

```bash
# For npm installation
npm install -g openshift-mcp-server
npm list -g openshift-mcp-server

# For binary
sudo mv openshift-mcp-server-linux-x86_64 /usr/local/bin/openshift-mcp-server
which openshift-mcp-server
```

### "Cannot connect to cluster"

Verify kubeconfig:

```bash
# Check kubeconfig
kubectl config view

# Test connection
kubectl get nodes

# Set correct kubeconfig
export KUBECONFIG=~/.kube/config
```

### "Toolsets not working"

Check available toolsets:

```bash
openshift-mcp-server --help
```

Verify toolset is available for your cluster:

```bash
# For Virtualization
kubectl get crd virtualmachines.kubevirt.io

# For OSSM
kubectl get ns openshift-operators

# For OADP
kubectl get crd backups.velero.io
```

### "Port already in use"

Use different port:

```bash
openshift-mcp-server --port 8081
```

### "Permission denied"

Check RBAC permissions:

```bash
kubectl auth can-i get pods --all-namespaces
kubectl auth can-i get nodes
kubectl auth can-i list events --all-namespaces
```

## Verifying Installation

```bash
# Check version
openshift-mcp-server --version

# Check available toolsets
openshift-mcp-server --help | grep toolsets

# Test cluster access
kubectl get nodes

# Verify kubeconfig
kubectl cluster-info
```

## Next Steps

- Configure for [Cursor Integration](cursor-integration.md)
- Explore [SRE Workflows](../workflows/cluster-health.md)
- Review [Configuration Reference](configuration.md)
- Check [Toolsets Guide](../reference/toolsets.md)

---

**Ready?** → [Quick Start Guide](quickstart.md)
