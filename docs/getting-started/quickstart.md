# Quick Start Guide

Get up and running with OpenShift MCP Server in 5 minutes.

## Prerequisites

- Access to a Kubernetes or OpenShift cluster
- `kubeconfig` file (typically at `~/.kube/config`)
- Cursor IDE, Claude Desktop, or VS Code installed

## 5-Minute Setup

### Step 1: Install the MCP Server (1 min)

Choose your preferred method:

```bash
# Fastest method - using npm
npx -y kubernetes-mcp-server@latest --read-only
```

That's it! The server will auto-download and start.

### Step 2: Configure Your Client (2 min)

=== "Cursor IDE"

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

=== "Claude Desktop"

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

=== "VS Code Insiders"

    ```bash
    code-insiders --add-mcp '{"name":"kubernetes","command":"npx","args":["kubernetes-mcp-server@latest"]}'
    ```

### Step 3: Verify Installation (1 min)

```bash
npx kubernetes-mcp-server --help
```

You should see the help output without errors.

### Step 4: Start Using (1 min)

In your AI client (Cursor, Claude, etc.), ask:

```
"List all namespaces in my cluster"
```

Or try:

```
"Show me all pods in the default namespace that are not Running"
```

## ✅ Verification Checklist

- [ ] `npx kubernetes-mcp-server --help` runs without errors
- [ ] Configuration file is in the correct location
- [ ] `kubectl get namespaces` works (proves kubeconfig is accessible)
- [ ] MCP server appears in your client's MCP servers list
- [ ] You can query cluster information successfully

## 🎯 First SRE Workflow: Quick Health Check

Try these commands to validate your setup:

```
"Give me a quick health check of my cluster:
1. How many nodes do we have?
2. Are there any nodes with high resource utilization?
3. Are there any pods in error or pending state?"
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

### "Permission denied"

Check your cluster permissions:

```bash
kubectl auth can-i get pods --all-namespaces
```

## 📚 Next Steps

- **Learn SRE Workflows**: [Cluster Health Monitoring](../workflows/cluster-health.md)
- **Advanced Configuration**: [Configuration Guide](configuration.md)
- **Production Setup**: [Security Best Practices](../advanced/security.md)

## 💡 Tips

- **Always use `--read-only`** in development and production to prevent accidental modifications
- **Test with safe queries first** like `kubectl get namespaces` equivalent
- **Keep your kubeconfig secure** - don't commit it to version control
- **For multi-cluster**, ensure your `~/.kube/config` has multiple contexts

---

**All set?** → [Explore SRE Workflows](../workflows/cluster-health.md)
