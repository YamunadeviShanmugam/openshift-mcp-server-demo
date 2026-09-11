# Incident Response

Rapid incident detection, diagnosis, and resolution for SREs.

## Incident Response Workflow

### Phase 1: Detection (0-5 minutes)

```
"An incident has been detected. What's happening?
1. Show current cluster health
2. List all error events (last 30 minutes)
3. Show any pods in error state
4. Check if API is responding
5. Show recent deployment changes"
```

### Phase 2: Initial Diagnosis (5-15 minutes)

```
"Initial diagnosis of the incident:
1. Which component is affected?
2. Show related logs and errors
3. Check resource availability
4. Show recent changes (deployments, configs)
5. Affected users/services?"
```

### Phase 3: Root Cause Analysis (15-45 minutes)

```
"Root cause analysis:
1. Timeline of events leading to incident
2. What changed before incident?
3. Are similar incidents in history?
4. What's the underlying issue?
5. Why wasn't this caught earlier?"
```

### Phase 4: Mitigation (5-30 minutes)

```
"Mitigate the incident:
1. What's the quickest fix?
2. Show rollback procedure (if deployment issue)
3. Recommend immediate actions
4. Timeline to resolution"
```

### Phase 5: Resolution (Varies)

```
"Resolve the incident:
1. Verify fix is working
2. Confirm metrics are healthy
3. Check for side effects
4. Document resolution steps"
```

### Phase 6: Post-Incident (Next day)

```
"Post-incident review:
1. What caused the incident?
2. Why was it not detected earlier?
3. How can we prevent it?
4. Action items for prevention"
```

## Common Incident Scenarios

### Scenario: Service Unavailability

```
"Service is down. Quickly diagnose:
1. Pod status for service [SERVICE]
2. Recent deployment changes
3. Resource constraints?
4. Network connectivity?
5. Database connectivity?
6. Recommendation: rollback, scale, or fix config?"
```

### Scenario: High Latency

```
"Service latency spiked. Diagnose:
1. CPU/memory usage (throttling?)
2. Database performance
3. Network bandwidth saturation?
4. API server latency
5. Dependent services status
6. Recommendation: scale, optimize, or investigate dependency?"
```

### Scenario: Data Corruption

```
"Possible data corruption. Diagnose:
1. When did corruption start?
2. Which service is affected?
3. What changed around that time?
4. Is backup available pre-corruption?
5. Recovery options?"
```

### Scenario: Security Breach

```
"Suspected security incident. Diagnose:
1. Unusual API calls or access patterns
2. Any privilege escalation attempts?
3. Unusual network traffic?
4. Which pods/users involved?
5. Containment options?"
```

### Scenario: Resource Exhaustion

```
"Cluster resource exhaustion. Diagnose:
1. Which resource exhausted? (CPU, memory, disk)
2. Which pods/workloads consuming it?
3. Which nodes affected?
4. Can we evict non-critical pods?
5. Can we scale infrastructure?"
```

## Quick Diagnosis Playbooks

### Pod CrashLooping

```
"Pod [POD] is crashing. Diagnose quickly:
1. Restart count and timeline
2. Last 50 lines of logs
3. Error pattern (same error or different?)
4. Recent deployments/changes
5. Resource constraints?"
```

### Node Offline

```
"Node [NODE] is offline. Diagnose:
1. Node status and conditions
2. When did it go offline?
3. Can we SSH to node?
4. Kubelet status
5. Any pods affected?"
```

### API Timeout

```
"API calls timing out. Diagnose:
1. Which APIs slow? 
2. API server CPU/memory
3. etcd latency
4. Who's querying (which client)?
5. Rate limiting active?"
```

## Runbook Examples

### Database Pod Recovery

```
1. Check if PVC is still accessible
2. Restore database from backup (if data corrupted)
3. Scale pod to 0, then 1
4. Verify startup logs
5. Run health check
```

### Revert Bad Deployment

```
1. Identify previous working revision
2. Show rollback command
3. Execute rollback
4. Verify new pods are running
5. Check service health
```

### Clear Node Disk

```
1. Identify large files
2. Stop container runtime if needed
3. Clean docker volumes: `docker volume prune`
4. Clean system logs
5. Restart kubelet if needed
```

## Escalation Criteria

### Escalate to Platform Team

- Cluster-level issue (API, etcd, CNI)
- Infrastructure issue (node hardware)
- Multiple services affected
- Control plane issues

### Stay in Application Team

- Single application issue
- Configuration problem
- Application bug
- Resource limit issue

## Incident Communication

### Initial Alert (To Team)

```
"Incident Alert:
- Service: [SERVICE]
- Severity: P1/P2/P3
- Status: Ongoing
- Detection: [AUTO/MANUAL]
- Impact: [NUMBER] users affected
- Current action: Investigating"
```

### Status Update (Every 15 minutes)

```
"Incident Update:
- Current status: [INVESTIGATING/MITIGATING/RESOLVED]
- What we found: [KEY FINDINGS]
- Current action: [WHAT'S BEING DONE]
- ETA: [ESTIMATED TIME]"
```

### Post-Incident Summary

```
"Incident Summary:
- Duration: [TIME]
- Root cause: [CAUSE]
- Resolution: [HOW IT WAS FIXED]
- Impact: [AFFECTED USERS/DURATION]
- Prevention: [ACTION ITEMS]"
```

## Metrics to Track

- **MTTR** (Mean Time To Repair) - How long to fix?
- **MTTD** (Mean Time To Detect) - How long to detect?
- **MTBF** (Mean Time Between Failures) - How often?
- **RTO** (Recovery Time Objective) - Target fix time
- **RPO** (Recovery Point Objective) - Max acceptable data loss

## Incident Severity Levels

| Severity | Definition | Response Time | Escalation |
|----------|-----------|---|---|
| P0 | Complete service down | Immediate | Executive |
| P1 | Service degraded, users impacted | <15 min | Management |
| P2 | Partial degradation | <1 hour | Team lead |
| P3 | Minor issue, no user impact | <4 hours | Team |

## Prevention Checklist

- [ ] Monitoring and alerting in place
- [ ] Health checks configured properly
- [ ] Resource limits set appropriately
- [ ] Backups tested regularly
- [ ] Runbooks documented
- [ ] Team trained on procedures
- [ ] Load testing conducted
- [ ] Chaos engineering tests run

## Next Steps

- [Cluster Health Monitoring](cluster-health.md) - Prevent incidents
- [Troubleshooting](troubleshooting.md) - Diagnose issues
- [Disaster Recovery](disaster-recovery.md) - Prepare for worst

---

**Remember:** "An incident prevented is an incident solved!"
