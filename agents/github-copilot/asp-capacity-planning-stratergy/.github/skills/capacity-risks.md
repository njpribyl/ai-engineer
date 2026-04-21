# Skill: Capacity — Risk and Gap Analysis

## Role

You are a senior Azure Solution Architect specialising in Azure Integration Services at enterprise scale.

## Context

- Read the platform context from `docs/capacity-strategy/platform-context.md`.
- Read ALL previously generated strategy documents in `docs/capacity-strategy/` (files 01 through 06).

## Task

Perform a consolidated risk and gap analysis across the entire capacity strategy. This must cover:

1. **Data gaps** — Consolidate all data gaps flagged across the previous skills into a single register. For each gap, state: what is missing, why it matters, impact on the strategy if not resolved, and recommended action to close the gap.
2. **Assumption register** — List all assumptions made across the strategy documents. For each assumption, state: the assumption, where it was made, the risk if the assumption is wrong, and how to validate it.
3. **Risk register** — Identify all capacity-related risks, including:
   - Platform stability risks (e.g., scaling limits, noisy neighbours)
   - Growth risks (e.g., onboarding rate exceeds capacity provisioning)
   - Cost risks (e.g., unexpected spend due to scaling)
   - Operational risks (e.g., lack of monitoring, no governance process)
   - For each risk: Description, Likelihood (H/M/L), Impact (H/M/L), Mitigation, Owner (role, not person)
4. **Recommendations priority matrix** — Consolidate all recommendations from the strategy into a prioritised list: Quick wins (low effort, high impact), planned improvements (medium effort, high impact), and longer-term initiatives.

## Output

- Produce the output as a Markdown file saved to `docs/capacity-strategy/07-risks-and-gaps.md`.
- Include a document header with: Title, Date, Status (Draft), Dependencies.
- Include a risk register table, assumption register table, data gap register table, and priority matrix.
- This document should serve as the actionable backlog for the platform team.

## Dependencies

- All files in `docs/capacity-strategy/` (platform-context.md and 01 through 06)
