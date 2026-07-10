# DAX Measures — NHS RTT Dashboard

All measures live in a dedicated `_Measures` table. Below are the core measures with explanations.

---

## Total Waiting List
The headline backlog — sum of all patients across every wait band.
```dax
Total Waiting = SUM ( NHS_RTT[PatientCount] )
```

## Within 18 Weeks
Patients who have waited fewer than 18 weeks (the compliant group).
```dax
Within 18 Weeks =
CALCULATE (
    [Total Waiting],
    NHS_RTT[BandOrder] < 18
)
```

## % Within 18 Weeks — the 92% standard
The single most important KPI. Format as percentage.
```dax
% Within 18 Weeks =
DIVIDE (
    [Within 18 Weeks],
    [Total Waiting]
)
```

## Long-waiter breaches
Patients waiting beyond the critical thresholds.
```dax
52 Week Breaches =
CALCULATE ( [Total Waiting], NHS_RTT[BandOrder] >= 52 )

65 Week Breaches =
CALCULATE ( [Total Waiting], NHS_RTT[BandOrder] >= 65 )

78 Week Breaches =
CALCULATE ( [Total Waiting], NHS_RTT[BandOrder] >= 78 )
```

## Performance Status (RAG flag)
Classifies performance against the standard for conditional formatting.
```dax
Performance Status =
VAR pct = [% Within 18 Weeks]
RETURN
    SWITCH ( TRUE (),
        pct >= 0.92, "On Target",
        pct >= 0.75, "At Risk",
        "Underperforming"
    )
```

## Month-over-month trend (time intelligence)
Requires a marked Date table and multiple months of data.
```dax
Waiting List MoM =
VAR curr = [Total Waiting]
VAR prev =
    CALCULATE ( [Total Waiting], DATEADD ( DimDate[Date], -1, MONTH ) )
RETURN curr - prev
```

---

## Key Power Query steps (ETL)
1. **Folder query** — connect to a folder, not a single file (refreshable pipeline)
2. **Remove Top Rows** — strip the 13 NHS title/notes rows
3. **Use First Row as Headers** — promote the real header
4. **Unpivot Other Columns** — collapse ~96 wide week-band columns into `WeeksBand` + `PatientCount`
5. **Filter** — remove pre-aggregated total/% rows (rebuilt as DAX measures)
6. **BandOrder** — numeric sort key so wait bands order correctly
7. **WaitBandGroup** — bucket 52 bands into 5 (Under 18 / 18-36 / 36-52 / 52-65 / 65+)
