# Platform Context — Capacity Strategy

> **Instructions:** Fill in the values below before running the capacity strategy agent.
> This file is referenced by all skills as the shared input. Keep it up to date as the platform evolves.

---

## Platform Overview

- **Platform Name:** [e.g., NHS Connect iPaaS / Mars HIP]
- **Platform Purpose:** [Brief description of what the platform does]
- **Cloud Region(s):** [e.g., UK South, UK West]

---

## Current Architecture

### Environment

| Component                              | Value                          |
| -------------------------------------- | ------------------------------ |
| App Service Environment version        | ASEv3                          |
| Number of ASPs                         | 3                              |
| Number of Logic App Standard resources | [e.g., 15]                    |
| Total live interfaces                  | [e.g., 120]                   |

### App Service Plans

> One row per ASP. Add or remove rows as needed.

| ASP Name | SKU | Min Instances | Max Instances | Autoscale Enabled | Purpose / Workload Description |
| -------- | --- | ------------- | ------------- | ----------------- | ------------------------------ |
| [e.g., asp-integration-01] | I1v2 | [e.g., 3] | [e.g., 10] | [Yes / No] | [e.g., High-throughput message interfaces] |
| [e.g., asp-integration-02] | I1v2 | [e.g., 2] | [e.g., 8]  | [Yes / No] | [e.g., Batch / scheduled interfaces] |
| [e.g., asp-integration-03] | I2v2 | [e.g., 3] | [e.g., 6]  | [Yes / No] | [e.g., Complex orchestration / heavy transforms] |

### Autoscale Rules

> One block per ASP. If multiple rules exist per ASP, add a row for each.

| ASP Name | Rule # | Metric | Operator | Threshold | Direction | Instance Change | Cooldown (mins) |
| -------- | ------ | ------ | -------- | --------- | --------- | --------------- | --------------- |
| [asp-integration-01] | 1 | CPU % | > | [e.g., 70] | Scale Out | [e.g., +1] | [e.g., 5] |
| [asp-integration-01] | 2 | CPU % | < | [e.g., 30] | Scale In  | [e.g., -1] | [e.g., 10] |
| [asp-integration-02] | 1 | CPU % | > | [e.g., 75] | Scale Out | [e.g., +2] | [e.g., 5] |
| [asp-integration-02] | 2 | CPU % | < | [e.g., 40] | Scale In  | [e.g., -1] | [e.g., 10] |
| [asp-integration-03] | 1 | [e.g., Memory %] | > | [e.g., 65] | Scale Out | [e.g., +1] | [e.g., 5] |
| [asp-integration-03] | 2 | [e.g., Memory %] | < | [e.g., 35] | Scale In  | [e.g., -1] | [e.g., 10] |

### Logic App Distribution

> How Logic App resources are currently spread across ASPs.

| ASP Name | Logic App Resources Hosted | Total Workflows | Total Interfaces | Notes |
| -------- | -------------------------- | --------------- | ---------------- | ----- |
| [asp-integration-01] | [e.g., 5] | [e.g., 40] | [e.g., 40] | |
| [asp-integration-02] | [e.g., 6] | [e.g., 50] | [e.g., 50] | |
| [asp-integration-03] | [e.g., 4] | [e.g., 30] | [e.g., 30] | |

---

## Current Performance

> One row per ASP. Capture steady-state and peak values.

| ASP Name | Avg CPU % | Peak CPU % | Avg Memory % | Peak Memory % | Scale-Out Frequency | Notes |
| -------- | --------- | ---------- | ------------ | ------------- | ------------------- | ----- |
| [asp-integration-01] | [e.g., 55] | [e.g., 85] | [e.g., 40] | [e.g., 65] | [e.g., multiple times daily] | |
| [asp-integration-02] | [e.g., 35] | [e.g., 60] | [e.g., 30] | [e.g., 50] | [e.g., weekly] | |
| [asp-integration-03] | [e.g., 70] | [e.g., 92] | [e.g., 55] | [e.g., 75] | [e.g., daily] | [e.g., heavy transforms causing CPU spikes] |

### Known Performance Issues

| Issue | Affected ASP(s) | Impact | Notes |
| ----- | ---------------- | ------ | ----- |
| [e.g., Frequent scale-out hitting max instances] | [asp-integration-01] | [e.g., Latency spikes during peak] | |
| [describe or N/A] | | | |

---

## Planned Growth

| Data Point                         | Value                          |
| ---------------------------------- | ------------------------------ |
| Interfaces to onboard              | [e.g., 1800]                  |
| Source platform being replaced     | [e.g., MuleSoft]              |
| Migration timeline                 | [e.g., 12-24 months]          |
| Expected onboarding rate           | [e.g., 50-100 per month]      |
| Interface complexity mix           | [e.g., 60% light, 30% medium, 10% heavy] |

---

## Constraints

| Constraint                         | Detail                         |
| ---------------------------------- | ------------------------------ |
| Budget sensitivity                 | [High / Medium / Low / Unknown] |
| Environments in scope              | [e.g., Prod, Pre-Prod, DR]   |
| Regulatory requirements            | [e.g., NHS DSPT, data residency] |
| Change control lead time           | [e.g., 2 weeks CAB approval]  |
| Monitoring tooling in place        | [e.g., Azure Monitor, App Insights, Log Analytics] |
| ASE tenancy model                  | [Single-tenant / Shared]       |
| Known platform limits being hit    | [describe or N/A]             |
| Reserved instances in use          | [Yes / No / Partial]          |
