---
name: git-workflows
description: Use when designing branching strategies, writing Git automation scripts, reviewing merge/rebase practices, managing releases with tags, handling GitOps patterns, or debugging history problems. Triggers on "branching strategy", "trunk-based", "gitflow", "rebase vs merge", "git hooks", "release tagging", "gitops", "commit conventions", "monorepo git", or any git workflow question. Ships scripts for branch health checking, commit lint validation, and tag/release automation. 4 references covering branching models, commit standards, release workflows, and GitOps patterns. NOT a general git tutorial — specifically workflow design and automation.
version: 1.0.0
tags: [git, branching, gitops, release, hooks, commit-conventions, sre]
---

# Git Workflows

Production git practices for SRE teams. Covers branching models, commit hygiene, release automation, and GitOps patterns — the decisions that actually matter at scale.

## When to use

- Choosing or evaluating a branching strategy (trunk-based, Gitflow, GitHub Flow)
- Writing pre-commit or pre-push git hooks
- Designing release tagging and versioning automation
- Implementing GitOps with Git as the source of truth for infra state
- Debugging messy histories (force-push damage, diverged branches, lost commits)
- Setting up commit message conventions (Conventional Commits, semantic versioning)

## When NOT to use

- Learning git basics (clone, commit, push) — use official git docs
- CI/CD pipeline setup → use `ci-pipeline`
- IaC state management → use `iac`

## Core principle: Git is the audit trail

For SRE work, every infrastructure change, config drift, and deployment trigger must be traceable to a commit. This means:

- **Atomic commits** — one logical change per commit; rollback must be possible at commit granularity
- **Signed commits** — GPG or SSH signing for production-affecting repos
- **Protected main/master** — no direct pushes; PR + CI gate enforced
- **Tags are immutable** — never force-push a release tag

## Branching models at a glance

| Model | Best for | Risk |
|---|---|---|
| Trunk-based | High-frequency deploy teams (multiple per day) | Requires solid feature flags |
| GitHub Flow | SaaS, always-deployable services | Long-lived PRs accumulate drift |
| Gitflow | Regulated environments, scheduled releases | Branch rot, complex merges |

## Scripts

```bash
# Check branch health (stale branches, unmerged work)
python scripts/branch_health.py --repo . --stale-days 30

# Validate commit messages against Conventional Commits spec
python scripts/commit_lint.py --from HEAD~10

# Automate a release tag with changelog extraction
python scripts/release_tagger.py --bump minor --dry-run
```

## References

- [Branching Models](./references/branching_models.md) — trunk-based vs Gitflow vs GitHub Flow decision tree
- [Commit Conventions](./references/commit_conventions.md) — Conventional Commits spec, enforcement tools
- [Release Workflows](./references/release_workflows.md) — tagging, changelogs, semantic versioning
- [GitOps Patterns](./references/gitops_patterns.md) — Flux, ArgoCD, pull-vs-push reconciliation
