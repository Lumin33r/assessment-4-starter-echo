---
name: rubric-acceptance-review
description: "Use before presentation/submission or whenever auditing progress against Assessment IV. Performs a requirement-by-requirement gap analysis and creates a prioritized completion plan."
---
# Rubric Acceptance Review

Audit the actual repository and, when access is available, deployed evidence.

For each rubric line item classify:

- `VERIFIED` — implementation plus executable evidence;
- `IMPLEMENTED` — code/config exists but evidence is incomplete;
- `PARTIAL` — some required behavior exists;
- `MISSING` — no credible implementation;
- `BLOCKED` — external dependency prevents verification.

For each non-verified item identify:
1. exact gap;
2. smallest corrective task;
3. dependencies;
4. verification command/test;
5. rubric value/priority.

Prioritize:
1. missing core requirements;
2. broken end-to-end integration;
3. testing/evidence;
4. documentation;
5. bonus points.

Never prioritize bonus work above an unverified core requirement.

End with a presentation-ready demonstration sequence:
`architecture -> infrastructure -> endpoints -> Kubernetes -> gateway -> dashboard -> CI/CD -> failure demonstration -> teardown/reproducibility`.
