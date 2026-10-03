---
applyTo: "dashboard/**/*.{js,jsx,ts,tsx,css,html}"
description: "React internal operations dashboard rules for Assessment IV."
---
# React Dashboard Instructions

This UI is an internal platform operations dashboard.

Show operational truth from APIs rather than hardcoded demo state.

Minimum information:
- model/service;
- owning team;
- health/readiness;
- version or endpoint/model metadata when available.

Implement at least two:
- live polling;
- version display;
- request counts;
- test-request interface.

Handle loading, partial failure, and unavailable services visibly. One unhealthy team service must not make the entire dashboard unusable.

Keep model-specific prediction forms small and appropriate to the business contract. The dashboard should demonstrate platform operability, not become a large consumer product.
