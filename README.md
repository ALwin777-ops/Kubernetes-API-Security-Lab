# Kubernetes API Security Lab

A hands-on Kubernetes security lab focused on API audit logging, Role-Based Access Control (RBAC), least-privilege authorization, and validation of denied API requests.

## Project Overview

This project demonstrates how Kubernetes API activity can be audited and how RBAC policies can restrict a service account to the minimum permissions required. A dedicated Kind cluster provides an isolated environment for testing authorization behavior and reviewing audit evidence.

## Architecture

    Administrator (kubectl)
              |
              v
      Kubernetes API Server
          /           \
         v             v
    RBAC checks     Audit Policy
       |                |
       v                v
    Pod access       Audit Log
    Allowed / Denied   |
         |              v
         v         Security investigation
    rbac-lab namespace
         |
         v
    security-auditor
    (least privilege)

## Security Controls Implemented

- Configured Kubernetes API server audit logging.
- Created a dedicated rbac-lab namespace.
- Created the security-auditor service account.
- Defined a Role permitting get, list, and watch on Pods only.
- Bound the Role to the service account using a RoleBinding.
- Tested allowed and denied actions using kubectl auth can-i.
- Simulated a denied Secret-listing request using identity impersonation.
- Verified HTTP 403 Forbidden in the audit log.

## Validation Results

| Security test | Observed result |
|---|---|
| List Pods as security-auditor | Allowed |
| Get Secrets as security-auditor | Denied |
| Create Deployments as security-auditor | Denied |
| Attempt to list Secrets | HTTP 403 Forbidden |
| Audit log records denied request | Confirmed |
| NGINX test Pod | Running and Ready |

## Tools and Technologies

- Kubernetes, Kind, kubectl
- Kubernetes RBAC and API audit logging
- Docker, YAML, PowerShell

## Repository Structure

- audit/audit-policy.yaml
- cluster/kind-config.yaml
- rbac/auditor-rbac.yaml
- evidence/validation-results.md
- README.md

## Scope and Limitations

This local lab demonstrates RBAC authorization and API audit visibility. Authorization tests use kubectl --as impersonation and do not independently validate authentication using a service-account credential. This is not a comprehensive Kubernetes API penetration test.

## Future Improvements

- Test authentication with a dedicated, short-lived service-account token.
- Add automated RBAC regression tests.
- Evaluate posture with kube-bench or Kubescape.
- Forward audit events to centralized monitoring.

## Disclaimer

For educational and authorized security testing only.
