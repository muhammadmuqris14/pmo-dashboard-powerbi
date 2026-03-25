# PMO Portfolio Performance Dashboard
**Tool:** Power BI Desktop · DAX · Excel  
**Type:** Self-initiated · Built independently  
**Context:** Personal PMO tracking tool built alongside active government ICT project delivery

---
## Overview

Built this dashboard independently to maintain structured visibility across concurrent government ICT projects — supplementing WhatsApp-based team updates with a proper visual tracking layer.

The dashboard was not assigned work. I identified the gap in project visibility, designed the data model, built the dataset, wrote all 16 DAX measures, and assembled the full report layout from scratch in Power BI Desktop.

---

## Dashboard Visuals

| Visual | Type | Purpose |
|---|---|---|
| Portfolio Health Overview | Donut Chart | Project health distribution at a glance |
| Project Progress Comparison | Clustered Bar | Completion % side by side across projects |
| Budget vs Actual Cost | Line Chart | Spend tracking over time |
| Budget Variance by Project | Clustered Bar | Over/under budget per project |
| Project Risk Exposure | Clustered Bar | Issues and risks stacked per project |
| Issues by Project Manager | Clustered Bar | Accountability view — issues per PM |
| Projects by Project Manager | Clustered Bar | Workload distribution across PMs |
| Project Summary Table | Pivot Table | Full project register: dates, status, completion %, issues, risks |
| Portfolio Insights Panel | Text Cards | Dynamic DAX-generated written summary (see below) |

### KPI Cards (7)
- Total Projects
- On Track
- Delayed
- At Risk
- Total Issues
- Total Risks
- Projects Over Budget

### Slicers (3)
- Status
- Department
- Project Manager

---

## DAX Measures (16 Total)

### Status Counters
```dax
Total Projects = COUNTROWS('dataset')

On Track = CALCULATE(COUNTROWS('dataset'), 'dataset'[Status] = "On Track")

Delayed = CALCULATE(COUNTROWS('dataset'), 'dataset'[Status] = "Delayed")

At Risk = CALCULATE(COUNTROWS('dataset'), 'dataset'[Status] = "At Risk")
```

### Issue & Risk Tracking
```dax
Total Issues = SUM('dataset'[Number of Issues])

Total Risks = SUM('dataset'[Number of Risks])
```

### Budget Measures
```dax
Total Budget = SUM('dataset'[Budget (RM)])

Total Actual Cost = SUM('dataset'[Actual Cost (RM)])

Budget Variance = [Total Actual Cost] - [Total Budget]

Budget Variance % = DIVIDE([Budget Variance], [Total Budget], 0)

Projects Over Budget =
CALCULATE(
    COUNTROWS('dataset'),
    FILTER(
        'dataset',
        'dataset'[Actual Cost (RM)] > 'dataset'[Budget (RM)]
    )
)
```

### Completion
```dax
Avg Completion % = AVERAGE('dataset'[Completion %])
```

### Portfolio Insights Panel (Dynamic Text Measures)
These four measures auto-generate written portfolio health statements that update live when slicers change:

```dax
Insight Completion =
VAR AC = FORMAT([Avg Completion %], "0.0")
RETURN
"📊 Average portfolio completion is " & AC & "%"

Insight Delay Rate =
VAR DelayPct = DIVIDE([Delayed], [Total Projects], 0) * 100
RETURN
"⚠ " & FORMAT(DelayPct, "0") & "% of projects are currently delayed"

Insight Over Budget =
VAR OB = [Projects Over Budget]
RETURN
"💰 " & OB & " projects are exceeding planned budget"

Insight Top Risk =
VAR TopProject =
    TOPN(1,
        SUMMARIZE('dataset',
            'dataset'[Project Name],
            "TotalRisk", [Total Issues] + [Total Risks]),
        [TotalRisk], DESC)
VAR ProjectName = MAXX(TopProject, 'dataset'[Project Name])
RETURN
"🔴 Highest risk: " & ProjectName
```

`Insight Top Risk` uses `TOPN` + `SUMMARIZE` to dynamically identify and surface the highest-risk project by combined issue and risk count — updates automatically with slicer context.

---

## Data Model

Flat fact table with the following fields:

| Field | Description |
|---|---|
| Project Name | Project identifier |
| Project Manager | Responsible PM |
| Department | Owning department |
| Status | On Track / At Risk / Delayed |
| Project Health | Health classification for donut chart |
| Start Date | Planned start date |
| End Date | Planned end date |
| Completion % | Task completion percentage |
| Budget (RM) | Planned budget |
| Actual Cost (RM) | Actual spend to date |
| Number of Issues | Open issues count |
| Number of Risks | Open risks count |

---

## Why I Built This

During active project delivery, progress updates happened mainly through WhatsApp messages and periodic meetings. There was no single view of where projects stood at any given time — who was behind, which project was burning budget, or which PM had the highest issue load.

I built this to answer those questions at a glance without waiting for a status meeting. It was used personally as a tracking supplement, not a client deliverable — but it directly shaped how I monitored and reported on project status week to week.

---

## Files in This Repo

| File | Description |
|---|---|
| `Project Portfolio Performance Dashboard.pbix` | Full Power BI file — open in Power BI Desktop |
| `dashboard-preview.png` | Screenshot of the main report page |
| `README.md` | This file |

---

## Skills Demonstrated

`Power BI` `DAX` `CALCULATE` `FILTER` `TOPN` `SUMMARIZE` `Data Modelling` `KPI Design` `Budget Variance Tracking` `Dynamic Text Measures` `PMO Reporting` `Self-Directed Learning`

---

## Related Repositories

- [Project Management Portfolio](https://github.com/muhammadmuqris14/project-management-portfolio) — Main portfolio with full case studies
- [Project Scheduling – Microsoft Project](https://github.com/muhammadmuqris14/project-scheduling-msproject) — MS Project schedule samples
- [RAID Log Template](https://github.com/muhammadmuqris14/pmo-raid-log-template) — Risk, Assumption, Issue, Dependency tracker
- [Weekly Status Report Template](https://github.com/muhammadmuqris14/pmo-status-report-template) — PMO reporting template
- [Project Task Tracker](https://github.com/muhammadmuqris14/project-task-tracker) — Excel-based task tracking

---

*Built by Muhammad Muqris bin Shazly — Project Coordinator / PMO Analyst*  
*Contact: muhammadmuqris14@gmail.com · [LinkedIn](https://www.linkedin.com/in/muhammad-muqris-shazly-0080a3257/)*
