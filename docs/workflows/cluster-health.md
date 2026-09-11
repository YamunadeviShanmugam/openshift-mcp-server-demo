# Cluster Health Monitoring

Real-time cluster health assessment and proactive monitoring for SREs.

## Overview

Cluster health monitoring is critical for maintaining a reliable Kubernetes/OpenShift environment. This guide covers common monitoring scenarios and how to use OpenShift MCP Server for intelligent analysis.

## Key Metrics to Monitor

- **Node Health**: CPU, memory, disk usage, network status
- **Pod Status**: Running, pending, failed, crashing states
- **Resource Utilization**: CPU and memory across nodes and pods
- **Events**: Warnings, errors, and state changes
- **API Server Health**: Control plane availability and performance

## Quick Health Check

### 1. Overall Cluster Status

```
"Give me a comprehensive health check of my cluster including:
1. Number of nodes and their status
2. Any nodes with high resource utilization
3. Total pods and their status distribution
4. Any pods in error or pending state
5. Recent critical events"
```

**Expected Output:**
```
Nodes:
├─ node-1 (Ready, 12GB/16GB memory, 3CPU/4CPU)
├─ node-2 (Ready, 14GB/16GB memory, 3.5CPU/4CPU)
└─ node-3 (NotReady, High memory pressure)

Pods Status:
├─ Running: 47
├─ Pending: 2
├─ Failed: 1
└─ Unknown: 0

Issues Found:
├─ node-3 has memory pressure
├─ Pod: monitoring/prometheus (CrashLoopBackOff)
└─ Warning event: High API latency
```

### 2. Node-Level Analysis

```
"Analyze node resource allocation:
1. Sort nodes by memory usage (highest first)
2. Show CPU and memory requests vs actual usage
3. Identify oversubscribed nodes
4. List any nodes approaching capacity thresholds"
```

### 3. Pod Health by Namespace

```
"Check health of all pods in the production namespace:
1. Show any pods not in Running state
2. List recent restarts (pods with high restart count)
3. Show pods with resource requests > 500m CPU or 500Mi memory
4. Identify pods exceeding their resource limits"
```

### 4. Persistent Volume Monitoring

```
"Show the status of all persistent volumes and claims:
1. List all PVs and their usage percentage
2. Show which pods are using each PVC
3. Alert if any PV is over 80% capacity
4. List any pending PVCs"
```

## Scheduled Health Checks

### Daily Health Report

Create a pattern you run daily:

```
"Generate a daily health report:
1. Node count and average utilization
2. Pod crash rate and most crashed pods
3. Top 5 namespaces by resource usage
4. Any OOMKilled pods
5. Failed jobs in the last 24 hours
6. Certificate expiry status (if using cert-manager)
7. Storage capacity warnings"
```

### Weekly Capacity Planning

```
"Analyze cluster capacity for the next quarter:
1. Current resource utilization trends
2. Number of additional nodes needed
3. Storage growth rate
4. Network bandwidth bottlenecks
5. Recommendation for capacity upgrades"
```

## Alerting Conditions

### High Memory Usage

```
"Show all pods using more than 1Gi of memory:
1. List by namespace
2. Show which pods are not requesting memory limits
3. Recommend appropriate limits"
```

### Pod Crashes

```
"Find all pods that have crashed in the last hour:
1. Show restart count per pod
2. Display last logs before crash
3. Show events leading to crash
4. Identify common error patterns"
```

### Node Pressure

```
"Check for node pressure conditions:
1. Memory pressure (show affected pods)
2. Disk pressure (show mounted volumes)
3. PID pressure (show high-process-count pods)
4. Recommendation for remediation"
```

### Control Plane Health

```
"Assess control plane health:
1. Show status of etcd nodes
2. Check API server response times
3. Show scheduler backlog
4. Check for any controller manager issues"
```

## Proactive Monitoring Patterns

### Resource Trend Analysis

```
"Analyze resource trends over the last 7 days:
1. Show CPU and memory usage patterns by hour
2. Identify peak usage times
3. Predict when we'll hit capacity
4. Recommend scaling policies"
```

### Cost Optimization

```
"Identify cost optimization opportunities:
1. Find pods with overly generous resource requests
2. Show underutilized nodes
3. List pods with no resource requests (dangerous)
4. Recommend consolidation or rightsizing"
```

### Reliability Metrics

```
"Calculate cluster reliability metrics:
1. Pod uptime percentage (last 30 days)
2. Node availability percentage
3. Failed deployment percentage
4. Service error rates (if using observability tools)"
```

## Integration with Observability

### If Using Prometheus

```
"Show me Prometheus metrics for:
1. Node memory available
2. Pod CPU throttling rate
3. API request latency p95/p99
4. etcd latency"
```

### If Using Datadog/New Relic

```
"Query my monitoring service for:
1. Alert status across all environments
2. Most triggered alerts (last 24 hours)
3. Average resolution time per alert
4. Flapping alerts that need tuning"
```

## Automated Checks via CLI

Save these as shell scripts for daily execution:

```bash
#!/bin/bash
# daily-health-check.sh

echo "=== Daily Health Check ==="
npx kubernetes-mcp-server << 'EOF'
1. Cluster Status Summary
2. Node Resource Utilization
3. Pod Crash Analysis
4. Event Summary (last 24 hours)
5. Storage Capacity Check
EOF
```

## Best Practices

### ✅ DO

- Monitor multiple metrics simultaneously (CPU, memory, disk)
- Set baseline values and alert on deviations
- Regular capacity planning (weekly/monthly)
- Correlate pod crashes with node events
- Track trends over time (daily, weekly, monthly)

### ❌ DON'T

- Only monitor one metric (e.g., CPU alone)
- Ignore pending pods (they indicate issues)
- Set thresholds based on peak usage (use average + 20%)
- Forget about disk and network (CPU is not everything)
- Monitor without understanding root causes

## Troubleshooting

### "All pods show as Pending"

```
"Why are these pods pending?
1. Check available resources on nodes
2. Show node capacity and what's scheduled
3. Check for resource limits blocking scheduling
4. Show any pod scheduling failures"
```

### "Node shows NotReady"

```
"Diagnose why this node is NotReady:
1. Show node conditions (MemoryPressure, DiskPressure, etc.)
2. Show kubelet status
3. Check recent node events
4. Show pods on this node and their status"
```

### "Frequent pod restarts"

```
"Investigate why this pod keeps restarting:
1. Show restart count and timestamps
2. Display logs before each restart
3. Show resource limits vs actual usage
4. Check for liveness probe failures"
```

## Advanced Queries

### Custom Resource Analysis

```
"Analyze all custom resources in the cluster:
1. List all CRDs
2. Show instances per CRD
3. Identify unused or deprecated CRDs
4. Check custom resource validation errors"
```

### RBAC Audit

```
"Audit cluster RBAC:
1. Show all ClusterRoles with '*' permissions
2. List service accounts with cluster-admin access
3. Find all roles with secret access
4. Check for overly permissive bindings"
```

### Network Policy Review

```
"Review network policies:
1. List all network policies
2. Show ingress/egress rules
3. Identify any overly restrictive policies blocking traffic
4. Check for policy gaps"
```

## Next Steps

- [Troubleshooting Guide](troubleshooting.md) - Debug cluster issues
- [Security & Compliance](security-compliance.md) - Audit and secure clusters
- [Performance Tuning](performance-tuning.md) - Optimize cluster performance
- [Disaster Recovery](disaster-recovery.md) - Backup and recovery procedures

---

**Tips:** Save query patterns you use frequently and share with your team!
