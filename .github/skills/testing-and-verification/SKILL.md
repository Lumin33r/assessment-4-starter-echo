---
name: testing-and-verification
description: "Use when adding tests, diagnosing integration gaps, proving rubric requirements, performing acceptance testing, or deciding whether a feature is actually complete."
---
# Testing and Verification

## Test from inside out

For a changed vertical slice, run in this order:

1. pure unit tests;
2. FastAPI/service tests with mocked dependencies;
3. contract tests;
4. container/local integration tests;
5. direct SageMaker endpoint verification when relevant;
6. Kubernetes deployment checks;
7. gateway smoke test;
8. dashboard/API operational check;
9. end-to-end business scenario.

Stop at the first meaningful failure and diagnose it. Do not bury a broken lower layer under more generated code.

## Required business scenarios

Maintain representative fixtures for:

### Fraud
- clearly low-risk transaction;
- clearly high-risk transaction;
- malformed transaction;
- SageMaker timeout/error.

### Recommendations
- valid user with candidate items;
- unknown/invalid user or candidate handling;
- ranking semantics;
- SageMaker timeout/error.

### Forecasting
- valid history and forecast horizon;
- invalid/insufficient history;
- correct forecast length;
- SageMaker timeout/error.

## Routing assertions

Explicitly prove:
- `/fraud/...` reaches fraud service only;
- `/recommendations/...` reaches recommendation service only;
- `/forecasting/...` reaches forecasting service only.

A generic 200 response is insufficient.

## Kubernetes acceptance evidence

Capture commands/results for:
- namespaces;
- quotas/limit ranges;
- deployments/pods;
- services/endpoints;
- rollout status;
- readiness;
- one controlled failure scenario.

## CI acceptance evidence

A successful workflow must demonstrate:
- tests ran;
- image built;
- image pushed;
- deployment updated;
- rollout completed;
- post-deploy verification ran.

## Failure injection

Prefer one simple, explainable demonstration:
- intentionally point a test deployment at an invalid endpoint and show readiness/propagated error behavior; or
- use an invalid readiness condition to show traffic gating; or
- attempt a pod exceeding ResourceQuota and show rejection.

Restore healthy state and document both the failure and recovery.
