# 🗺️ Guide 05 — MITRE ATT&CK Mapping

Complete mapping of all detection rules in this lab to the MITRE ATT&CK framework (v14).

---

## Coverage Overview

```
Tactics Covered:    4 / 14
Techniques Covered: 5
Sub-techniques:     1
```

---

## Detailed Mapping Table

| Detection Rule | ATT&CK ID | Technique Name | Tactic | Data Source |
|---------------|-----------|----------------|--------|-------------|
| Brute Force Detection | T1110 | Brute Force | Credential Access | Azure AD Sign-in Logs |
| Impossible Travel | T1078 | Valid Accounts | Initial Access | Azure AD Sign-in Logs |
| Privilege Escalation — New Admin | T1078.004 | Cloud Accounts | Privilege Escalation | Azure AD Audit Logs |
| Suspicious PowerShell | T1059.001 | PowerShell | Execution | Security Events (4688, 4104) |
| Mass File Download | T1030 | Data Transfer Size Limits | Exfiltration | Office 365 / SharePoint Logs |

---

## Tactic → Technique Breakdown

### TA0001 — Initial Access
- **T1078** — Valid Accounts
  - Detection: Impossible travel signals use of compromised valid credentials from unexpected geographies

### TA0003 — Privilege Escalation
- **T1078.004** — Cloud Accounts
  - Detection: Privileged role assignment to new or unexpected users, especially self-assignments

### TA0006 — Credential Access
- **T1110** — Brute Force
  - Detection: Repeated failed logins (≥10) followed by a successful login indicating credential stuffing or password spraying

### TA0002 — Execution
- **T1059.001** — Command and Scripting Interpreter: PowerShell
  - Detection: Encoded commands, download cradles, AMSI bypass, Defender tampering flags

### TA0010 — Exfiltration
- **T1030** — Data Transfer Size Limits
  - Detection: Bulk file downloads (>50 files in 30 min) from SharePoint/OneDrive

---

## ATT&CK Navigator Coverage

To visualize this coverage in the ATT&CK Navigator:

1. Go to https://mitre-attack.github.io/attack-navigator/
2. Click **Create New Layer** → **Enterprise**
3. Use the **Search** feature to highlight techniques:
   - T1110, T1078, T1078.004, T1059.001, T1030
4. Color them to show detection coverage

---

## Detection Gaps (Future Work)

The following high-priority techniques are **not yet covered** and represent roadmap items:

| ATT&CK ID | Technique | Priority |
|-----------|-----------|----------|
| T1098 | Account Manipulation | High |
| T1566 | Phishing | High |
| T1021 | Remote Services | Medium |
| T1071 | Application Layer Protocol | Medium |
| T1190 | Exploit Public-Facing Application | High |

---

## References

- [MITRE ATT&CK v14 — Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/)
- [ATT&CK for Cloud](https://attack.mitre.org/matrices/enterprise/cloud/)
- [Microsoft Sentinel MITRE Coverage](https://learn.microsoft.com/en-us/azure/sentinel/mitre-coverage)
