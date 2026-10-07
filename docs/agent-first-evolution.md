# Adage's agent-first evolution

Adage remains a configuration-driven AWS deployment framework. The agent model extends its architecture with specifications, deterministic checks, scoped observation, and reviewable evidence.

## Architecture first, agent fit later

The [May 2025 documentation](https://github.com/usekarma/adage/commit/360597536ec4e283ee1d8f4aa9b0da377409ff49) already describes separate configuration and infrastructure repositories, reusable components, SSM dependency resolution, and Git-controlled changes. The motivation was understandable, composable cloud infrastructure. The current agent use case emerged from those choices; it did not originate them.

| Original choice | Agent engineering benefit |
| --- | --- |
| Declarative desired state | Compare intended instances with observed resources. |
| Configuration separate from implementation | Change intent without duplicating infrastructure code; inspect both sides independently. |
| Reusable components and predictable interfaces | Bound a change by component and nickname; test implementation independently. |
| Explicit environment/account boundaries | Make targets reviewable and verify identity before planning. |
| Git history and review | Trace intent and prepare auditable changes. |
| SSM discovery and runtime metadata | Follow dependencies without sharing Terraform state between components. |
| Minimal hidden coupling | Reduce assumptions and expose missing evidence. |

These properties support safe reasoning. SSM can contain stale or manually edited values; a nickname does not prove an environment; configuration presence does not enforce Git approval. IAM and execution controls must enforce authority separately.

## Roles and workflow

Adage defines the infrastructure control model. `aws-config` defines intent and environment bindings; `aws-iac` implements reusable Terraform/Terragrunt components. SSM bridges published inputs and runtime discovery. Karma is the workload on which this model is tested.

The [agent business solution template](https://github.com/usekarma/agent-business-solution-template) complements Adage with a general workflow: ambiguous objective → specification → agent work → deterministic verification → evidence → human release decision. It does not replace Terraform or grant AWS permissions.

An infrastructure task should record its objective, accounts/regions, component instances, acceptance criteria, permitted observations, and prohibited mutations. The agent inspects both repositories and history, proposes changes, runs checks, and prepares a plan and risk report. A human reviews evidence and separately authorizes consequential execution. Read-only post-change checks verify the outcome; operational observation tests whether expected savings and health actually follow.

## Authority is enforced, not inferred

Agents may inspect code and AWS read-only, develop components, modify local configuration, validate, plan, analyze cost/drift, and prepare pull requests. They must not self-authorize apply/destroy, SSM publishing, environment rebinding, data deletion, sensitive IAM/security changes, or irreversible operations.

Use restricted observation/planning credentials and a separately authorized execution path. Planning must not silently bootstrap or change a remote backend. Saved plans and raw evidence may contain sensitive values; public reports should redact account identifiers, configuration values, and secrets.

Implemented safeguards are under review in [aws-iac PR #1](https://github.com/usekarma/aws-iac/pull/1) and [aws-config PR #1](https://github.com/usekarma/aws-config/pull/1), rather than on `main`. They include agent instructions, target preflight, mutation guards, verification tools, and evidence guidance. Read their current limitations and check CI before relying on them. Legacy deployment commands remain mutating interfaces; an approval acknowledgement is not permission for an agent to grant itself authority.

## First cost proof

**Status: experiment to validate; no complete public end-to-end proof is claimed here.**

The owner-specified exact steady-state targets are $0.51/month for strall.com and $0.61/month for usekarma.dev, totaling $1.12/month. They describe the intended minimal state, not the cost of every historical Karma service. Their billing scope and assumptions require verification.

The objective is to explain every cent above that target and produce a safe remediation plan without destructive changes.

1. Derive expected instances and dependencies from versioned configuration and IaC; distinguish active intent from retained historical definitions.
2. Inventory relevant accounts and regions read-only. Record inaccessible scope, persistent resources, and discrepancies with Terraform/SSM metadata.
3. Retrieve billing evidence for an explicit period and cost basis. Separate billed spend from monthly forecasts; account for usage, shared charges, credits, taxes, and rounding as applicable.
4. Map charges to services/resources and back to configuration/IaC where possible. Preserve an unexplained residual when evidence is missing; never invent attribution.
5. Propose scoped remediation with expected monthly savings, dependencies, service impact, data-loss risk, recovery requirements, and required authorization.
6. Run deterministic checks and prepare relevant Terraform/Terragrunt plans. Missing access or failed plans remain blockers, not passing evidence.

The report should include repository revisions, observation timestamps, coverage, billing totals, target assumptions, an attribution ledger, resource ownership, proposed changes, check/plan results, and unresolved questions. Success requires reconciliation to billing precision with no unexplained excess and no unobserved scope silently treated as empty. If charges cannot be attributed or the target excludes unavoidable costs, report that result explicitly and revise assumptions with the owner.

No destructive operation is part of this proof. Savings remain estimates until a separately authorized change is followed by health checks and billing observation. Link a public, redacted evidence artifact here when one exists.

## What comes next

The future direction is repeatable desired/actual/cost reconciliation and broader component verification. Promotion depends on observed results, not agent confidence. The [existing quickstarts](../README.md#getting-started) remain the practical entry point; the agent workflow adds evidence and authority boundaries around infrastructure engineering.
