# Multi-Cluster Management

Manage multiple Kubernetes/OpenShift clusters with OpenShift MCP Server.

## Multi-Cluster Setup

### Prerequisites

- Multiple cluster kubeconfigs
- Combined in `~/.kube/config`
- Multiple contexts defined

### Configuration

```toml
kubeconfig = "/home/user/.kube/config"
cluster_provider_strategy = "kubeconfig"
```

### List Clusters

```
"Show all available clusters and their status"
```

## Switching Contexts

### Query Specific Cluster

```
"In the production cluster, show me:
1. Node count
2. Pod status
3. Recent errors"
```

### Compare Clusters

```
"Compare clusters:
1. Production vs staging resources
2. Configuration differences
3. Deployment status across clusters"
```

## Multi-Cluster Operations

### Cross-Cluster Deployment

```
"Deploy application to all clusters:
1. Show current deployments
2. Verify configurations match
3. Check health on each cluster"
```

### Disaster Recovery

```
"Failover from primary to secondary:
1. Primary cluster status
2. Secondary cluster readiness
3. Switch DNS/LB to secondary
4. Verify traffic"
```

## Best Practices

- Standardize cluster naming
- Keep kubeconfigs updated
- Document cluster purposes
- Monitor all clusters
- Test cross-cluster procedures

## Next Steps

- [Security Best Practices](security.md)
- [Advanced Topics](../advanced/custom-toolsets.md)

---

**Key Insight:** Multi-cluster is not complexity - it's flexibility!
