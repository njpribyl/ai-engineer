# Skill: Capacity — ASP Growth Plan

## Role

You are a senior Azure Solution Architect specialising in Azure Integration Services at enterprise scale.

## Context

- Read the platform context from `docs/capacity-strategy/platform-context.md`.
- Read the current state assessment from `docs/capacity-strategy/01-current-state-assessment.md`.
- Read the sizing model from `docs/capacity-strategy/02-asp-sizing-and-scaling.md`.

## Task

Define a growth plan for introducing new ASPs as interfaces are onboarded. The plan must address:

1. **Capacity thresholds** — Define the trigger points at which a new ASP should be provisioned. Consider multiple signals: CPU/memory utilisation, interface count, workflow count, and scale-out frequency.
2. **Growth timeline** — Map the expected onboarding rate against the current capacity to project when new ASPs will be needed. Present this as a phased plan (e.g., Month 1-3, Month 4-6, Month 7-12, Month 13-24).
3. **Provisioning lead time** — Factor in change control, testing, and deployment lead times. Recommend how far in advance ASPs should be provisioned relative to the trigger points.
4. **ASE limits** — Identify any ASE-level limits (e.g., max ASPs per ASE, total instance count) that may require introducing additional ASEs, and define the trigger for that decision.
5. **Scaling decision tree** — Provide a simple decision tree or flowchart logic for the operations team to follow when capacity thresholds are approached.

## Output

- Produce the output as a Markdown file saved to `docs/capacity-strategy/03-asp-growth-plan.md`.
- Include a document header with: Title, Date, Status (Draft), Dependencies.
- Include a phased capacity projection table.
- Include a decision tree in text or Mermaid diagram format.
- Where data is missing, state assumptions clearly and flag for validation.

## Dependencies

- `docs/capacity-strategy/platform-context.md`
- `docs/capacity-strategy/01-current-state-assessment.md`
- `docs/capacity-strategy/02-asp-sizing-and-scaling.md`
