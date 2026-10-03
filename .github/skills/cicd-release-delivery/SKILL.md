---
name: cicd-release-delivery
description: "Use when creating or fixing GitHub Actions workflows for testing, Docker/ECR build and push, EKS deployment, verification, rollback, branch targeting, or infrastructure lifecycle."
---
# CI/CD Release Delivery

Build CI/CD after a deployment path can be executed manually. Automation should encode a known-good process.

## Minimum workflow

1. checkout;
2. configure language/runtime;
3. install dependencies;
4. lint/test;
5. authenticate to AWS;
6. authenticate to ECR;
7. build image(s);
8. tag with immutable identifier such as commit SHA;
9. push;
10. configure kubectl for EKS;
11. update/apply manifests;
12. wait for rollout;
13. smoke-test;
14. fail the workflow if verification fails.

## Strong assessment additions

When core delivery works, consider:
- path filters/matrix for services;
- reusable workflows;
- dev/main branch targeting;
- deployment environments;
- rollback on failed smoke test;
- Terraform plan workflow;
- explicit destroy workflow;
- manifest lint/validation workflow.

Do not add advanced workflow choreography before one build-to-deploy path is proven.
