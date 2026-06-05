# 📋 Sample Logs

Simulated Azure AD / Microsoft 365 log data for testing and validating the KQL detection rules in this lab without requiring a live Azure environment.

## Files

| File | Description | Tests Rule |
|------|-------------|-----------|
| `signin-logs-sample.json` | Simulated SigninLogs with brute force and impossible travel scenarios | `brute-force-detection.kql`, `impossible-travel.kql` |
| `audit-logs-sample.json` | Simulated AuditLogs with privileged role assignment events | `privilege-escalation.kql` |

## Embedded Scenarios

### signin-logs-sample.json
- **Brute Force + Success** — `john.doe@contoso.com` has 3+ failed logins followed by a successful login from a different IP using a suspicious user agent (`python-requests`)
- **Impossible Travel** — `priya.sharma@contoso.com` logs in from Bengaluru, India and then Frankfurt, Germany within 47 minutes (~7,000 km apart)

### audit-logs-sample.json
- **Privilege Escalation** — New user is assigned Global Administrator role
- **Self-Assignment (Critical)** — Same user then self-assigns Privileged Role Administrator from an external IP

## How to Use These Logs for Testing

### Option 1 — Ingest via Log Analytics Custom Logs (Free Tier)
1. Azure Portal → Log Analytics Workspace → Custom Logs
2. Upload the JSON file as a custom table
3. Run the KQL rules against the custom table name

### Option 2 — Use Azure Sentinel Demo Environment
Microsoft provides a free [Sentinel Training Lab](https://github.com/Azure/Azure-Sentinel) with pre-loaded data — use these sample logs as reference to understand expected output.

### Option 3 — Local KQL Testing with Kusto Explorer
1. Download [Kusto Explorer](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer) (free)
2. Load JSON as a local table
3. Run KQL queries locally without any Azure cost
