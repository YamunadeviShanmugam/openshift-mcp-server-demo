# Disaster Recovery

Backup strategies, recovery procedures, and disaster recovery planning for SREs.

## Overview

Disaster recovery is critical for production environments. This guide covers backup strategies, recovery procedures, and RTO/RPO planning.

## Backup Strategy

### 1. Backup Scope Analysis

```
"Audit backup coverage:
1. Which persistent volumes are backed up?
2. Which applications have backups?
3. Show backup frequency by workload
4. Identify missing backups
5. Calculate overall RTO/RPO"
```

### 2. OADP (OpenShift API for Data Protection)

```
"If using OADP, analyze backup status:
1. Show all backup schedules
2. List failed backups
3. Show backup size and duration
4. Check backup storage capacity
5. Verify restore procedures"
```

### 3. Velero Integration

```
"If using Velero, verify backups:
1. Show backup schedules
2. Check last backup success date/time
3. Show backup retention policies
4. Verify backup location is accessible
5. Test restore process"
```

## Recovery Procedures

### 1. Pod Recovery

```
"Recover a deleted pod:
1. Show pod deletion timestamp
2. Check if backup contains this pod
3. List available snapshots
4. Restore pod configuration
5. Verify pod is running"
```

### 2. Persistent Volume Recovery

```
"Recover a persistent volume:
1. Show last successful snapshot
2. List available restore points
3. Create snapshot clone
4. Mount and verify data
5. Attach to pod"
```

### 3. Namespace Recovery

```
"Recover entire namespace [NAMESPACE]:
1. Show backup timestamp
2. List all resources in backup
3. Create recovery namespace
4. Restore all objects
5. Verify application functionality"
```

### 4. Full Cluster Recovery

```
"Full cluster disaster recovery:
1. Verify backup completeness
2. Show cluster state at backup time
3. Restore core components (etcd, API)
4. Restore workloads
5. Restore storage
6. Verify cluster health"
```

## RTO/RPO Analysis

### 1. Recovery Time Objective (RTO)

```
"Calculate RTO for each workload:
1. Time to detect failure
2. Time to restore backup
3. Time for pod startup
4. Time to pass health checks
5. Total RTO and SLA compliance"
```

### 2. Recovery Point Objective (RPO)

```
"Calculate RPO for each workload:
1. Backup frequency
2. Data change rate
3. Maximum acceptable data loss
4. Backup RPO vs workload RPO
5. Increase frequency if needed"
```

## High Availability

### 1. Multi-Zone/Multi-Region

```
"Analyze multi-zone resilience:
1. How many zones is cluster distributed across?
2. Show pod distribution across zones
3. Check node affinity/anti-affinity
4. Show zone failure impact
5. Recommend multi-zone improvements"
```

### 2. Pod Distribution

```
"Analyze pod resilience:
1. Show pod replicas on same node (bad)
2. Show pods with no replicas
3. Check pod disruption budgets
4. Show graceful shutdown configuration
5. Recommend distribution improvements"
```

## Runbook Examples

### Database Outage Recovery

```
"Runbook: Recover from database outage:
1. Detect database pod failure
2. Check persistent volume status
3. Restore database from backup
4. Verify data integrity
5. Reconnect dependent applications
6. Monitor for issues"
```

### Cluster Failure Recovery

```
"Runbook: Recover from cluster failure:
1. Bring up new cluster
2. Restore etcd from backup
3. Restore persistent volumes
4. Deploy applications
5. Switch traffic/DNS
6. Verify all systems"
```

### Data Corruption Recovery

```
"Runbook: Recover from data corruption:
1. Identify corruption point in time
2. Restore from pre-corruption backup
3. Verify data is intact
4. Identify and fix corruption cause
5. Resume operations"
```

## Testing & Validation

### 1. Backup Testing

```
"Test backup process:
1. Trigger manual backup
2. Verify backup completion
3. Check backup data integrity
4. Verify backup is restorable
5. Calculate backup size/time"
```

### 2. Restore Testing

```
"Test restore process:
1. Create test namespace
2. Restore backup to test namespace
3. Verify all objects restored
4. Verify application functionality
5. Check data integrity"
```

### 3. Disaster Recovery Drill

```
"Conduct DR drill:
1. Simulate cluster failure
2. Execute recovery runbook
3. Measure actual RTO
4. Verify RPO achievement
5. Document lessons learned"
```

## Best Practices

### ✅ DO

- Backup early and often
- Test backups regularly (restore testing)
- Document recovery procedures
- Plan for multi-zone failover
- Monitor backup job success
- Version backups (don't delete old backups)
- Encrypt backups in transit and at rest
- Store backups in different location than cluster

### ❌ DON'T

- Assume backups work (test them!)
- Only backup when you think of it
- Rely on single backup copy
- Forget about backup retention
- Backup without testing restore
- Ignore encryption
- Keep backups on same storage
- Skip incremental backups

## Backup Architecture

```
┌─────────────────┐
│   Production    │
│    Cluster      │
│                 │
├─────────────────┤
│  Workloads      │
│  Storage        │
│  Configs        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│    Backup       │
│    System       │
│  (OADP/Velero)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Backup        │
│   Storage       │
│  (S3/Azure)     │
└─────────────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│Region 1│ │Region 2│
│Storage │ │Storage │
└────────┘ └────────┘
```

## Checklist

- [ ] Backup schedule defined
- [ ] Backup retention policy set
- [ ] Backup testing scheduled
- [ ] RTO/RPO documented
- [ ] Recovery runbooks written
- [ ] Team trained on procedures
- [ ] Multi-zone/region planned
- [ ] Failover tested
- [ ] Monitoring on backup jobs
- [ ] Backup encryption enabled

## Next Steps

- [Cluster Health Monitoring](cluster-health.md) - Prevent failures
- [Troubleshooting](troubleshooting.md) - Diagnose issues
- [Security & Compliance](security-compliance.md) - Protect backups
- [Incident Response](incident-response.md) - Respond to incidents

---

**Remember:** A backup is only useful if you've tested the restore!
