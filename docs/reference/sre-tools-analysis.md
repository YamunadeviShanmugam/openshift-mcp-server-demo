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

### Missing: Advanced Must-Gather Analysis
Currently we have a Must-Gather Analysis page, but it could include:
- Automated must-gather parsing
- Common error pattern detection
- Historical analysis across multiple bundles

### Missing: Resource Quota & Limit Management
**Why Important:** Prevent cluster overprovisioning, enforce multi-tenant isolation

Suggested page: `docs/workflows/resource-management.md`
- Pod/Node resource limits
- Namespace quotas
- LimitRanges
- Resource requests/limits best practices

### Missing: RBAC & Security Audit
**Why Important:** Access control, compliance, security hardening

Suggested page: `docs/workflows/security-audit.md`
- RBAC permission review
- Service account management
- Network policy audit
- Pod security policies

### Missing: Cost Optimization & Capacity Planning
**Why Important:** Budget control, infrastructure planning

Suggested page: `docs/workflows/cost-optimization.md`
- Node utilization analysis
- Pod efficiency metrics
- Unused resource cleanup
- Capacity forecasting

### Missing: Multi-Cluster Failover & Load Balancing
**Why Important:** High availability, disaster recovery

Suggested page: `docs/workflows/multi-cluster-operations.md`
- ACM (Advanced Cluster Management) integration
- Cross-cluster traffic routing
- Failover procedures
- Load distribution

### Missing: Storage & Persistent Volume Management
**Why Important:** Data persistence, backup verification

Suggested page: `docs/workflows/storage-management.md`
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
|----------|--------|-----------|
| **Cluster Health** | ✅ Complete | core, observability, cluster-diagnostics |
| **Troubleshooting** | ✅ Complete | cluster-diagnostics, logs, metrics |
| **Security & Compliance** | ⚠️ Partial | openshift, cni-diagnostics |
| **Performance Tuning** | ✅ Complete | metrics, observability |
| **Disaster Recovery** | ✅ Complete | oadp, kubernetes resources |
| **Observability** | ✅ Complete | logs, metrics, traces |
| **Incident Response** | ✅ Complete | All tools |
| **Must-Gather Analysis** | ✅ New | cluster-diagnostics |
| **Resource Management** | ❌ Missing | Need quota/limit tools |
| **RBAC Auditing** | ❌ Missing | Need security audit tools |
| **Cost Optimization** | ❌ Missing | Need capacity analysis |
| **Multi-Cluster Ops** | ⚠️ Partial | ACM available but not documented |

## 🚀 Quick Reference: Tool Selection by Scenario

### Cluster Won't Start?
1. cluster-diagnostics → node debugging
2. observability/logs → error logs
3. oadp → check recent changes from backup

### High CPU Usage?
1. observability/metrics → Prometheus queries
2. core → identify resource-hungry pods
3. performance-tuning workflow → optimization

### Network Connectivity Issue?
1. cni-diagnostics → CNI validation
2. ovn-kubernetes → network flow analysis
3. netobserv → network metrics

### Backup Failed?
1. oadp → backup status
2. core → PVC health
3. cluster-diagnostics → storage issues

### Security Concern?
1. openshift → RBAC review
2. cni-diagnostics → network policies
3. Must-Gather → audit logs

## 📝 Recommendations

### High Priority Additions
1. ✅ Must-Gather Analysis - Added
2. 📋 Resource Quota Management - Create workflow
3. 📋 RBAC Audit - Create workflow
4. 📋 ACM Multi-Cluster Ops - Document workflow

### Documentation Improvements
1. Add cost/capacity analysis tools
2. Document read-only & destructive-disable modes
3. Create advanced security audit procedures
4. Add storage troubleshooting guide

### Toolset Suggestions
- Consider adding: `security-audit` toolset for RBAC/policy audits
- Consider adding: `capacity-planning` toolset for cost analysis
- Consider adding: `storage` toolset for PV/PVC operations

## 📚 Where to Learn More

- [Official Docs](https://github.com/openshift/openshift-mcp-server)
- [Toolsets Guide](toolsets.md)
- [SRE Workflows](../workflows/)
- [Advanced Topics](../advanced/)

---

**Last Updated:** Sep 11, 2026
**Coverage:** 16+ toolsets documented, 3+ missing for complete SRE toolkit
