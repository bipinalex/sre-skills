---
name: observability
description: Use when designing or reviewing metrics, logging, tracing, or alerting for a service or platform. Triggers on "observability", "metrics", "logging", "tracing", "alerting", "dashboards", "Prometheus", "Grafana", "OpenTelemetry", "structured logging", "alert fatigue", "on-call noise", "distributed tracing", "log aggregation", or any monitoring/observability question. Ships scripts for alert quality scorer, log schema validator, and dashboard coverage checker. 4 references on the three pillars, alerting philosophy, structured logging, and OpenTelemetry instrumentation. NOT an SLO/error-budget skill — specifically instrumentation, signal design, and alert hygiene.
version: 1.0.0
tags: [observability, metrics, logging, tracing, alerting, prometheus, grafana, opentelemetry, sre]
---

# Observability

Instrument systems so that when things break at 3am, the on-call engineer knows what is broken, why, and how to fix it — without digging through raw logs for an hour.

## When to use

- Designing the metrics/logging/tracing strategy for a new service
- Reducing on-call alert fatigue (too many pages, too many false positives)
- Reviewing dashboards for signal vs noise
- Implementing structured logging and a log schema
- Instrumenting services with OpenTelemetry
- Building a centralised logging or tracing platform (Loki, Tempo, Jaeger, Zipkin)

## When NOT to use

- Defining SLOs and error budgets → use `slo-architect`
- Responding to an active incident → use engineering/incident-response
- Infrastructure cost monitoring → use `infrastructure`

## Core principle: the three pillars serve one goal — faster MTTR

```
Metrics   ⟶  Are we in trouble? (fast, cheap, aggregated)
Logs      ⟶  What happened? (detailed, expensive, noisy)
Traces    ⟶  Where is the latency? (distributed, request-scoped)
```

Use metrics to page, logs to diagnose, traces to localise. Never build dashboards from logs alone — log queries under load will make your bad day worse.

## Alert design rules

1. **Every alert must have a runbook** — no alert fires without a link to "what to do"
2. **Alert on symptoms, not causes** — page on error rate, not on CPU
3. **Multi-window burn rate for SLO alerts** — short window catches spikes, long window catches slow burns
4. **< 5 pages per on-call shift** is the target — more means you have monitoring, not observability

## Instrumentation checklist (per service)

- [ ] RED metrics exposed: Rate, Errors, Duration
- [ ] Structured JSON logs with `trace_id`, `span_id`, `service`, `level`, `message`
- [ ] Distributed trace context propagated across all downstream calls
- [ ] At least one dashboard covering RED metrics + infra health
- [ ] Alert on P99 latency breach and error rate breach (linked to SLO)

## Scripts

```bash
# Score existing alerts for quality (actionability, runbook presence, noise ratio)
python scripts/alert_scorer.py --rules-file prometheus-rules.yml

# Validate log output against a structured schema
python scripts/log_schema_validator.py --log-sample logs/sample.json --schema schemas/service-log.json

# Check dashboard coverage against a list of services
python scripts/dashboard_coverage.py --grafana-url http://grafana:3000 --services services.txt
```

## References

- [Three Pillars](./references/three_pillars.md) — metrics vs logs vs traces, when to use each, tooling landscape
- [Alerting Philosophy](./references/alerting_philosophy.md) — symptom-based alerting, alert fatigue, runbook design
- [Structured Logging](./references/structured_logging.md) — log schema design, correlation IDs, log levels, retention
- [OpenTelemetry](./references/opentelemetry.md) — auto vs manual instrumentation, collector config, exporter setup
