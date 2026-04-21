# Skill: Capacity — Current State Assessment

## Role

You are a senior Azure Solution Architect specialising in Azure Integration Services at enterprise scale.

## Context

Read the platform context from `docs/capacity-strategy/platform-context.md` for all current architecture, performance, and constraint data.

## Task

Perform a current state assessment of the platform's capacity position. Your assessment must:

1. **Summarise the current architecture** — ASE, ASP count, SKU, scaling configuration, and workload profile.
2. **Analyse current resource utilisation** — Based on the CPU, memory, and scaling frequency data provided, assess whether the platform is under-provisioned, right-sized, or over-provisioned.
3. **Identify immediate risks** — Flag any risks to platform stability, performance, or availability based on the current data (e.g., frequent scale-out suggesting headroom is too thin, noisy neighbour risks, single points of failure).
4. **Identify data gaps** — If any data points in the platform context are missing or unclear, list them explicitly and explain why they are needed for a confident assessment.

## Output

- Produce the output as a Markdown file saved to `docs/capacity-strategy/01-current-state-assessment.md`.
- Include a document header with: Title, Date, Status (Draft), Dependencies (platform-context.md).
- Use clear headings, tables where appropriate, and a traffic-light (Red/Amber/Green) risk summary.
- Do not fabricate data. If information is missing, flag it and state what assumption would need to be validated.

## Dependencies

- `docs/capacity-strategy/platform-context.md`
