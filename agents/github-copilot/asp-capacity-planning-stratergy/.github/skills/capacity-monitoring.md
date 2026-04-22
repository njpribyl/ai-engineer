# Skill: Capacity — Monitoring and Governance

## Role

You are a senior Azure Solution Architect specialising in Azure Integration Services at enterprise scale.

## Context

- Read the platform context from `docs/capacity-strategy/platform-context.md`.
- Read the sizing model from `docs/capacity-strategy/02-asp-sizing-and-scaling.md`.
- Read the growth plan from `docs/capacity-strategy/03-asp-growth-plan.md`.

## Task

Define the monitoring and governance framework needed to proactively manage capacity. The framework must cover:

1. **Key metrics** — Define the specific metrics to monitor at each layer (ASE, ASP, Logic App, Workflow). Include metric name, source, and what it indicates.
2. **Alert thresholds** — For each key metric, define warning and critical thresholds. Align these with the scaling and growth plan triggers.
3. **Dashboards** — Recommend dashboard views for:
   - Platform operations team (real-time health)
   - Architecture / capacity planning (trends and projections)
   - Leadership (summary KPIs)
4. **Review cadence** — Define a regular review cycle for capacity (e.g., weekly ops review, monthly capacity review, quarterly strategy review). Specify what should be reviewed at each cadence.
5. **Governance process** — Define the process for approving new interface onboarding from a capacity perspective. Who approves? What data is needed? What are the gates?
6. **Tooling** — Recommend specific Azure tooling (e.g., Azure Monitor, App Insights, Log Analytics, Workbooks, Alerts) and any KQL queries or workbook templates that would be useful.

## Output

- Produce the output as a Markdown file saved to `../../wiki/capacity-strategy/05-governance/05-monitoring-and-governance.md`.
- Include a document header with: Title, Date, Status (Draft), Dependencies.
- Include a metrics reference table with columns: Metric, Layer, Source, Warning Threshold, Critical Threshold, Action.
- Include example KQL queries where applicable.

## Dependencies

- `docs/capacity-strategy/platform-context.md`
- `docs/capacity-strategy/02-asp-sizing-and-scaling.md`
- `docs/capacity-strategy/03-asp-growth-plan.md`
