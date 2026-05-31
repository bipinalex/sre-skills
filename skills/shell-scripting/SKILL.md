---
name: shell-scripting
description: Use when writing or reviewing Bash/Python automation scripts for SRE tasks — health checks, log parsing, on-call runbooks, cron jobs, deployment wrappers, or secret rotation scripts. Triggers on "bash script", "shell script", "python automation", "cron job", "runbook script", "log parser", "health check script", "on-call automation", or any scripting task in a Linux/Unix environment. Ships scripts for script linting/style checking, a runbook templater, and a cron expression validator. 4 references on Bash best practices, Python for SRE, error handling patterns, and script security. NOT a programming tutorial — specifically SRE-context automation patterns.
version: 1.0.0
tags: [bash, shell, python, scripting, automation, runbook, sre, cron]
---

# Shell Scripting

Opinionated Bash and Python scripting for SRE automation. Focus is on correctness, safety, and operability — scripts that don't silently fail at 3am.

## When to use

- Writing health-check or readiness scripts for services
- Automating on-call runbook steps (drain, restart, validate, alert)
- Parsing logs to extract signals or build reports
- Wrapping deployment commands with retry, rollback, and notification logic
- Writing cron jobs with proper error handling and alerting
- Rotating secrets or certificates on a schedule

## When NOT to use

- Full application development (use proper frameworks)
- CI/CD pipeline YAML → use `ci-pipeline`
- Terraform/Ansible automation → use `iac`

## Core principle: scripts must be safe to run twice

Every SRE script must be:

- **Idempotent** — running it again doesn't cause harm
- **Loud on failure** — exit non-zero, log the reason, alert if critical
- **Auditable** — log what it did, when, and as whom
- **Bounded** — timeouts on network calls; never hang indefinitely

## Bash safety header (always use this)

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

# Trap errors with context
trap 'echo "ERROR: line $LINENO, exit code $?" >&2' ERR
```

## Scripts

```bash
# Lint a shell script for common SRE anti-patterns
python scripts/script_linter.py path/to/script.sh

# Generate a runbook script template for a named failure scenario
python scripts/runbook_templater.py --scenario "pod-crashloop" --service payments-api

# Validate a cron expression and show next 5 run times
python scripts/cron_validator.py "*/15 * * * *"
```

## References

- [Bash Best Practices](./references/bash_best_practices.md) — set flags, trap patterns, quoting, arrays
- [Python for SRE](./references/python_for_sre.md) — subprocess, pathlib, logging, retry patterns
- [Error Handling Patterns](./references/error_handling.md) — exit codes, retries, alerting hooks
- [Script Security](./references/script_security.md) — injection risks, secret handling, least privilege
