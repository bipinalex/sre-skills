---
name: iac
description: Use when writing, reviewing, or debugging Infrastructure as Code — Terraform modules, Ansible playbooks, Pulumi stacks, CloudFormation templates, or Helm charts. Triggers on "terraform", "ansible", "pulumi", "cloudformation", "helm chart", "IaC", "infrastructure as code", "module design", "state management", "drift detection", "policy as code", or any IaC question. Ships scripts for Terraform plan analyzer, IaC security scanner (misconfiguration detector), and module dependency mapper. 4 references on Terraform patterns, Ansible best practices, state management, and policy-as-code. NOT a cloud architecture skill — specifically writing and operating IaC.
version: 1.0.0
tags: [iac, terraform, ansible, pulumi, cloudformation, helm, sre, infra-automation]
---

# Infrastructure as Code

Write IaC that is readable, testable, and safe to apply at 2am. Infrastructure should be boring — predictable state, clear diffs, no surprises.

## When to use

- Writing Terraform modules for reusable infra patterns
- Reviewing IaC for security misconfigurations (open security groups, public buckets, missing encryption)
- Designing Terraform workspace and state management strategy
- Writing Ansible playbooks for configuration management
- Building Helm charts for Kubernetes workloads
- Implementing policy-as-code with OPA/Sentinel/Checkov

## When NOT to use

- Cloud architecture design decisions → use `infrastructure`
- Kubernetes operator development → use engineering/kubernetes-operator
- CD pipeline that applies the IaC → use `cd-pipeline`

## Core principle: IaC is code — treat it like code

- **Module boundaries** — modules are reusable units with typed inputs and documented outputs; never copy-paste blocks
- **Remote state** — always use remote state with locking (S3 + DynamoDB, GCS, Terraform Cloud)
- **Plan before apply** — `terraform plan` output is a required release artifact; never apply blindly
- **Drift is a bug** — config drift between IaC and live infra means something changed outside the pipeline

## Terraform module layout

```
modules/
└── {name}/
    ├── main.tf          # Resources
    ├── variables.tf     # Typed inputs with validation blocks
    ├── outputs.tf       # Exposed values
    ├── versions.tf      # Provider + Terraform version constraints
    └── README.md        # auto-generated from terraform-docs
```

## Scripts

```bash
# Analyse a terraform plan output for risk (destructive changes, sensitive diffs)
python scripts/plan_analyzer.py --plan-file tfplan.json --risk-threshold medium

# Scan IaC files for security misconfigurations
python scripts/iac_scanner.py --path ./infra --tool checkov

# Map module dependencies across a Terraform monorepo
python scripts/module_mapper.py --root ./infra
```

## References

- [Terraform Patterns](./references/terraform_patterns.md) — module design, workspace strategy, state backends
- [Ansible Best Practices](./references/ansible_best_practices.md) — role structure, idempotency, vault secrets
- [State Management](./references/state_management.md) — remote state, locking, state import/rm, workspace isolation
- [Policy as Code](./references/policy_as_code.md) — Checkov, OPA, Sentinel, Terrascan rule writing
