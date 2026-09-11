# Cursor Integration

Step-by-step guide to integrate OpenShift MCP Server with Cursor IDE.

## Prerequisites

- Cursor IDE installed
- Node.js v14+ (for npm)
- Access to Kubernetes/OpenShift cluster
- `~/.kube/config` file

## Quick Setup (2 minutes)

### Step 1: Edit MCP Configuration

Open `~/.cursor/mcp.json` (create if doesn't exist):

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

### Step 2: Restart Cursor

Close and reopen Cursor IDE for changes to take effect.

### Step 3: Verify Connection

Open Cursor's command palette (Cmd+K on macOS, Ctrl+K on Linux/Windows) and look for MCP servers status.

### Step 4: Start Using

In Cursor chat, try:
```
"List all namespaces in my cluster"
```

## Using in Cursor

### Example 1: Quick Cluster Health Check

```
"Give me a quick health check of my Kubernetes cluster:
1. How many nodes are Ready?
2. Are there any pods in CrashLoopBackOff?
3. Show me any Warning or Error events from the last hour"
```

### Example 2: Troubleshoot a Failing Deployment

```
"My deployment 'web-server' in the 'production' namespace is failing.
Help me debug:
1. Show pod status and replica count
2. Get the last 50 lines of pod logs
3. Show any relevant events
4. Based on the logs, what's likely causing the failure?"
```

### Example 3: Security Audit

```
"Audit RBAC in my cluster:
1. List all service accounts with cluster-admin role
2. Show any pods running as root
3. List network policies (or lack thereof)
4. Identify risky configurations"
```

### Example 4: Performance Analysis

```
"Analyze cluster performance:
1. Show nodes with highest CPU/memory usage
2. List pods being throttled
3. Check API server response times
4. Recommend optimizations"
```

## Advanced Usage

### Multi-Cluster Support

If you have multiple clusters in your `~/.kube/config`:

```
"Switch to cluster 'production' and show me:
1. Node count
2. Pod status
3. Any recent errors"
```

The MCP server will automatically detect and support all clusters in your kubeconfig.

### Custom Prompts

Save frequently used prompts:

```
# Save in a text file for reuse
Prompt: Daily Health Check
"Generate a daily report:
1. Cluster health summary
2. Resource utilization
3. Pod status distribution
4. Any issues detected
5. Recommended actions"
```

### Combining with Other Tools

Use Cursor's MCP integration with other tools:

```
"Using the Kubernetes MCP server:
1. Get all pods in CrashLoopBackOff
2. For each, show the last 10 lines of logs
3. Generate a debugging checklist"
```

## Troubleshooting

### "MCP Server Not Connecting"

**Solution:**
1. Verify `~/.cursor/mcp.json` exists and is valid JSON
2. Check kubeconfig: `kubectl get namespaces`
3. Restart Cursor IDE
4. Check Cursor debug output

### "Permission Denied"

**Solution:**
1. Verify cluster access: `kubectl auth can-i get pods --all-namespaces`
2. Check kubeconfig path
3. Ensure kubeconfig user has permissions

### "Command not found: npx"

**Solution:**
1. Install Node.js: https://nodejs.org/
2. Verify installation: `node --version`
3. Restart terminal

## Tips & Tricks

### Use Context in Queries

```
"In the production cluster, check if deployment 'api-server' is healthy"
```

### Break Down Complex Queries

Instead of one huge query:
```
"Tell me everything about the cluster"
```

Use multiple focused queries:
```
"List all nodes and their status"
"Show pods in error state"
"Identify resource-constrained nodes"
```

### Save Useful Prompts

Create a collection of useful prompts for your team:

```markdown
# Common SRE Queries

## Quick Health Check
"Show cluster health: nodes ready, pods running, recent errors"

## Incident Debug
"Diagnose why this pod [POD] is failing: show logs, events, resources"

## Capacity Planning
"Analyze cluster capacity: utilization, growth rate, upgrade needs"

## Security Audit
"RBAC audit: show cluster-admin users, privileged pods, open policies"
```

## Best Practices

- **Always use `--read-only`** to prevent accidental modifications
- **Start with simple queries** before complex ones
- **Share useful prompts** with your team
- **Document your queries** for reproducibility
- **Regularly test** to ensure integration works

## Next Steps

- [SRE Workflows](../workflows/cluster-health.md)
- [Troubleshooting Guide](troubleshooting.md)
- [Advanced Topics](../advanced/multi-cluster.md)

---

**Happy debugging!** 🚀
