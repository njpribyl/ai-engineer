# Platform Context — Capacity Strategy

> **Example:** This is a sample completed context file showing realistic values.
> Replace values with your environment data before using for formal planning.

---

## Platform Overview

- **Platform Name:** Contoso Health Integration Platform (CHIP)
- **Platform Purpose:** Azure-native integration platform for clinical and operational messaging between EMR, LIMS, pharmacy, scheduling, and analytics systems.
- **Cloud Region(s):** UK South (primary), UK West (DR)

---

## Current Architecture

### Environment

| Component                              | Value                          |
| -------------------------------------- | ------------------------------ |
| App Service Environment version        | ASEv3                          |
| Number of ASPs                         | 3                              |
| Number of Logic App Standard resources | 200                             |
| Total live interfaces                  | 164                            |

### App Service Plans

> One row per ASP. Add or remove rows as needed.

| ASP Name | SKU | Min Instances | Max Instances | Autoscale Enabled | Purpose / Workload Description |
| -------- | --- | ------------- | ------------- | ----------------- | ------------------------------ |
| asp-chip-realtime-01 | I2v2 | 4 | 14 | Yes | Real-time ADT/ORM/ORU message processing and API-triggered workflows |
| asp-chip-batch-01 | I1v2 | 2 | 8  | Yes | Scheduled extracts, bulk synchronization, and reconciliation jobs |
| asp-chip-orchestration-01 | I3v2 | 3 | 9  | Yes | Multi-step orchestration, document transforms, and partner routing |

### Autoscale Rules

> One block per ASP. If multiple rules exist per ASP, add a row for each.

| ASP Name | Rule # | Metric | Operator | Threshold | Direction | Instance Change | Cooldown (mins) |
| -------- | ------ | ------ | -------- | --------- | --------- | --------------- | --------------- |
| asp-chip-realtime-01 | 1 | CPU % | > | 68 | Scale Out | +2 | 5 |
| asp-chip-realtime-01 | 2 | CPU % | < | 32 | Scale In  | -1 | 10 |
| asp-chip-batch-01 | 1 | CPU % | > | 72 | Scale Out | +1 | 5 |
| asp-chip-batch-01 | 2 | CPU % | < | 35 | Scale In  | -1 | 15 |
| asp-chip-orchestration-01 | 1 | Memory % | > | 70 | Scale Out | +1 | 5 |
| asp-chip-orchestration-01 | 2 | Memory % | < | 40 | Scale In  | -1 | 15 |

### Logic App Distribution

> How Logic App resources are currently spread across ASPs.

### Logic App Distribution

> How Logic App resources are currently spread across ASPs. Distribution calibrated from workflow-telemetry-baseline.csv.

| ASP Name | Logic App Resources Hosted | Total Workflows | Total Interfaces | Avg P95Sec | Notes |
| -------- | -------------------------- | --------------- | ---------------- | --------- | ----- |
| asp-chip-realtime-01      |  72 | 92 | 88 | 2.0 | Real-time + API-triggered: High throughput (CDP, SFSC, SFMC, Okta systems). P95 < 2s indicates CPU-bound workloads. ~620 peak runs/5min |
| asp-chip-batch-01         |  58 | 54 | 50 | 6.5 | Batch/Scheduled: Mix of polling (1nav, monitoring) and async transforms. P95 < 10s; lower throughput clusters |
| asp-chip-orchestration-01 |  70 | 46 | 26 | 45.0 | Heavy orchestration + transforms: Blob engine, customer invoice, consent transforms. P95 > 30s indicates memory-intensive workflows |
| **Total**                 | **200** | **192** | **164** | - | All Logic App Standard resources across ASPs |

---

## Current Performance

> One row per ASP. Calibrated from workflow-telemetry-baseline.csv analysis (30-day baseline).

| ASP Name | Avg CPU % | Peak CPU % | Avg Memory % | Peak Memory % | Scale-Out Frequency | Notes |
| -------- | --------- | ---------- | ------------ | ------------- | ------------------- | ----- |
| asp-chip-realtime-01 | 62 | 89 | 44 | 68 | 4-6 times daily | High-throughput real-time workflows (P95 1-2s, peak 620/5min). CPU-bound; memory low due to short duration |
| asp-chip-batch-01 | 38 | 65 | 41 | 57 | 2-3 times weekly | Batch/polling workflows (P95 5-8s). Moderate throughput; scales mainly for sustained load windows |
| asp-chip-orchestration-01 | 71 | 95 | 67 | 86 | Daily + unscheduled | Heavy transforms + orchestration (P95 30-45s, outliers to 1800s). Memory and CPU both high; less responsive to scaling |

### Known Performance Issues

| Issue | Affected ASP(s) | Impact | Notes |
| ----- | ---------------- | ------ | ----- |
| Peak real-time throughput (620/5min) approaching scaling limits | asp-chip-realtime-01 | CPU spikes to 89%; autoscale lag causes p95 latency +35% for 20–40 mins | Occurs during business hours (07:00-09:00 local) |
| Long-duration workflows blocking orchestration capacity | asp-chip-orchestration-01 | Sustained CPU 71% baseline; memory spikes to 86%; queuing observed on lower-priority workflows | Customer invoice workflows (24.12s avg) dominate; transform logic needs optimization |
| SNAT port exhaustion on outbound integrations | asp-chip-realtime-01 | Intermittent connection resets for OAuth/API workflows during peak | Trex, Okta, Salesforce integrations affected; 10-15 failures/hour at peak |

---

## Planned Growth

| Data Point                         | Value                          |
| ---------------------------------- | ------------------------------ |
| Interfaces to onboard              | 640 (net new from migration + organic growth) |
| Source platform being replaced     | Legacy integration middleware (batch ETL, point-to-point APIs) |
| Migration timeline                 | 18 months                      |
| Expected onboarding rate           | 30-45 per month                |
| Interface complexity mix           | 65% light (<1s), 25% medium (5-10s), 10% heavy (30-100s) |
| **Telemetry Profile** | Derived from 30-day baseline; 159 unique workflows analyzed |
| **Highest throughput workflow** | wf-customer-identity-cdp: 620 peak/5min, 529k runs/month, 1.35s avg |
| **Heaviest workflow** | wf-customer-invoice: 24.12s avg, 1805s p99, concurrent blocking observed |

---

## Constraints
| Constraint                         | Detail                         |
| ---------------------------------- | ------------------------------ |
| Budget sensitivity                 | High                           |
| Environments in scope              | Production, Pre-Production, DR |
| Regulatory requirements            | UK data residency, DSPT-aligned controls, ISO 27001 |
| Change control lead time           | 10 business days (CAB + security review) |
| Monitoring tooling in place        | Azure Monitor, Application Insights, Log Analytics, Action Groups |
| ASE tenancy model                  | Single-tenant                  |
| Known platform limits being hit    | SNAT port pressure on two outbound-heavy workflows during peak windows |
| Reserved instances in use          | Partial (1-year reservations on I2v2 only) |

---

## Telemetry Data Source

| Field | Details |
| ----- | ------- |
| Data Collection Period | Last 30 days (baseline from workflow-telemetry-baseline.csv) |
| Unique Logic App Resources | 40+ (grouped across 3 ASPs) |
| Unique Workflows | 159 |
| Total Workflow Executions | 3.2M+ |
| Query Method | Application Insights KQL query (AppTraces / WorkflowRunEnd events) |
| Key Aggregations | Per-workflow throughput (TotalRuns, P95RunsPer5Min, PeakRunsPer5Min), duration percentiles (P50/P95/P99Sec) |

**Top 3 Workflows by Throughput:**
1. wf-customer-identity-cdp: 529k runs, 68/5min avg, 620/5min peak
2. wf-customer-identity-crm-sales: 320k runs, 45/5min avg, 416/5min peak
3. wf-customer-identity-mktg-consent: 277k runs, 36/5min avg, 418/5min peak

**Top 3 Workflows by Duration:**
1. wf-customer-invoice: 24.12s avg, 1805s p99 (blocking orchestration pipeline)
2. wf-mktg-consent-transform: 29.15s avg, 205s p99 (transform-heavy)
3. wf-mktg-http-oauth-reqresp: 2.9s avg, 13s p95 (auth flows)
