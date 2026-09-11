# Observability Setup

Configure comprehensive observability for your Kubernetes/OpenShift cluster.

## Overview

Observability includes three pillars:
- **Metrics** - What's happening numerically
- **Logs** - What events occurred
- **Traces** - How requests flow through the system

## Prometheus Integration

### Querying Metrics

```
"Query Prometheus for:
1. Pod CPU usage (current)
2. Node memory available
3. API request latency p99
4. etcd commit latency"
```

### Common Queries

```
"Show me:
1. Pods using most CPU
2. Nodes approaching capacity
3. High error rate APIs
4. Slow database queries"
```

## Loki Log Querying

### Search Logs

```
"Search logs for:
1. ERROR and FATAL lines (last 24h)
2. Pod restart patterns
3. Failed deployments
4. Security-related events"
```

### Log Analysis

```
"Analyze logs to find:
1. Common error patterns
2. Which pods crash most often
3. Slow startup patterns
4. Unusual network activity"
```

## Jaeger Distributed Tracing

### Trace Analysis

```
"Using Jaeger, find:
1. Slowest endpoints
2. Services with most errors
3. Traces with high latency
4. Bottleneck services"
```

### Performance Debugging

```
"Debug slow requests:
1. Show trace timeline
2. Identify slow components
3. Show service dependencies
4. Recommend optimizations"
```

## OpenTelemetry Integration

### OTEL Configuration

```toml
[telemetry]
enabled = true
endpoint = "http://localhost:4317"
sampling_rate = 0.1
```

### Collecting Metrics

```
"Collect OTEL metrics for:
1. Cluster health
2. Application performance
3. User experience
4. Resource utilization"
```

## Best Practices

### ✅ DO

- Instrument all applications
- Collect metrics, logs, and traces
- Set reasonable retention policies
- Alert on anomalies
- Document your SLOs

### ❌ DON'T

- Over-sample (resource waste)
- Under-sample (miss issues)
- Ignore logs in compliance tools
- Forget about log rotation
- Alert on everything

## Next Steps

- [Cluster Health Monitoring](cluster-health.md)
- [Performance Tuning](performance-tuning.md)

---

**Key Insight:** Good observability catches issues before users do!
