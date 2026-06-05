# 🔍 Detection Rules

This folder contains all custom KQL analytics rules deployed in Microsoft Sentinel. Each rule is mapped to a MITRE ATT&CK technique and includes configuration metadata for direct deployment.

## Rules Index

| File | Description | MITRE ID | Severity |
|------|-------------|----------|----------|
| `brute-force-detection.kql` | Detects repeated failed sign-in attempts | T1110 | High |
| `impossible-travel.kql` | Detects logins from geographically impossible locations | T1078 | High |
| `privilege-escalation.kql` | Detects new Global Admin / privileged role assignments | T1078.004 | Critical |
| `suspicious-powershell.kql` | Detects encoded/obfuscated PowerShell command execution | T1059.001 | Medium |
| `mass-file-download.kql` | Detects unusually large data downloads suggesting exfiltration | T1030 | High |

## How to Deploy

1. Go to **Microsoft Sentinel → Analytics → + Create → Scheduled Query Rule**
2. Paste the KQL from the relevant `.kql` file into the **Rule query** section
3. Set the **Frequency** and **Lookup period** as noted in each file header
4. Map the **Entity mappings** (Account, IP, Host) as specified
5. Set **Severity** and **MITRE ATT&CK** tags
6. Enable and save the rule
