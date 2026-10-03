---
applyTo: "services/**/*.py,tests/**/*.py"
description: "FastAPI service, gateway, Python testing, and SageMaker adapter rules for Assessment IV."
---
# Python / FastAPI Instructions

- Keep HTTP handling, model-specific transformation, and AWS invocation separable and independently testable.
- Use Pydantic request/response models. Reject malformed requests with meaningful 4xx responses.
- Inject or wrap the SageMaker Runtime client so unit/integration tests can replace it without AWS.
- Set explicit connect/read timeouts where the client stack supports them.
- Convert downstream SageMaker errors into consistent API errors; do not leak raw stack traces.
- `/health` tests process liveness. `/ready` tests readiness/configuration and should fail when critical runtime configuration is absent.
- `/predict` tests must assert semantic output, not only status code.
- Use structured logging with service/team/request context where practical.
- Endpoint name and AWS region come from environment/config, never literals scattered through handlers.
- Add/update pytest tests with every behavior change.
- For gateway routes, test that each route reaches only the correct downstream service.
