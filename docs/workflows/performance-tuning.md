# Performance Tuning

Optimize cluster performance and resource efficiency for SREs.

## Performance Metrics

Key metrics to monitor:
- **API Latency** - p50, p95, p99 (should be <100ms)
- **etcd Latency** - Commit latency (should be <25ms)
- **Pod Startup Time** - Time to ready state
- **Memory Usage** - RSS, working set
- **CPU Usage** - Actual vs throttled

## CPU Performance

### 1. CPU Throttling Analysis

```
"Analyze CPU throttling across cluster:
1. Show pods experiencing CPU throttling
2. List pods that request less than they need
3. Show pods with high CPU limit but low request
4. Recommend limit/request ratios
5. Show impact on latency"
```

### 2. CPU Utilization Optimization

```
"Optimize CPU utilization:
1. Show pods using <10% of requested CPU
2. List nodes with low CPU utilization
3. Identify pods for consolidation
4. Show CPU density improvements
5. Recommend node consolidation"
```

## Memory Performance

### 1. Memory Optimization

```
"Optimize memory usage:
1. Show pods with high memory usage
2. Identify memory leaks (growing over time)
3. Show memory waste (requested vs actual)
4. Recommend memory limits
5. Show OOM risk assessment"
```

### 2. Memory Pressure Detection

```
"Monitor memory pressure:
1. Show nodes approaching memory limits
2. List pods eviction candidates
3. Show memory pressure events
4. Recommend memory upgrades
5. Show pod priority and preemption order"
```

## Disk I/O Performance

### 1. Disk Usage Analysis

```
"Analyze disk usage:
1. Show nodes with high disk utilization
2. List large persistent volumes
3. Identify unused volumes
4. Show volume I/O metrics
5. Recommend cleanup or upgrades"
```

### 2. Disk Pressure Monitoring

```
"Monitor disk pressure:
1. Show nodes with disk pressure
2. List evicted pods (disk-related)
3. Show inode usage on nodes
4. Identify runaway log producers
5. Recommend log rotation configuration"
```

## Network Performance

### 1. Network Bandwidth Analysis

```
"Analyze network bandwidth:
1. Show top pod-to-pod communication patterns
2. Identify bandwidth hotspots
3. Show egress vs ingress traffic
4. Check for unnecessary inter-pod traffic
5. Recommend network policy optimizations"
```

### 2. DNS Performance

```
"Analyze DNS performance:
1. Show DNS query latency
2. Identify DNS cache hit rate
3. Show DNS resolution errors
4. Check CoreDNS pod resource usage
5. Recommend CoreDNS tuning"
```

## API Server Performance

### 1. API Latency Analysis

```
"Analyze API server performance:
1. Show API request latency percentiles
2. Identify slowest APIs
3. Show API error rates
4. Check etcd latency
5. Recommend optimization"
```

### 2. API Load Balancing

```
"Analyze API load across servers:
1. Show requests per API server
2. Identify overloaded API servers
3. Check rate limiting status
4. Show watch connection count
5. Recommend load balancing"
```

## Workload Performance

### 1. Pod Startup Time

```
"Analyze pod startup times:
1. Show average startup time by deployment
2. Identify slow-starting pods
3. Show initialization phases
4. Check image pull latency
5. Recommend startup optimizations"
```

### 2. Application Latency

```
"Analyze application performance:
1. Show request latency from traces
2. Identify slow endpoints
3. Show bottleneck services
4. Check for cascading failures
5. Recommend optimization"
```

## Optimization Strategies

### 1. Resource Request Tuning

```
"Optimize resource requests:
1. Analyze actual usage vs requested
2. Calculate appropriate requests
3. Show cost impact of changes
4. Recommend new request values
5. Show safety margins (headroom)"
```

### 2. Horizontal Scaling

```
"Analyze horizontal scaling:
1. Show pods per node (density)
2. Recommend node consolidation
3. Identify scaling bottlenecks
4. Show HPA status
5. Recommend target metrics"
```

### 3. Vertical Scaling

```
"Analyze vertical scaling needs:
1. Show underutilized node resources
2. Recommend node type upgrades
3. Show cost vs performance trade-off
4. Identify over-provisioned nodes
5. Recommend rightsizing"
```

## Cluster-Wide Optimization

### 1. Cluster Efficiency Report

```
"Generate cluster efficiency report:
1. Resource utilization by namespace
2. Cluster packing efficiency
3. Recommended consolidations
4. Estimated cost savings
5. Performance vs cost trade-off"
```

### 2. Bottleneck Analysis

```
"Identify cluster bottlenecks:
1. What's the limiting resource? (CPU, memory, network, disk)
2. Which namespace/workloads are causing it?
3. Impact on overall cluster performance
4. Recommended mitigation"
```

## Best Practices

### ✅ DO

- Set appropriate resource requests and limits
- Monitor actual usage vs limits
- Right-size resources based on data
- Use horizontal scaling for load
- Monitor trends (growing resources?)
- Test performance under load
- Use CPU/memory metrics for scaling decisions

### ❌ DON'T

- Set resource requests too high (wastes resources)
- Set requests without monitoring
- Ignore CPU throttling
- Assume vertical scaling helps everything
- Over-provision "just in case"
- Ignore network bottlenecks
- Scale based on hunches

## Performance Tuning Checklist

- [ ] Resource requests and limits set appropriately
- [ ] No pods with CPU throttling
- [ ] No memory pressure on nodes
- [ ] API latency within SLA
- [ ] etcd latency healthy (<25ms)
- [ ] Pod startup time acceptable
- [ ] No excessive pod evictions
- [ ] Cluster packing efficiency good (>60%)
- [ ] No DNS resolution issues
- [ ] Network bandwidth not saturated

## Next Steps

- [Cluster Health Monitoring](cluster-health.md) - Monitor performance
- [Troubleshooting](troubleshooting.md) - Debug performance issues
- [Disaster Recovery](disaster-recovery.md) - Backup and recovery

---

**Key Insight:** Most performance issues are resource-related. Start with actual usage data!
