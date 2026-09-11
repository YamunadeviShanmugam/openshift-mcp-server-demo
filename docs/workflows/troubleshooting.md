# Troubleshooting & Debugging

Comprehensive guide to diagnosing and resolving cluster issues with OpenShift MCP Server.

## Troubleshooting Methodology

```
1. Symptoms → What's the problem?
2. Scope → Which components are affected?
3. Root Cause → Why is it happening?
4. Solution → How do we fix it?
5. Prevention → How do we avoid it next time?
```

## Common Issues & Solutions

### Issue: Pod Not Starting (CrashLoopBackOff)

#### Diagnosis

```
"Diagnose this failing pod [NAMESPACE/POD_NAME]:
1. Show pod status and restart count
2. Get the last 50 lines of pod logs
3. Show any events related to this pod
4. Check resource requests vs node available resources
5. Show the pod's liveness/readiness probe configuration"
```

#### Analysis

- **Pod logs** - Application error messages, exit codes
- **Recent events** - Scheduling issues, eviction attempts
- **Resource availability** - Insufficient CPU, memory, disk
- **Probe configuration** - Liveness probe killing pod too quickly
- **Image availability** - Container image pull errors

#### Solution Examples

```
"Show logs for pod that crashed, identify error pattern"
"Is this pod being OOMKilled? Show memory limit and actual usage"
"When was this pod last restarted? Show restart timestamps"
"Check if liveness probe is failing - show probe configuration"
```

### Issue: Node Not Ready

#### Diagnosis

```
"Why is node [NODE_NAME] not ready?
1. Show node status and conditions
2. List all conditions (Ready, DiskPressure, MemoryPressure, etc.)
3. Show kubelet logs from this node
4. Check all pods scheduled on this node
5. Show recent node events"
```

#### Analysis

- **NotReady conditions** - What's causing the not-ready state
- **Kubelet logs** - Node-level errors
- **Disk space** - Is `/` or `/var/lib/kubelet` full?
- **Memory** - Memory pressure or OOM killer?
- **Network** - Node can't reach API server?

#### Solution Examples

```
"Node has memory pressure - show pods using most memory"
"Node disk is 90% full - which large directories to clean?"
"Kubelet appears crashed - show kubelet systemd logs"
"Network issue detected - show node network configuration"
```

### Issue: Deployment Failing to Update

#### Diagnosis

```
"Deployment [DEPLOYMENT] is stuck, diagnose:
1. Show deployment status and desired vs ready replicas
2. Show all replicasets associated with this deployment
3. Show events for failed pods
4. Check if image exists and is accessible
5. Show resource requests and node availability"
```

#### Analysis

- **ReplicaSet status** - How many replicas are ready?
- **Pod events** - Why are pods failing to start?
- **Image pull errors** - Can the node pull the image?
- **Quota/limits** - Are namespace quotas exceeded?
- **Rolled back?** - Is there an older working version?

#### Solution Examples

```
"Rollback this deployment to the previous working version"
"Current image pull is failing - show image repository errors"
"Not enough resources - show node capacity and request size"
"Quota exceeded - show namespace quotas and current usage"
```

### Issue: High Resource Usage

#### Diagnosis

```
"Top 5 pods using most CPU and memory:
1. Show resource requests vs actual usage
2. Show which are throttled (CPU)
3. Show which are near OOM
4. List pods without resource requests
5. Recommend appropriate resource limits"
```

#### Analysis

- **Workload type** - Is this expected (batch job vs daemon)?
- **Trend** - Growing over time or steady?
- **Limits** - Are limits set? Are they appropriate?
- **Requests** - Missing requests cause scheduling issues
- **Competitors** - What else is on this node?

#### Solution Examples

```
"Why is this pod using 3GB of memory? Show process details"
"This pod's CPU is being throttled - increase limits"
"Pods without requests are causing noisy neighbor problems"
"Batch job spike is normal - show peak vs average"
```

### Issue: Persistent Volume Issue

#### Diagnosis

```
"Diagnose PV/PVC issues:
1. Show all persistent volumes and their status
2. Show all pending PVCs
3. Show volume usage percentage
4. List pods using each volume
5. Check for volume mounting errors"
```

#### Analysis

- **Pending PVC** - Why isn't storage provisioned?
- **Full volume** - Is data growing unbounded?
- **Access mode** - Is volume accessible by pod?
- **Volume plugin** - Is the storage backend working?
- **inode usage** - Full inode table (different from disk full)?

#### Solution Examples

```
"This PVC is pending - show why storage isn't provisioning"
"Volume is 95% full - which pods are using most space?"
"Pod can't mount volume - show access mode mismatches"
"Inode exhaustion detected - how many files on this volume?"
```

### Issue: API Server Overloaded

#### Diagnosis

```
"Diagnose API server performance issues:
1. Show API request latency metrics
2. Identify top APIs being called
3. Show API errors (4xx, 5xx)
4. Check etcd latency
5. Show control plane pod resource usage"
```

#### Analysis

- **Request rate** - Are we hitting rate limits?
- **Latency** - p99 latency vs baseline?
- **Error rate** - Which endpoints failing?
- **etcd** - Is etcd the bottleneck?
- **Client** - Which client generating most requests?

#### Solution Examples

```
"API latency increased 10x - show what changed (new client?)"
"etcd latency spike detected - check database size"
"Rate limiting triggered - which client to rate limit?"
```

### Issue: Network Connectivity

#### Diagnosis

```
"Diagnose network connectivity for pod [NAMESPACE/POD]:
1. Check if pod can reach other pods
2. Test DNS resolution
3. Check network policies blocking traffic
4. Show pod network interface configuration
5. Test external connectivity if needed"
```

#### Analysis

- **DNS** - Can pods resolve service names?
- **Network policies** - Are they blocking required traffic?
- **Service** - Does the service exist and have endpoints?
- **CNI** - Is the container network interface working?
- **Firewall** - Are firewalls blocking traffic?

#### Solution Examples

```
"Pod can't reach database - show network policy rules"
"DNS resolution failing - check CoreDNS pod status"
"Service has no endpoints - show selector vs pod labels"
"External IP unreachable - check egress rules"
```

## Debugging Workflows

### 1. Application Error Debugging

```
"Application in pod [POD] is erroring. Help debug:
1. Show the logs (last 100 lines)
2. Highlight ERROR or FATAL lines
3. Show stack trace if available
4. Check environment variables
5. Show any related events
6. Based on errors, what's the likely root cause?"
```

### 2. Performance Debugging

```
"Application is slow. Diagnose performance:
1. Is it CPU-bound or I/O-bound?
2. Show resource usage trends
3. Check for throttling
4. Is it database-related? Show DB pod stats
5. Is it disk-related? Show storage metrics"
```

### 3. Memory Leak Debugging

```
"Suspect memory leak in pod [POD]:
1. Show memory usage over time
2. Is it growing continuously?
3. Show restart frequency
4. Check for OOMKilled events
5. Show pod memory limits
6. Recommendation: increase limits vs code fix"
```

### 4. Deployment Update Debugging

```
"Deployment update is stuck. Debug:
1. Show all replicasets
2. How many replicas old version has?
3. How many replicas new version has?
4. Show events for new pods
5. Are new pods failing to start?"
```

## Advanced Debugging

### Log Analysis at Scale

```
"Show all ERROR logs across the cluster in the last hour:
1. Which namespaces have most errors?
2. Which pods are erroring?
3. Common error patterns?
4. Any related events?"
```

### Event Correlation

```
"Show events leading to pod failure:
1. All events for pod in last 5 minutes
2. Events for the pod's node
3. Events for related services/ingress
4. Any warning events before failure?"
```

### Historical Analysis

```
"This pod frequently crashes. Analyze pattern:
1. When does it crash? Time of day?
2. What resource was constrained?
3. What events preceded crashes?
4. Is it correlated with other pod crashes?"
```

## Common Error Messages & Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| `ImagePullBackOff` | Can't pull image | Check image URL, registry credentials, network |
| `CrashLoopBackOff` | Pod keeps crashing | Check logs, resource limits, probe config |
| `Pending` | Can't schedule pod | Check resource availability, node selectors |
| `OOMKilled` | Out of memory | Increase memory limit or fix memory leak |
| `Evicted` | Node pressure | Free disk/memory on node or migrate workloads |
| `FailedScheduling` | Can't schedule | Check quotas, node labels, taints/tolerations |

## Best Practices

### ✅ DO

- Check logs first (most issues are in logs)
- Look at events (shows what happened)
- Check resource availability
- Verify configuration is correct
- Check for recent changes (deployments, configs)

### ❌ DON'T

- Restart pod without understanding root cause
- Increase resource limits without investigating
- Ignore warnings (they often precede failures)
- Assume permission issue is in code (check RBAC)
- Forget about node-level problems

## Preventive Measures

### Health Checks

```
"Set up probes for all pods:
1. Which pods lack liveness probes?
2. Which pods lack readiness probes?
3. Show probe timeouts that are too short
4. Show probe failure thresholds that are risky"
```

### Resource Management

```
"Enforce resource requests/limits:
1. List pods without resource requests
2. Show pods requesting too much (likely misconfig)
3. Show pods with no limits (risky)
4. Recommend appropriate values based on actual usage"
```

### Logging & Monitoring

```
"Improve observability:
1. Which pods/namespaces send most logs?
2. Are structured logs being used?
3. Show trace sampling rate
4. Check alerting completeness"
```

## Next Steps

- [Cluster Health Monitoring](cluster-health.md) - Proactive monitoring
- [Security & Compliance](security-compliance.md) - Audit and secure
- [Performance Tuning](performance-tuning.md) - Optimize cluster
- [Incident Response](incident-response.md) - Handle incidents

---

**Key Insight:** Most issues are visible in logs, events, and metrics. Check these first!
