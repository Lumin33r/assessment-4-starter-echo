---
name: sagemaker-model-delivery
description: "Use when creating, training, evaluating, deploying, invoking, troubleshooting, or documenting the three SageMaker models/endpoints required by Assessment IV."
---
# SageMaker Model Delivery

The assessment names three algorithm families. Prefer SageMaker built-in algorithms and small reproducible datasets.

Create model tooling under:

```text
ml/
  common/
  fraud/
  recommendations/
  forecasting/
```

Each model folder should eventually contain:
- README or notes describing the contract;
- data generation/acquisition/preprocessing;
- training/deployment script;
- representative invocation payload;
- verification script/test;
- teardown instructions or integration with project teardown.

## Common workflow

For each model:

1. Define the business prediction contract first.
2. Define raw/synthetic data schema.
3. Generate/acquire data reproducibly.
4. Split train/test or train/validation as appropriate.
5. Transform into the algorithm's required SageMaker format.
6. Upload prepared data to S3.
7. start a SageMaker training job using the managed algorithm container;
8. wait for completion and inspect metrics/logs;
9. create/deploy the SageMaker model to a small real-time endpoint;
10. invoke the endpoint directly with a representative payload;
11. validate semantics, not just successful HTTP/AWS response;
12. expose endpoint name/config to the FastAPI service;
13. record cost/teardown considerations.

Use deterministic random seeds for synthetic data.

## Fraud — XGBoost

### Goal
Binary classification: `fraudulent` vs `legitimate`, with a probability/score.

### Suggested synthetic schema
Generate several thousand transactions with features such as:
- transaction amount;
- hour of day;
- account age days;
- distance from usual location;
- transactions in last hour;
- merchant risk score;
- card-present flag;
- foreign transaction flag;
- failed attempts in last 24h.

Create the target using a probabilistic rule that increases fraud likelihood for plausible combinations, then add noise. Keep fraud relatively rare but ensure enough positive examples for learning/testing.

### API contract example
Input:
```json
{
  "amount": 850.0,
  "hour": 2,
  "account_age_days": 18,
  "distance_from_home_km": 900,
  "transactions_last_hour": 8,
  "merchant_risk": 0.92,
  "card_present": false,
  "foreign_transaction": true,
  "failed_attempts_24h": 3
}
```

Output should normalize the raw model result into something like:
```json
{
  "prediction": "fraudulent",
  "fraud_probability": 0.91
}
```

### Acceptance
- direct SageMaker invocation works;
- obvious low-risk/high-risk fixtures produce defensible relative behavior;
- service parser is unit tested.

## Recommendations — Factorization Machines

### Goal
Use Factorization Machines as a sparse interaction scorer. Do not overcomplicate this into a full production recommender.

### Suggested synthetic data
Create:
- 100–500 users;
- 50–200 items;
- a few user segments/preferences;
- a few item categories/attributes;
- positive interactions generated when preferences align, plus noise.

Encode a user-item pair into a sparse vector, for example:
- one-hot user ID;
- one-hot item ID;
- optional compact contextual/category features.

Train the Factorization Machines model for binary interaction/click/like probability.

### Serving pattern
For a recommendation request:
1. accept a `user_id` and optional candidate item IDs;
2. construct one sparse feature vector per candidate;
3. score candidates through the model;
4. sort descending;
5. return top K items and scores.

This keeps the endpoint genuinely model-backed while allowing the FastAPI adapter to perform ranking.

### Acceptance
- direct endpoint scores a known user-item pair;
- service ranks multiple candidate items;
- tests prove the recommendations route cannot invoke another team's endpoint.

## Forecasting — DeepAR

### Goal
Forecast future demand/usage from historical related time series.

### Suggested synthetic data
Generate multiple product/service time series with:
- daily or hourly frequency;
- base demand per series;
- weekly seasonality;
- trend;
- promotional/event spikes;
- random noise.

Use many related series rather than only one, because DeepAR learns across series.

Prepare the training data in the format expected by SageMaker DeepAR (for example JSON Lines), with start timestamp and target sequence; include categories/dynamic features only if they add clear value.

### API contract example
Input:
```json
{
  "series_id": "product-17",
  "start": "2026-01-01 00:00:00",
  "history": [42, 45, 39, 51, 60, 63, 55],
  "prediction_length": 7
}
```

Output should normalize SageMaker output into:
```json
{
  "series_id": "product-17",
  "forecast": [ ... ],
  "quantiles": {
    "p10": [ ... ],
    "p50": [ ... ],
    "p90": [ ... ]
  }
}
```

### Acceptance
- direct endpoint returns the expected forecast horizon;
- parser handles mean/quantile output;
- invalid history/frequency is rejected before invoking SageMaker where appropriate.

## Cost controls

- use small datasets;
- use modest CPU training/inference instances supported by the chosen algorithms;
- avoid hyperparameter tuning unless the core rubric is already verified;
- delete real-time endpoints after grading/demo when they are no longer needed;
- document how to recreate them.

## Do not

- create fake endpoint names and call the integration complete;
- substitute a generic LLM for the required ML workloads;
- train locally and claim it is a SageMaker training workflow unless the rubric/instructor explicitly allows it;
- leave model creation as an undocumented manual prerequisite.
