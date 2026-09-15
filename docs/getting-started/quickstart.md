# Quick Start Guide

Get up and running with OpenShift MCP Server in 5 minutes.

## Prerequisites

- Access to a Kubernetes or OpenShift cluster
- `kubeconfig` file (typically at `~/.kube/config`)
- Cursor IDE or Claude Desktop installed
- Node.js 14+ (for npm method)

## 5-Minute Setup

### Step 1: Install the MCP Server (1 min)

Choose your preferred method:

=== "npm (Recommended)"

```bash
npx -y openshift-mcp-server@latest \
  --toolsets core,openshift,cluster-diagnostics,helm,kubevirt,observability/metrics,observability/logs
```

=== "Native Binary"

```bash
# Download from GitHub releases
wget https://github.com/openshift/openshift-mcp-server/releases/download/v1.0.0/openshift-mcp-server-linux-x86_64
chmod +x openshift-mcp-server-linux-x86_64
./openshift-mcp-server-linux-x86_64
```

=== "Docker"

```bash
docker run -v ~/.kube/config:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  ghcr.io/openshift/openshift-mcp-server:latest
```

### Step 2: Configure Your Client (2 min)

=== "Cursor IDE"

Edit `~/.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "openshift-mcp-server": {
      "command": "npx",
      "args": ["-y", "openshift-mcp-server@latest"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      }
    }
  }
}
```

=== "Claude Desktop"

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

### Step 3: Verify Installation (1 min)

```bash
npx openshift-mcp-server@latest --version
```

You should see version information without errors.

### Step 4: Start Using (1 min)

In Cursor or Claude, ask:

```
"Show me the health of my OpenShift cluster:
- How many nodes?
- Any nodes with resource pressure?
- How many pods are in error states?
- What about namespace resource usage?"
```

Or try specific toolsets:

```
"List all OpenShift projects and show me deployment status"
```

```
"Troubleshoot why my VM is not starting - is it a storage issue or resource problem?"
```

```
"Show me Prometheus metrics for CPU usage across namespaces"
```

## ✅ Verification Checklist

- [ ] `npx openshift-mcp-server@latest --version` runs without errors
- [ ] Configuration file is in the correct location
- [ ] `kubectl get namespaces` works (proves kubeconfig is accessible)
- [ ] MCP server appears in your client's MCP servers list
- [ ] You can query cluster information successfully
- [ ] Toolsets are loading (check for any warnings)

## 🎯 First SRE Workflow: Quick Cluster Assessment

Try these commands to validate your setup:

```
"Give me a quick health check of my OpenShift cluster:
1. How many nodes and what's their status?
2. Are there nodes with memory or CPU pressure?
3. Which namespaces have the most pod activity?
4. Show me any pods in CrashLoopBackOff or Error states"
```

Or for VM management:

```
"List all VirtualMachines in the cluster and show me which ones are running"
```

Or for observability:

```
"Query Prometheus for the top 5 namespaces by CPU usage"
```

## 🆘 Troubleshooting

### "Command not found: npx"

Install Node.js from https://nodejs.org/

```bash
node --version  # Should be v14 or higher
npm --version   # Should be npm 6 or higher
```

### "Cannot connect to cluster"

Verify your kubeconfig:

```bash
kubectl get namespaces
# If this fails, your kubeconfig isn't accessible
```

### "Toolsets not loading"

Check which toolsets are available:

```bash
npx openshift-mcp-server@latest --help | grep toolsets
```

Enable specific toolsets:

```bash
npx openshift-mcp-server@latest \
  --toolsets core,openshift,cluster-diagnostics,helm,kubevirt
```

### "Permission denied"

Check your cluster permissions:

```bash
kubectl auth can-i get pods --all-namespaces
kubectl auth can-i get nodes
```

### "Kubeconfig not found"

Set the KUBECONFIG environment variable:

```bash
export KUBECONFIG=~/.kube/config
npx openshift-mcp-server@latest
```

## 📚 Next Steps

- **Learn SRE Workflows**: [Cluster Health Monitoring](../workflows/cluster-health.md)
- **Advanced Configuration**: [Configuration Guide](configuration.md)
- **Understand Toolsets**: [Toolsets Reference](../reference/toolsets.md)
- **Production Setup**: [Security Best Practices](../custom-tools-support/security.md)
- **Troubleshooting**: [Troubleshooting Guide](../reference/troubleshooting.md)

## 💡 Tips

- **Enable only needed toolsets** to reduce context size and improve AI accuracy
- **Test with safe queries first** like listing namespaces
- **Keep your kubeconfig secure** - don't commit it to version control
- **For multi-cluster**, ensure your `~/.kube/config` has multiple contexts
- **Use descriptive queries** for better AI understanding of what you need
- **Check the Toolsets Guide** for available capabilities

---

**All set?** → [Explore SRE Workflows](../workflows/cluster-health.md)
