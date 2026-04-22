# Skill: Capacity — Workload Distribution Strategy

## Role

You are a senior Azure Solution Architect specialising in Azure Integration Services at enterprise scale.

## Context

- Read the platform context from `../../../../../wiki/capacity-strategy/context/platform-context.md`.
- Read the current state assessment from `../../../../../wiki/capacity-strategy/01-assessments/01-current-state-assessment.md`.
- Read the sizing model from `../../../../../wiki/capacity-strategy/02-recommendations/02-asp-sizing-and-scaling.md`.

## Task

Define how interfaces should be distributed across ASPs and Logic App Standard resources. The strategy must address:

1. **Logic App resource boundaries** — How many workflows should be grouped within a single Logic App Standard resource? What are the practical limits and the recommended ceiling?
2. **ASP assignment model** — Define the criteria for assigning a Logic App resource to a specific ASP. Consider:
   - Workload profile (CPU/memory intensity)
   - Criticality tier (P1 vs. P2 vs. P3)
   - Business domain or interface owner
   - Shared vs. isolated hosting requirements
3. **Noisy neighbour mitigation** — How should high-throughput or resource-intensive interfaces be isolated to prevent impact on other interfaces?
4. **Rebalancing process** — Define a process for rebalancing workloads across ASPs when utilisation becomes uneven.
5. **Naming and tagging conventions** — Recommend conventions that make it easy to identify which interfaces are on which ASP and Logic App resource.

## Output

- Produce the output as a Markdown file saved to `../../../../../wiki/capacity-strategy/04-distribution/04-workload-distribution.md`.
- Include a document header with: Title, Date, Status (Draft), Dependencies.
- Include a distribution model diagram or table.
- Include a worked example showing how a batch of new interfaces would be assigned.

## Dependencies

- `../../../../../wiki/capacity-strategy/context/platform-context.md`
- `../../../../../wiki/capacity-strategy/01-assessments/01-current-state-assessment.md`
- `../../../../../wiki/capacity-strategy/02-recommendations/02-asp-sizing-and-scaling.md`
