# ⚔️ Guide 04 — Attack Simulation & Detection Validation

How to simulate attack scenarios in the lab and validate that detection rules fire correctly.

---

## Overview

Each simulation below maps to a specific detection rule. Run these in your Azure free trial environment to generate real telemetry and verify your Sentinel rules produce incidents.

> ⚠️ **Only run these simulations in your own lab environment. Never attempt these against production systems or systems you do not own.**

---

## Simulation 1 — Brute Force Attack (T1110)

**Goal:** Trigger the `brute-force-detection.kql` rule

**Method:** Use Azure AD's built-in "Bad Password" simulation

```bash
# Using Azure CLI — attempt login with wrong password repeatedly
# Install: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli

for i in {1..15}; do
  az login --username testuser@yourtenant.onmicrosoft.com \
           --password "WrongPassword$i" 2>/dev/null
  echo "Attempt $i failed (expected)"
  sleep 5
done

# Then login successfully with correct credentials
az login --username testuser@yourtenant.onmicrosoft.com \
         --password "CorrectPassword"
```

**Expected Result:**
- 15 `ResultType: 50126` entries in SigninLogs
- 1 successful `ResultType: 0` entry
- Sentinel incident created within 5–10 minutes

---

## Simulation 2 — Impossible Travel (T1078)

**Goal:** Trigger the `impossible-travel.kql` rule

**Method:** Login from two different IP addresses resolving to distant locations

```
Step 1: Login normally from your current location
        → Note your login time and IP

Step 2: Use a VPN server in a distant country (e.g., US or Germany)
        → Login again within 30–60 minutes

Step 3: Check SigninLogs for both entries — the KQL rule will detect
        the geographic impossibility
```

**Tools:** Any free VPN with server selection (ProtonVPN free tier)

**Expected Result:**
- Two SigninLogs entries >500 km apart within 60 minutes
- Sentinel incident with both IPs and locations shown

---

## Simulation 3 — Privileged Role Assignment (T1078.004)

**Goal:** Trigger the `privilege-escalation.kql` rule

**Method:** Assign a privileged role to a test user account

```
Azure Portal
→ Azure Active Directory → Roles and Administrators
→ Search: "Security Administrator"
→ Add assignments
→ Select: testuser@yourtenant.onmicrosoft.com
→ Assign
```

**Expected Result:**
- AuditLogs entry with `OperationName: "Add member to role"`
- Sentinel incident: "Privilege Escalation — New Privileged Role Assignment"
- RoleName = "Security Administrator" in alert details

**Cleanup:** Remove the role assignment after testing

---

## Simulation 4 — Suspicious PowerShell (T1059.001)

**Goal:** Trigger the `suspicious-powershell.kql` rule

**Method:** Run a benign but flagged PowerShell command on a connected VM

```powershell
# Safe simulation — this does NOT download anything malicious
# It mimics the command PATTERN that attackers use

# Simulates encoded command pattern (base64 of "Write-Host Hello")
$encoded = [Convert]::ToBase64String([Text.Encoding]::Unicode.GetBytes("Write-Host Hello"))
powershell.exe -EncodedCommand $encoded -NoProfile

# Simulates WebClient pattern (safe URL)
powershell.exe -Command "(New-Object Net.WebClient).DownloadString('https://example.com')"
```

**Note:** Requires a Windows VM connected to Sentinel with Security Events enabled.

**Expected Result:**
- Event ID 4688 (process creation) logged in SecurityEvent table
- Sentinel incident with CommandLine flagged

---

## Validation Checklist

After running simulations, verify:

```
✅ Logs appearing in correct table (SigninLogs, AuditLogs, SecurityEvent)
✅ KQL rule query returns results when run manually in Sentinel → Logs
✅ Analytics rule fires and creates an Incident
✅ Incident contains correct entity mappings (Account, IP)
✅ Alert severity matches configured level
✅ MITRE ATT&CK tags appear on the incident
```

---

## Checking Results

```kql
// Check all incidents created in last 24h
SecurityIncident
| where TimeGenerated >= ago(24h)
| project TimeGenerated, Title, Severity, Status, Description
| sort by TimeGenerated desc
```
