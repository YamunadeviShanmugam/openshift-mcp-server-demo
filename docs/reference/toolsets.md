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

---

Full reference at: [containers/kubernetes-mcp-server/docs](https://github.com/containers/kubernetes-mcp-server/blob/main/docs/)
