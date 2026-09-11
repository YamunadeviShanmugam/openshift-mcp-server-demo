# Security & Compliance

RBAC audits, policy enforcement, and compliance checks for secure OpenShift clusters.

## Overview

Security is critical for production Kubernetes/OpenShift environments. This guide covers RBAC audits, secret management, and compliance verification.

## RBAC Audits

### 1. Cluster Admin Access Review

```
"Audit cluster-admin access:
1. List all users/service accounts with cluster-admin role
2. Show all RoleBindings with cluster-admin
3. Show all ClusterRoleBindings with cluster-admin
4. Flag any unexpected cluster-admin access
5. Recommend least-privilege alternatives"
```

### 2. Service Account Audit

```
"Audit all service accounts in the cluster:
1. List all service accounts with custom roles
2. Show service accounts that can read secrets
3. Show service accounts with write permissions
4. Flag default service account usage
5. Recommend restricting default service account"
```

### 3. Role Analysis

```
"Analyze cluster roles and permissions:
1. List all custom ClusterRoles
2. Show roles with wildcards (overly permissive)
3. List roles with secret access
4. Show roles with pod execution access
5. Show roles with api-group '*' (dangerous)"
```

## Secret Management

### 1. Secret Audit

```
"Audit secrets in the cluster:
1. List all secrets by namespace
2. Show which pods access each secret
3. Identify unused secrets
4. Check for secrets in ConfigMaps (bad practice)
5. Show secret age (rotate old secrets)"
```

### 2. Secret Access Control

```
"Review secret access controls:
1. Which roles have access to secrets?
2. Show service accounts with secret read access
3. Identify overly permissive secret policies
4. Check if get secrets requires audit
5. Verify secret encryption at rest is enabled"
```

## Network Policy Compliance

### 1. Network Policy Review

```
"Review network policies for compliance:
1. List all network policies
2. Identify namespaces without default deny policies
3. Show overly permissive ingress/egress rules
4. Identify pods with no network policies
5. Check for east-west traffic restrictions"
```

### 2. Pod Security Policy

```
"Review pod security policies:
1. Show which namespaces enforce PodSecurityPolicy
2. List pods running as root
3. Show privileged containers
4. Check for pods with host access
5. Verify capability dropping"
```

## Image & Registry Security

### 1. Image Scan Audit

```
"Audit container images for vulnerabilities:
1. List all images deployed in cluster
2. Show images from untrusted registries
3. Identify images without version tags
4. Show images running as root
5. Check image scan results (if integrated)"
```

### 2. Registry Access Control

```
"Audit image registry access:
1. Show all image pull secrets
2. Identify registries without authentication
3. Check for hardcoded credentials (bad practice)
4. Show which service accounts pull images
5. Verify image signing if applicable"
```

## Compliance Checks

### 1. CIS Benchmark

```
"Check CIS Kubernetes Benchmark compliance:
1. Verify kubelet secure port configuration
2. Check API server audit logging
3. Show controller-manager security settings
4. Verify scheduler security settings
5. Check etcd encryption configuration"
```

### 2. Data Protection

```
"Review data protection compliance:
1. Verify encryption at rest is enabled
2. Show secrets storage location
3. Check if etcd is encrypted
4. Review persistent volume encryption
5. Show any unencrypted data stores"
```

### 3. Audit Logging

```
"Review audit logging configuration:
1. Check if API audit logging is enabled
2. Show audit log retention period
3. Verify audit logs are secure (encrypted, backed up)
4. Check what events are being audited
5. Show audit log completeness"
```

## Threat & Vulnerability Assessment

### 1. Privilege Escalation Risks

```
"Identify privilege escalation risks:
1. Show pods with privileged: true
2. List pods with capability: SYS_ADMIN
3. Show pods with host network access
4. Identify pods running as root
5. Show insecure pod security policies"
```

### 2. Data Exposure Risks

```
"Identify potential data exposure:
1. Show ConfigMaps with sensitive data
2. Identify secrets stored in wrong places
3. Check for hardcoded credentials
4. Show pods with excessive read permissions
5. Identify overly open network policies"
```

### 3. Supply Chain Security

```
"Review supply chain security:
1. Verify container images are from approved registries
2. Check if images are signed
3. Show which images are scanned for vulnerabilities
4. Verify helm charts are from trusted sources
5. Check deployment image policies (must be specific version)"
```

## Compliance Reports

### 1. Security Posture Report

```
"Generate security posture report:
1. RBAC compliance score
2. Pod security compliance score
3. Network policy coverage percentage
4. Image security score
5. Overall security rating with recommendations"
```

### 2. Audit Trail Report

```
"Generate audit trail report:
1. Failed authentication attempts (last 24h)
2. Privilege escalation attempts
3. Configuration changes (who changed what)
4. Secret access patterns
5. Suspicious API calls"
```

### 3. Vulnerability Report

```
"Generate vulnerability report:
1. Known CVEs in deployed images
2. Unpatched cluster components
3. Deprecated API versions in use
4. Expired certificates
5. Security patches available"
```

## Best Practices

### ✅ DO

- Use namespace-scoped roles when possible (not ClusterRoles)
- Implement network policies (default deny)
- Rotate secrets regularly
- Audit secret access
- Use service account tokens with minimal scope
- Implement PodSecurityPolicy or Pod Security Standards
- Sign container images
- Scan images for vulnerabilities

### ❌ DON'T

- Use cluster-admin for applications
- Grant wildcard permissions
- Store secrets in ConfigMaps
- Use default service account
- Deploy images with "latest" tag
- Skip image scanning
- Store credentials in code/manifests
- Run privileged containers unnecessarily

## Automated Compliance Checks

### Daily Security Audit

```bash
#!/bin/bash
# daily-security-audit.sh

echo "=== Daily Security Audit ==="
npx kubernetes-mcp-server << 'EOF'
1. Cluster Admin Access Review
2. Service Account Audit
3. Pod Security Analysis
4. Network Policy Coverage
5. Secret Access Audit
6. Summary of findings
EOF
```

### Weekly Compliance Report

```
"Generate weekly compliance report covering:
1. RBAC changes (who changed what)
2. New privileged containers
3. Network policy changes
4. Image policy violations
5. Secret access patterns
6. Any security events"
```

## Remediation Examples

### Too Permissive RBAC

```
"Current policy: [role yaml]
Propose restricted policy following least-privilege:
1. What specific resources are needed?
2. What specific verbs are needed?
3. What namespace scoping?
4. Show the improved role"
```

### Overly Permissive Network Policy

```
"Current policy allows too much traffic:
1. Show current traffic patterns (actual vs allowed)
2. Restrict ingress to only needed sources
3. Restrict egress to only needed destinations
4. Show updated policy"
```

### Exposed Secrets

```
"Secrets are accessible to service account [SA]:
1. Are these secrets actually needed?
2. Can we remove access?
3. Can we use different service account?
4. Show updated policy with minimal access"
```

## Integration with Compliance Tools

### If Using Falco

```
"Show Falco runtime security alerts:
1. Suspicious process execution
2. Unauthorized file access
3. Network anomalies
4. Container escape attempts
5. Privilege escalation attempts"
```

### If Using OPA/Gatekeeper

```
"Show policy violations:
1. Policies that are failing
2. Which resources violate policies
3. Common policy violation patterns
4. Recommended policy updates"
```

## Next Steps

- [Cluster Health Monitoring](cluster-health.md) - General monitoring
- [Troubleshooting](troubleshooting.md) - Debugging issues
- [Performance Tuning](performance-tuning.md) - Optimize cluster
- [Disaster Recovery](disaster-recovery.md) - Backup and recovery

---

**Remember:** Security should be enforced through policy, not through manual reviews!
