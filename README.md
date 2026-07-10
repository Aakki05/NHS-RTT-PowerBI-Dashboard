# NHS Elective Care — RTT Waiting List Performance Dashboard

A Power BI dashboard analysing NHS England **Referral to Treatment (RTT)** waiting list data, built to monitor the **18-week standard**, long-waiter breaches, and specialty-level performance across providers.

> Built on **real NHS England open data** (Consultant-led RTT Waiting Times, Incomplete Pathways).

---


##  The Problem

As of early 2026, the NHS elective waiting list sits at roughly **7.4 million pathways**. Every acute trust in England is measured against one operational standard: **at least 92% of patients waiting to start treatment should have waited under 18 weeks**, with a government target to reach that by March 2029.

This dashboard answers the questions a trust performance team asks every month:
- How many patients are waiting, and how does that compare to the 92% standard?
- Which specialties are the biggest bottlenecks?
- Where are the long-waiter (52+ week) breaches concentrated?

---

##  Key Insights

- **~7.4M total incomplete pathways** — the national waiting list.
- **~64.7% within 18 weeks** — well below the 92% standard, a ~27 percentage-point gap.
- **~90K patients waiting 52+ weeks** — the politically critical long-waiters.
- **Trauma & Orthopaedics, ENT, and Ophthalmology** carry the largest waiting lists — matching NHS England's published priority specialties.
- Volume and performance are **different problems**: some specialties fail the standard badly despite smaller lists, requiring targeted rather than blanket intervention.

---

##  Tools & Techniques

| Area | What I did |
|------|------------|
| **Power Query (ETL)** | Folder-based ingestion for refreshable pipeline; removed NHS title rows; **unpivoted 96 wide week-band columns** into a tidy fact table; removed pre-aggregated columns to rebuild in DAX; built a `BandOrder` sort key |
| **Data modelling** | Tidy fact-table design; sortable wait-band grouping aligned to NHS breach thresholds (18/52/65 weeks) |
| **DAX** | `CALCULATE`, `DIVIDE`, `SWITCH`, time intelligence; measures for the 18-week standard, breaches, and performance status |
| **Visualisation** | KPI cards, target-line charts, conditional-format heatmap, interactive slicers |
| **Domain** | NHS RTT reporting, the 18-week standard, breach analysis, accessibility |

---

##  Core Metrics

- **% within 18 weeks** — the national standard (target ≥92%)
- **Total waiting list** — headline backlog
- **52 / 65 / 78-week breaches** — long-waiter tracking
- **Waiting list by specialty** — bottleneck identification

---

##  Data Source

**NHS England — Consultant-led Referral to Treatment (RTT) Waiting Times**, Incomplete Pathways (Provider level). Publicly available open data:
`https://www.england.nhs.uk/statistics/statistical-work-areas/rtt-waiting-times/`

---

##  What This Demonstrates

- End-to-end BI development: raw open data → cleaned model → executive dashboard
- Data engineering discipline (unpivoting, refreshable pipelines, avoiding double-counting)
- Domain-aware analysis aligned to real NHS performance standards
- Design for a non-technical audience (trust boards / performance teams)

---

*Built with Microsoft Power BI. Data is public NHS England open data; this is a personal portfolio project and is not affiliated with the NHS.*
