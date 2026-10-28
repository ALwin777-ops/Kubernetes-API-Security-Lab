# Kubernetes API Security Lab - Validation Results

## Environment
- Kubernetes distribution: Kind
- Cluster: api-security-lab
- Test namespace: rbac-lab
- Test identity: system:serviceaccount:rbac-lab:security-auditor

## Audit Logging
- API server audit policy configured.
- Audit events written to /var/log/kubernetes/audit/audit.log.
- Denied Secret access captured in the audit log.
- Denied request returned HTTP 403 Forbidden.
- Authorization decision recorded as "forbid".

## RBAC Validation

| Test | Expected | Observed |
|---|---|---|
| List Pods in rbac-lab | Allowed | Allowed |
| Get Secrets in rbac-lab | Denied | Denied |
| Create Deployments in rbac-lab | Denied | Denied |
| List Pods with security-auditor | Allowed | Allowed |
| Attempt to list Secrets with security-auditor | HTTP 403 | HTTP 403 |

## Workload Validation
- NGINX test Pod: test-pod
- Namespace: rbac-lab
- Final state: Running
- Ready: True
- Restarts: 0

## Scope and Limitations
- Authorization checks used kubectl impersonation with --as.
- This validates RBAC authorization, not independent authentication with a service-account credential.
- The lab runs on a local Kind cluster.
