# 📊 Workbooks

Microsoft Sentinel Workbooks provide interactive dashboards for security monitoring and visualization.

## security-overview-workbook.json

A custom security monitoring workbook covering:

- **Sign-in Activity Overview** — Sign-in volume over time, failed vs successful, top users
- **Failed Login Heatmap** — Failed logins by hour and day of week
- **Geographic Sign-in Map** — World map of sign-in origins
- **Top Alert Categories** — Distribution of fired analytics rules
- **Incident Trend** — Incidents opened/closed over time by severity
- **Privileged Role Changes** — Timeline of all role assignment events

## How to Import

```
Sentinel → Workbooks → + Add workbook
→ Edit (pencil icon) → Advanced Editor (</>)
→ Paste content of security-overview-workbook.json
→ Apply → Save
```

## How to Build Your Own

```
Sentinel → Workbooks → + Add workbook
→ Edit mode → Add → Add query

# Example: Sign-in failures by user
SigninLogs
| where ResultType != "0"
| summarize FailedCount = count() by UserPrincipalName
| sort by FailedCount desc
| render barchart
```
