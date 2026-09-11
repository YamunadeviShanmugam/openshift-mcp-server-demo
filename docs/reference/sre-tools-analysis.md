# SRE Tools Coverage Analysis

A comprehensive review of OpenShift MCP Server toolsets and important SRE tools.

## ✅ Currently Well-Documented

### Core & Cluster Management
- **core** - Pod, Namespace, Event management
- **cluster-diagnostics** - Node debugging, health checks
- **openshift** - Project/namespace, OpenShift resources
- **configuration** - Kubeconfig management

### Observability & Monitoring
- **observability/metrics** - Prometheus queries, alerting
- **observability/logs** - Loki log aggregation
- **observability/traces** - Tempo distributed tracing
- **observability/otelcol** - OpenTelemetry configuration

### Infrastructure & Deployment
- **helm** - Chart deployment & management
- **tekton** - Pipeline execution & troubleshooting
- **oadp** - Backup/restore with Velero

### OpenShift Ecosystem
- **kubevirt** - VM management & troubleshooting
- **ossm** - Service Mesh (Istio) management
- **kcp** - Multi-tenancy workspaces
- **config** - Configuration management

### Networking
- **cni-diagnostics** - CNI plugin troubleshooting
- **ovn-kubernetes** - OVN network analysis
- **netobserv** - Network flow monitoring
- **netedge** - Edge network diagnostics

## 🎯 Recommended Additions for SRE

### Advanced Must-Gather Analysis ✅
- See: [Must-Gather Analysis](../../advanced/must-gather.md)
- Could extend with automated parsing and historical analysis

### Resource Quota & Limit Management 📋
**Why Important:** Prevent cluster overprovisioning, enforce multi-tenant isolation

**Topics to cover:**
- Pod/Node resource limits
- Namespace quotas
- LimitRanges
- Resource requests/limits best practices

### RBAC & Security Audit 📋
**Why Important:** Access control, compliance, security hardening

**Topics to cover:**
- RBAC permission review
- Service account management
- Network policy audit
- Pod security policies

### Cost Optimization & Capacity Planning 📋
**Why Important:** Budget control, infrastructure planning

**Topics to cover:**
- Node utilization analysis
- Pod efficiency metrics
- Unused resource cleanup
- Capacity forecasting

### Multi-Cluster Failover & Load Balancing 📋
**Why Important:** High availability, disaster recovery

**Topics to cover:**
- ACM (Advanced Cluster Management) integration
- Cross-cluster traffic routing
- Failover procedures
- Load distribution

### Storage & Persistent Volume Management 📋
**Why Important:** Data persistence, backup verification

**Topics to cover:**
- PV/PVC health checks
- Storage class validation
- Snapshot management
- Volume migration

## 🔧 Advanced SRE Tools Available

### Plan Must-Gather
- Custom must-gather collection from specific nodes
- Component-specific gathering
- Custom timeout/since filtering
- **Use when:** Collecting diagnostic data for specific issues

### Multi-Cluster Context
- Manage multiple clusters via kubeconfig
- Context-aware tool execution
- **Use when:** Operating infrastructure across regions/environments

### Read-Only Mode
- Safe inspection without write permissions
- **Use when:** Auditing, investigating, preventing accidents

### Disable-Destructive Mode
- Prevent delete/update operations
- Allow create/read only
- **Use when:** Production access with safety guarantees

## 📊 SRE Workflow Coverage

| Workflow | Status | Tools Used |
|---|---|---|
| Cluster Health | ✅ | core, observability, cluster-diagnostics |
| Troubleshooting | ✅ | cluster-diagnostics, logs, metrics |
| Security & Compliance | ⚠️ | openshift, cni-diagnostics |
| Performance Tuning | ✅ | metrics, observability |
| Disaster Recovery | ✅ | oadp, kubernetes resources |
| Observability | ✅ | logs, metrics, traces |
| Incident Response | ✅ | All tools |
| Must-Gather Analysis | ✅ | cluster-diagnostics |
| Resource Management | ❌ | Need quota/limit tools |
| RBAC Auditing | ❌ | Need security audit tools |
| Cost Optimization | ❌ | Need capacity analysis |
| Multi-Cluster Ops | ⚠️ | ACM available but not documented |

## 🚀 Quick Reference: Tool Selection by Scenario

### Cluster Won't Start?
- cluster-diagnostics → node debugging
- observability/logs → error logs
- oadp → check recent changes from backup

### High CPU Usage?
- observability/metrics → Prometheus queries
- core → identify resource-hungry pods
- [Performance Tuning](../../workflows/performance-tuning.md) → optimization

### Network Connectivity Issue?
- cni-diagnostics → CNI validation
- ovn-kubernetes → network flow analysis
- netobserv → network metrics

### Backup Failed?
- oadp → backup status
- core → PVC health
- cluster-diagnostics → storage issues

### Security Concern?
- openshift → RBAC review
- cni-diagnostics → network policies
- [Must-Gather Analysis](../../advanced/must-gather.md) → audit logs

## 📝 Recommendations

### High Priority Additions
1. ✅ [Must-Gather Analysis](../../advanced/must-gather.md) - Already added
2. 📋 Resource Quota Management - Extend documentation
3. 📋 RBAC Audit - Create workflow guide
4. 📋 ACM Multi-Cluster Ops - Document workflow

### Documentation Enhancements
1. Add cost/capacity analysis tools
2. Document read-only & destructive-disable modes
3. Create advanced security audit procedures
4. Add storage troubleshooting guide

### Potential Toolset Additions
- `security-audit` - RBAC/policy audits
- `capacity-planning` - Cost analysis
- `storage` - PV/PVC operations

## 📚 Additional Resources

- [Official Docs](https://github.com/openshift/openshift-mcp-server)
- [Toolsets Guide](toolsets.md)
- [SRE Workflows](../../workflows/)
- [Advanced Topics](../../advanced/)

---

**Last Updated:** Sep 11, 2026  
**Coverage:** 16+ toolsets documented, 3+ recommended additions for complete SRE toolkit
