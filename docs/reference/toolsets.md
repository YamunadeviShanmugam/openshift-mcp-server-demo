# Toolsets Reference

Comprehensive guide to all available toolsets and tools.

## Default Toolsets

### core

Most common Kubernetes tools

- Pod management
- Namespace operations
- Event viewing
- Node information

### config

Kubeconfig management

- View configuration
- List contexts
- Switch contexts

## Optional Toolsets

### helm

Helm chart management

- Install/uninstall charts
- List releases
- Show values

### tekton

Tekton pipeline management

- Start pipelines
- Monitor runs
- Show logs

### openshift

OpenShift-specific tools

- Project management
- OpenShift resources

### observability

Monitoring and tracing

- Prometheus queries
- Loki logs
- Jaeger traces

## Enabling Toolsets

```toml
toolsets = ["core", "config", "helm", "tekton", "openshift", "observability/metrics"]
```

## Tool Descriptions

### Pod Management Tools

**pods.list** - List pods in a namespace
```
Parameters:
  - namespace: Kubernetes namespace
  - selector: Label selector (optional)
```

**pods.get** - Get pod details
```
Parameters:
  - pod: Pod name
  - namespace: Kubernetes namespace
```

**pods.logs** - Retrieve pod logs
```
Parameters:
  - pod: Pod name
  - namespace: Kubernetes namespace
  - container: Container name (optional)
  - previous: Get previous logs (optional)
```

### Namespace Tools

**namespaces.list** - List all namespaces
```
Returns all namespaces with their status
```

**namespaces.get** - Get namespace details
```
Parameters:
  - namespace: Namespace name
```

### Node Tools

**nodes.list** - List all cluster nodes
```
Returns nodes with status and resource info
```

**nodes.get** - Get node details
```
Parameters:
  - node: Node name
```

**nodes.top** - Get node resource usage
```
Returns CPU and memory metrics for nodes
```

## Configuration

### Via Environment Variables

```bash
export MCP_TOOLSETS="core,config,helm"
```

### Via Configuration File

```yaml
mcp:
  toolsets:
    - core
    - config
    - helm
    - observability
```

## Best Practices

1. **Enable only needed toolsets** - Reduce resource usage
2. **Use RBAC constraints** - Limit tool access
3. **Monitor tool usage** - Track what's being called
4. **Keep toolsets updated** - Ensure latest features
5. **Test in dev first** - Before production use

## Troubleshooting

### Toolset Not Available

Check if it's enabled:
```bash
kubectl get toolsets
```

Enable it in configuration and restart MCP server.

### Permission Denied

Verify RBAC permissions:
```bash
kubectl auth can-i get pods --as=system:serviceaccount:default:mcp-server
```

### Tools Timeout

Increase timeout values:
```toml
timeout = 30  # seconds
```

## More Information

Full reference at: [containers/kubernetes-mcp-server/docs](https://github.com/containers/kubernetes-mcp-server/blob/main/docs/)
