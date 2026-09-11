# Toolsets Reference

Comprehensive guide to all available OpenShift MCP Server toolsets and tools.

## Available Toolsets

The OpenShift MCP Server supports enabling or disabling specific groups of tools via the `--toolsets` command-line flag. The following toolsets are available:

### Core Toolsets (Default)

#### core ✓ (Default)
Most common tools for Kubernetes management (Pods, Generic Resources, Events, etc.)

### Cluster Management Toolsets

#### cluster-diagnostics
Tools for cluster diagnostics and troubleshooting
- Node debugging and execution
- Cluster health checks
- Resource diagnostics

#### openshift
OpenShift-specific tools for cluster management and troubleshooting
- Project/Namespace management
- OpenShift resources
- Cluster configuration

### Network & CNI Toolsets

#### cni-diagnostics
Tools for Container Network Interface (CNI) diagnostics and troubleshooting
- Network policy validation
- CNI plugin diagnostics
- Connectivity testing

#### ovn-kubernetes
OVN-Kubernetes CNI network troubleshooting tools
- OVN database queries
- Network flow analysis
- Interface diagnostics

#### netedge
NetEdge troubleshooting tools for OpenShift
- Edge node diagnostics
- Edge network analysis

#### netobserv
Network observability tools backed by the NetObserv console plugin API
- Network flows
- Network metrics
- Flow export

### Package & Config Management

#### helm
Tools for managing Helm charts and releases
- Chart installation/uninstallation
- Release management
- Values management

#### tekton
Tekton pipeline management tools for Pipelines, PipelineRuns, Tasks, TaskRuns
- Pipeline creation and execution
- Task management
- Pipeline run diagnostics

### OpenShift Ecosystem Toolsets

#### kubevirt
OpenShift Virtualization tools for managing virtual machines
- Virtual machine management
- VM troubleshooting
- VM migration
- See: [OpenShift Virtualization documentation](https://github.com/openshift/openshift-mcp-server/blob/main/docs/kubevirt.md)

#### ossm
Tools for managing OpenShift Service Mesh (OSSM)
- ServiceMeshControl Plane management
- VirtualService management
- Network policies
- See: [OSSM documentation](https://github.com/openshift/openshift-mcp-server/blob/main/docs/OSSM.md)

#### kcp
Manage kcp workspaces and multi-tenancy features
- Workspace creation and management
- Tenancy configuration
- Workspace diagnostics

#### oadp
OADP (OpenShift API for Data Protection) tools for managing Velero backups
- Backup management
- Restore operations
- Schedule configuration
- Disaster recovery planning

### Observability & Monitoring Toolsets

#### observability/metrics
Toolset for querying Prometheus and Alertmanager endpoints
- Metric queries
- Alert status
- Time-series analysis

#### observability/logs
Toolset for querying Loki logs
- Log queries
- Log filtering
- LogQL support

#### observability/traces
Distributed tracing tools for discovering Tempo instances
- Trace search and retrieval
- Trace attribute exploration
- Distributed tracing analysis

#### observability/otelcol
OpenTelemetry Collector configuration assistance
- Schema validation
- Component documentation
- Version management

## Core Tools Reference

### Node Management

**nodes_debug_exec** - Run commands on an OpenShift node using a privileged debug pod

Parameters:
- `command` (array, required) - Command to execute on the node
- `image` (string, optional) - Container image to use for debug pod
- Available utilities: systemctl, journalctl, ss, ip, ping, traceroute, nmap, ps, top, lsof, strace
- Host filesystem mounted at /host

Example:
```bash
nodes_debug_exec(command=['chroot', '/host', 'systemctl', 'status', 'kubelet'])
```

### Resource Management

**resources_create_or_update** - Create or update a Kubernetes resource via Server-Side Apply

Parameters:
- `manifest` - Complete desired resource state
- `apiVersion` - API version (e.g., v1, apps/v1)
- `kind` - Resource kind (e.g., Pod, Deployment, Service)

**resources_scale** - Get or update the scale of a Kubernetes resource

Parameters:
- `apiVersion` - API version
- `kind` - Resource kind
- `name` - Resource name
- `namespace` - Optional namespace
- `scale` - Optional new scale value

### Virtual Machine Troubleshooting

**vm_troubleshoot** - Diagnose OpenShift Virtualization VirtualMachine issues

Automatically detects:
- Missing StorageClasses
- Invalid PVC specifications
- Cloud-init configuration issues
- nodeSelector migration blockers
- Failed migrations
- Pod crashloops

Use this tool FIRST when users report:
- VM won't start
- Stuck in Provisioning state
- Crashlooping VMs
- Migration failures
- Unexpected VM behavior

## Configuration

### Via Environment Variables

```bash
export MCP_TOOLSETS="core,openshift,helm,kubevirt,observability/metrics,observability/logs"
```

### Via Command Line

```bash
openshift-mcp-server --toolsets core,openshift,helm,tekton,cluster-diagnostics
```

### Via Configuration File

```yaml
mcp:
  toolsets:
    - core
    - openshift
    - helm
    - kubevirt
    - cluster-diagnostics
    - observability/metrics
    - observability/logs
    - oadp
```

## Multi-Cluster Support

When multi-cluster support is enabled (default), all tools include an additional `context` argument to specify the Kubernetes context (cluster):

```bash
pod_list(context="production", namespace="default")
```

## Logging & Debugging

The server supports MCP logging capability with:
- Structured debug messages sent to clients
- Automatic error categorization by Kubernetes API
- Sensitive data redaction (tokens, keys, passwords, credentials)
- Appropriate severity levels for issues

## Best Practices

1. **Enable only needed toolsets** - Reduces context size and improves LLM accuracy
2. **Use dedicated parameters** - Prefer namespace parameter for scoping
3. **Leverage list tools** - Use namespace/pod/deployment lists to discover filters
4. **Monitor tool usage** - Track what's being called in multi-tenant environments
5. **Test in dev first** - Before enabling in production
6. **Review RBAC** - Ensure service accounts have appropriate permissions
7. **Use multi-cluster carefully** - Test context parameters before production use

## Troubleshooting

### Toolset Not Available

Verify it's enabled:
```bash
# Check enabled toolsets in configuration
openshift-mcp-server --help | grep toolsets
```

### Permission Denied

Check RBAC for service account:
```bash
kubectl auth can-i get pods --as=system:serviceaccount:default:mcp-server
```

### Tools Timeout

Increase timeout in configuration:
```yaml
timeout: 30  # seconds
```

## More Information

- Official Repository: [openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- Full Documentation: [GitHub Docs](https://github.com/openshift/openshift-mcp-server/tree/main/docs)
- Kubernetes MCP Base: [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
