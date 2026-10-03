# Copilot Guidance Pack — Assessment IV

Copy the contents of this pack into the root of the student's existing repository.

Key files:

- `.github/copilot-instructions.md` — always-on project delivery contract.
- `.github/instructions/*.instructions.md` — path-specific implementation rules.
- `.github/skills/*/SKILL.md` — task-specific workflows Copilot can load when relevant.
- `docs/MODEL_STRATEGY.md` — recommended model approach.
- `docs/RUBRIC_TRACEABILITY_TEMPLATE.md` — starting evidence matrix.

## First prompt to give the agent

After copying these files into the repository, ask Copilot Agent:

> Audit this repository against Assessment IV. Read the repository instructions and relevant skills. Do not start by generating missing components. First create/update `docs/IMPLEMENTATION_STATUS.md` and `docs/RUBRIC_TRACEABILITY.md`, identify the nearest incomplete vertical slice, and propose the smallest implementation step that will produce new executable end-to-end evidence. Then implement and verify that step.

## Recommended first milestone

Make **fraud/XGBoost** the reference vertical slice. It should become the pattern the other two teams follow.

Target path:

`synthetic fraud data -> SageMaker XGBoost training -> real endpoint -> fraud FastAPI adapter -> Docker -> EKS -> gateway route -> dashboard status/test request -> GitHub Actions verification`

Only after this works should the agent replicate the pattern for recommendations and forecasting.
