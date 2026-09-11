# Must-Gather Analysis

Analyze and troubleshoot clusters using must-gather bundles with AI-powered diagnostics.

## What is Must-Gather?

Must-gather is a diagnostic tool that collects comprehensive cluster information including:
- Pod logs and events
- Node status and metrics
- API server information
- Network configuration
- Storage status
- Custom resources

## Analysis Scenarios

### Scenario 1: Quick Diagnosis

**Ask:** "Analyze this must-gather bundle and find issues"

MCP performs:
- Extract and parse bundle
- Identify errors and warnings
- Suggest root causes
- Provide fix recommendations

### Scenario 2: Specific Problem

**Ask:** "Why did my cluster crash? Check the must-gather"

MCP performs:
- Search for error patterns
- Trace event timeline
- Analyze pod states
- Link cause and effect

### Scenario 3: Performance Analysis

**Ask:** "Why was cluster slow? Analyze must-gather"

MCP performs:
- Extract metrics
- Find resource bottlenecks
- Identify slow components
- Suggest optimizations

### Scenario 4: Security Review

**Ask:** "Check for security issues in must-gather"

MCP performs:
- Scan RBAC configuration
- Check network policies
- Review pod security
- Find vulnerabilities

## Quick Commands

| Command | Result |
|---------|--------|
| "Analyze must-gather.tar" | Full diagnostics |
| "Find errors in bundle" | Error analysis |
| "Pod failure root cause" | Failure analysis |
| "Timeline of events" | Event sequence |

## How to Generate Must-Gather

```bash
# Generate bundle
oc adm must-gather --dest-dir=./must-gather

# For specific component
oc adm must-gather -- /usr/bin/gather_sdn

# For network diagnostics
oc adm must-gather -- /usr/bin/gather_network_logs
```

## Analysis Workflow

1. **Generate** - Run must-gather on cluster
2. **Extract** - Decompress tar.gz bundle
3. **Upload** - Provide to MCP via Cursor
4. **Analyze** - Ask MCP for diagnostics
5. **Fix** - Implement recommendations

## What Gets Analyzed

### Pod & Container Info
- Pod status and events
- Container logs
- Resource usage
- Restart counts

### Node Information
- Node status
- Resource allocation
- Kernel logs
- System metrics

### Cluster Metadata
- API versions
- CRD definitions
- RBAC configuration
- Network setup

### Events & Logs
- Cluster events
- Warning/error messages
- Recent changes
- Audit logs

## Toolsets Used

- `cluster-diagnostics` - Bundle analysis
- `core` - Resource inspection
- `observability/logs` - Log analysis

## Common Issues Found

| Issue | Detection | Solution |
|-------|-----------|----------|
| Pod crash | Log analysis | Check image/config |
| Node down | Status check | Review kubelet logs |
| Network problem | Event review | Check CNI config |
| Resource exhaustion | Metrics analysis | Scale up nodes |

## Best Practices

1. Generate regularly during issues
2. Archive bundles for reference
3. Share with support teams
4. Compare bundles over time
5. Use for post-mortem analysis

---

**Navigation:** [Advanced Topics](api-reference.md)
