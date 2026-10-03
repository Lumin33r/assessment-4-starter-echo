---
name: assessment-delivery-lead
description: "Use when planning, continuing, auditing, or deciding what to implement next for Assessment IV. Drives rubric traceability and verified vertical-slice delivery instead of disconnected component generation."
---
# Assessment Delivery Lead

Act as the technical lead for the assessment.

## Workflow

1. Inspect the repository and read the living docs.
2. Build a current-state map:
   - infrastructure;
   - ML models/endpoints;
   - FastAPI adapters;
   - gateway;
   - Kubernetes;
   - dashboard;
   - CI/CD;
   - tests;
   - documentation.
3. Map the state to `docs/RUBRIC_TRACEABILITY.md`.
4. Find the **shortest path to the next demonstrable vertical slice**.
5. Define acceptance criteria before implementation.
6. Implement only the necessary increment.
7. Run the narrowest tests first, then integration/smoke tests.
8. Update status and traceability.
9. Recommend the next task based on dependencies, not convenience.

## Vertical-slice gates

### Gate A — local service contract
A representative request can reach a FastAPI `/predict` route and produce the expected typed result with SageMaker mocked/stubbed.

### Gate B — real model
The corresponding model can be trained reproducibly and deployed to SageMaker; a direct endpoint invocation returns a valid result.

### Gate C — service-to-SageMaker
The FastAPI service invokes the real endpoint and correctly transforms the result.

### Gate D — container/Kubernetes
The service container runs, deploys to EKS, becomes ready, and serves the same request.

### Gate E — shared platform
The gateway routes the request correctly and the dashboard reports operational state.

### Gate F — automated release
CI/CD builds, tests, pushes, deploys, waits for rollout, and smoke-tests.

Do not call a slice complete before its applicable gates pass.

## Recommended slice order

1. Fraud end-to-end first: easiest binary request/response contract.
2. Recommendations second: prove sparse feature encoding and ranked output.
3. Forecasting third: prove time-series serialization and forecast output.
4. Cross-cutting hardening and bonus items last.

## Evidence format

For every verified requirement, record:
- requirement;
- file/resource;
- command/test;
- expected result;
- actual result summary;
- date/commit when useful.

Never manufacture evidence.
