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

| ASP Name | Logic App Resources Hosted | Total Workflows | Total Interfaces | Notes |
| -------- | -------------------------- | --------------- | ---------------- | ----- |
| asp-chip-realtime-01      |  90 | 92 | 88 | Highest transactional load; receives most API-triggered traffic. ~1 workflow per resource to maximise isolation for real-time messaging |
| asp-chip-batch-01         |  55 | 54 | 50 | Nightly ETL and periodic sync flows; each batch interface deployed as its own Logic App resource |
| asp-chip-orchestration-01 |  55 | 46 | 26 | Fewer interfaces but multiple supporting Logic App resources per orchestration (parent + child workflows); heavier per-workflow compute footprint |
| **Total**                 | **200** | **192** | **164** | Matches 200 Logic App Standard resources declared in Environment table |

---

## Current Performance

> One row per ASP. Capture steady-state and peak values.

| ASP Name | Avg CPU % | Peak CPU % | Avg Memory % | Peak Memory % | Scale-Out Frequency | Notes |
| -------- | --------- | ---------- | ------------ | ------------- | ------------------- | ----- |
| asp-chip-realtime-01 | 57 | 86 | 49 | 71 | 3-5 times daily | Peaks align with outpatient check-in windows |
| asp-chip-batch-01 | 34 | 63 | 37 | 55 | 2-3 times weekly | Most load concentrated between 01:00-04:00 UTC |
| asp-chip-orchestration-01 | 66 | 93 | 61 | 82 | Daily | XSLT and PDF transform stages drive sustained memory pressure |

### Known Performance Issues

| Issue | Affected ASP(s) | Impact | Notes |
| ----- | ---------------- | ------ | ----- |
| Scale-out reaches max instances during Monday morning surge | asp-chip-realtime-01 | Elevated end-to-end latency (p95 +28%) for 30-45 minutes | Usually occurs 08:00-09:30 local time |
| Orchestration workflows occasionally queue behind large transform jobs | asp-chip-orchestration-01 | Delayed downstream notifications and SLA risk for non-urgent feeds | Investigating workload isolation by interface class |

---

## Planned Growth
| Data Point                         | Value                          |
| ---------------------------------- | ------------------------------ |
| Interfaces to onboard              | 1800                            |
| Source platform being replaced     | Legacy Mirth Connect + Mule 4 hybrid estate |
| Migration timeline                 | 18 months                      |
| Expected onboarding rate           | 30-45 per month                |
| Interface complexity mix           | 58% light, 32% medium, 10% heavy |

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
