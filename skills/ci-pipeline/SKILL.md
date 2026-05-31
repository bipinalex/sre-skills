---
name: ci-pipeline
description: Use when designing, reviewing, or debugging CI pipelines — build stages, test parallelism, caching strategies, security scanning, artifact management, or pipeline-as-code patterns. Triggers on "CI pipeline", "GitHub Actions", "GitLab CI", "Jenkins", "build pipeline", "test stages", "artifact cache", "SAST", "dependency scan", "pipeline optimization", or any continuous integration question. Ships scripts for pipeline YAML validator, stage dependency analyzer, and cache effectiveness checker. 4 references on pipeline design, security scanning, caching, and parallelism. NOT a CD/deployment skill — specifically the build-test-scan portion of the pipeline.
version: 1.0.0
tags: [ci, continuous-integration, github-actions, gitlab-ci, jenkins, pipeline, sast, sre]
---

# CI Pipeline

Design and operate CI pipelines that are fast, reliable, and secure. The build is a promise — if it's green, the artifact is safe to deploy.

## When to use

- Designing pipeline stages for a new service
- Reviewing an existing pipeline for speed, security gaps, or flaky test patterns
- Implementing SAST, dependency scanning, or secret detection in CI
- Optimising cache hit rates and parallelism
- Writing reusable workflow/pipeline templates
- Debugging pipeline failures (cache misses, flaky tests, permission errors)

## When NOT to use

- Deployment automation after the build → use `cd-pipeline`
- Infrastructure provisioning in pipelines → use `iac`
- Git branching strategy that feeds the pipeline → use `git-workflows`

## Core principle: CI is the quality gate, not the slow lane

A pipeline that takes 45 minutes is a pipeline people skip. Targets:

- **P50 build time < 10 min** for most services
- **Zero secrets in pipeline logs** — use masked env vars or secrets managers
- **Every PR gets the same pipeline** — no shortcuts on feature branches
- **Fail fast** — lint and unit tests before expensive integration tests

## Stage order (standard SRE pipeline)

```
[checkout] → [lint/format] → [unit tests] → [build artifact]
    → [SAST / secret scan] → [dependency audit] → [integration tests]
    → [publish artifact] → [notify]
```

## Scripts

```bash
# Validate pipeline YAML syntax and common anti-patterns
python scripts/pipeline_validator.py .github/workflows/ci.yml

# Analyse stage dependencies and find parallelism opportunities
python scripts/stage_analyzer.py .github/workflows/ci.yml

# Report cache hit rate from pipeline logs
python scripts/cache_checker.py --log-file pipeline.log
```

## References

- [Pipeline Design](./references/pipeline_design.md) — stage ordering, fail-fast, artifact promotion
- [Security Scanning](./references/security_scanning.md) — SAST tools, secret detection, dependency audit
- [Caching Strategies](./references/caching.md) — layer caches, dependency caches, cache invalidation
- [Parallelism Patterns](./references/parallelism.md) — matrix builds, test splitting, fan-out/fan-in
