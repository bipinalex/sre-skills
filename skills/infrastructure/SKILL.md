---
name: infrastructure
description: Use when designing, reviewing, or troubleshooting cloud or on-prem infrastructure — compute sizing, networking topology, load balancing, DNS, TLS, storage strategy, auto-scaling, cost optimisation, or capacity planning. Triggers on "infrastructure design", "cloud architecture", "networking", "load balancer", "auto-scaling", "capacity planning", "cost optimisation", "VPC", "DNS", "TLS cert", "compute sizing", or any infrastructure question. Ships scripts for resource utilisation analyser, cost anomaly detector, and capacity planner. 4 references on cloud infra patterns, networking, cost optimisation, and capacity planning. NOT an IaC-writing skill — specifically infrastructure design and operation decisions.
version: 1.0.0
tags: [infrastructure, cloud, aws, gcp, azure, networking, capacity-planning, cost, sre]
---

# Infrastructure

Design and operate infrastructure that is right-sized, secure, and cost-efficient. Infrastructure decisions made at design time are expensive to undo — this skill forces the right questions upfront.

## When to use

- Designing network topology (VPC, subnets, peering, ingress)
- Choosing compute strategy (VM vs container vs serverless vs bare metal)
- Reviewing load balancer configuration (health checks, timeouts, SSL termination)
- Building auto-scaling policies (target tracking, step scaling, scheduled)
- Capacity planning for a launch, traffic event, or growth projection
- Auditing infrastructure costs and identifying optimisation opportunities

## When NOT to use

- Writing the IaC to provision the infra → use `iac`
- Observability instrumentation for the infra → use `observability`
- Kubernetes operator design → use engineering/kubernetes-operator

## Core principle: infrastructure should be invisible to the application

Good infra has:
- **No single points of failure** — every component has a failover path
- **Defence in depth** — security controls at network, compute, and data layers
- **Defined RTO/RPO** — recovery targets must be documented and tested, not assumed
- **Cost accountability** — every resource tagged; alerts on anomalous spend

## Compute decision matrix

| Workload | Recommended | Avoid |
|---|---|---|
| Stateless web service | Containers (ECS/GKE) or serverless | Large VMs |
| Stateful DB | Managed service (RDS/Cloud SQL) | DIY on VM |
| Batch / async | Serverless / spot instances | Always-on compute |
| ML training | GPU spot fleet | On-demand for long runs |
| Edge / low-latency | CDN + edge functions | Single-region |

## Scripts

```bash
# Analyse resource utilisation and flag over/under-provisioned instances
python scripts/utilisation_analyser.py --provider aws --region eu-west-1 --days 14

# Detect cost anomalies (spike detection against 30-day baseline)
python scripts/cost_anomaly_detector.py --provider aws --threshold-pct 20

# Run a capacity plan for a traffic event
python scripts/capacity_planner.py --service payments-api --baseline-rps 800 --peak-rps 3000 --duration-hours 4
```

## References

- [Cloud Infra Patterns](./references/cloud_infra_patterns.md) — multi-AZ, multi-region, failover, DR tiers
- [Networking](./references/networking.md) — VPC design, subnets, security groups, NACLs, peering, DNS
- [Cost Optimisation](./references/cost_optimisation.md) — rightsizing, reserved vs spot, tagging strategy, savings plans
- [Capacity Planning](./references/capacity_planning.md) — traffic modelling, growth projections, load testing, headroom targets
