---
name: cd-pipeline
description: Use when designing deployment pipelines, release strategies, rollback mechanisms, environment promotion workflows, or progressive delivery patterns. Triggers on "CD pipeline", "deployment pipeline", "blue-green", "canary deploy", "rolling update", "progressive delivery", "environment promotion", "rollback strategy", "release automation", "GitOps deploy", or any continuous delivery / deployment question. Ships scripts for deployment strategy evaluator, rollback readiness checker, and environment drift detector. 4 references on deployment strategies, environment management, progressive delivery, and release gates. NOT a CI/build skill — specifically deployment and release automation.
version: 1.0.0
tags: [cd, continuous-delivery, continuous-deployment, blue-green, canary, rollback, progressive-delivery, sre]
---

# CD Pipeline

Release automation that minimises blast radius. The goal is confidence — deploy frequently, detect problems early, roll back in under 5 minutes.

## When to use

- Designing a deployment strategy for a new service (blue-green, canary, rolling)
- Implementing environment promotion gates (dev → staging → prod)
- Writing rollback automation triggered by SLO violations
- Setting up progressive delivery with feature flags or traffic splitting
- Reviewing a CD pipeline for missing safety gates
- Automating release notes and change management records

## When NOT to use

- Build and test automation → use `ci-pipeline`
- IaC provisioning of the deployment infrastructure → use `iac`
- SLO definition for the release gate → use `slo-architect`

## Core principle: every deployment must be reversible in < 5 minutes

Deployment strategy selection:

| Strategy | Downtime | Rollback speed | Resource cost | Best for |
|---|---|---|---|---|
| Rolling update | None | Slow (re-deploy previous) | Low | Stateless services |
| Blue-green | None | Instant (DNS/LB flip) | 2× compute | Services with hard rollback SLO |
| Canary | None | Fast (shift traffic back) | Low | High-traffic, risk-sensitive |
| Recreate | Brief | N/A | Low | Batch jobs, non-critical |

## Release gates (never skip these)

1. CI pipeline green on the exact artifact SHA being deployed
2. Smoke test passes in staging
3. Error budget not already exhausted (SLO check)
4. On-call engineer acknowledges deployment during business hours

## Scripts

```bash
# Evaluate which deployment strategy fits a service's risk profile
python scripts/strategy_evaluator.py --service payments-api --slo-target 99.9 --traffic-rps 1200

# Check rollback readiness (previous artifact available, runbook exists, rollback tested)
python scripts/rollback_checker.py --service payments-api --env prod

# Detect config drift between environments
python scripts/env_drift_detector.py --source staging --target prod
```

## References

- [Deployment Strategies](./references/deployment_strategies.md) — blue-green, canary, rolling, recreate decision tree
- [Environment Management](./references/environment_management.md) — promotion gates, config management, secrets per env
- [Progressive Delivery](./references/progressive_delivery.md) — feature flags, traffic shifting, Argo Rollouts
- [Release Gates](./references/release_gates.md) — SLO checks, smoke tests, change freeze windows
