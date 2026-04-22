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

## Data Collection Query

If per-workflow telemetry is not available in the platform context, use this KQL query against Application Insights to populate gaps G1 and G2 (per-workflow execution counts, duration, and arrival rates over the last 30 days):

```kusto
let BaseRuns =
    AppTraces
    | where TimeGenerated >= startofday(ago(30d))
    | where tostring(Properties.EventName) == "WorkflowRunEnd"
    | extend ParsedProps = parse_json(tostring(Properties.prop__properties))
    | extend Workflow = tostring(ParsedProps.resource.workflowName)
    | extend LogicApp = tostring(AppRoleName)
    | extend DurationMs = todouble(Properties.prop__durationInMilliseconds)
    | where isnotempty(Workflow);
let Performance =
    BaseRuns
    | summarize
        TotalRuns = count(),
        MinSec = round(min(DurationMs) / 1000.0, 2),
        AvgSec = round(avg(DurationMs) / 1000.0, 2),
        P50Sec = round(percentile(DurationMs, 50) / 1000.0, 2),
        P95Sec = round(percentile(DurationMs, 95) / 1000.0, 2),
        P99Sec = round(percentile(DurationMs, 99) / 1000.0, 2),
        MaxSec = round(max(DurationMs) / 1000.0, 2)
      by LogicApp, Workflow;
let ArrivalRates =
    BaseRuns
    | summarize RunsPer5Min = count() by LogicApp, Workflow, TimeBin = bin(TimeGenerated, 5m)
    | summarize
        AvgRunsPer5Min = round(avg(RunsPer5Min), 2),
        P95RunsPer5Min = round(percentile(RunsPer5Min, 95), 2),
        PeakRunsPer5Min = max(RunsPer5Min)
      by LogicApp, Workflow;
Performance
| join kind=leftouter ArrivalRates on LogicApp, Workflow
| project
    LogicApp,
    Workflow,
    TotalRuns,
    AvgRunsPer5Min,
    P95RunsPer5Min,
    PeakRunsPer5Min,
    AvgSec,
    P50Sec,
    P95Sec,
    P99Sec,
    MinSec,
    MaxSec
| order by TotalRuns desc
```

Export results to CSV and use to calibrate ASP sizing and workload isolation decisions in Steps 2 and 4.

## Dependencies

- `docs/capacity-strategy/platform-context.md`
