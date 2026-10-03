---
name: kubernetes-platform-delivery
description: "Use when designing, implementing, debugging, or verifying EKS/Kubernetes namespaces, deployments, services, probes, quotas, limits, RBAC, ConfigMaps, Secrets, and AWS access."
---
# Kubernetes Platform Delivery

## Build from a proven container

Do not begin by generating all Kubernetes manifests for all teams.

For the current vertical slice:
1. prove the service container locally;
2. create namespace/config;
3. create Deployment and Service;
4. configure AWS identity/access;
5. add startup/readiness/liveness probes;
6. add resource requests/limits;
7. deploy;
8. verify rollout and service behavior;
9. only then replicate the pattern.

## Namespace model

Use clear ownership, for example:
- `fraud-team`
- `recommendations-team`
- `forecasting-team`
- optional `platform` for gateway/dashboard

Ensure quotas/limits align with this layout.

## AWS access

Prefer pod/workload IAM integration supported by the cluster over static AWS access keys. The service needs only the permissions required to invoke its intended SageMaker endpoint where practical.

## Verification

After changes, inspect:
- `kubectl get ns`
- `kubectl get resourcequota,limitrange -A`
- `kubectl get deploy,pods,svc -A`
- `kubectl describe` for failures
- rollout status
- logs
- service endpoints
- representative request

Do not interpret `Running` as proof that prediction works.
