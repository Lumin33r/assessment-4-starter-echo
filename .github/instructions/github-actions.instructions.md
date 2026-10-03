---
applyTo: "k8s/**/*.yaml,k8s/**/*.yml"
description: "Kubernetes/EKS orchestration and verification rules for Assessment IV."
---
# Kubernetes Instructions

Every Kubernetes change must preserve the assessment's operational intent.

Required:
- namespace separation per team/workload;
- Deployment and Service for each FastAPI adapter;
- ConfigMaps for non-sensitive configuration;
- no real secret values committed;
- startup, readiness, and liveness probes with distinct purposes;
- resource requests/limits;
- ResourceQuota and LimitRange per required namespace;
- labels/selectors that are consistent and testable.

Probe semantics:
- startup: allow initialization without premature restarts;
- liveness: detect a wedged process;
- readiness: remove a pod from traffic when it cannot safely serve.

Do not make `/health` depend on SageMaker if that would cause a transient AWS issue to restart healthy application processes.

After manifest changes, validate syntax and perform an available client/server dry run. After deployment, verify rollout, pod state, endpoints, and service behavior.

Prefer IAM roles/workload identity for AWS access rather than static AWS credentials stored in Kubernetes Secrets.
