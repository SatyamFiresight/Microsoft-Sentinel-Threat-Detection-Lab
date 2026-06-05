# 📘 Guide 01 — Azure Sentinel Setup

Step-by-step guide to deploy Microsoft Sentinel on a free Azure account for this lab.

---

## Prerequisites

- Free Azure account → https://azure.microsoft.com/free ($200 credit, 30 days)
- Valid email address (use a new Microsoft account for clean lab setup)
- No credit card charges if you stay within free tier limits

---

## Step 1 — Create a Resource Group

A resource group is a logical container for all lab resources.

```
Azure Portal (portal.azure.com)
→ Search: "Resource Groups"
→ + Create
  → Subscription: Azure subscription 1 (free)
  → Resource Group Name: sentinel-lab-rg
  → Region: East US
→ Review + Create → Create
```

---

## Step 2 — Create a Log Analytics Workspace

Sentinel stores all data in a Log Analytics workspace.

```
Azure Portal
→ Search: "Log Analytics Workspaces"
→ + Create
  → Subscription: Azure subscription 1
  → Resource Group: sentinel-lab-rg
  → Name: sentinel-lab-workspace
  → Region: East US
→ Review + Create → Create
```

> ⏳ Deployment takes ~2 minutes.

---

## Step 3 — Enable Microsoft Sentinel

```
Azure Portal
→ Search: "Microsoft Sentinel"
→ + Create
→ Select workspace: sentinel-lab-workspace
→ Add Microsoft Sentinel
```

> ✅ Sentinel is now enabled. You'll land on the Sentinel overview dashboard.

---

## Step 4 — Configure Data Connectors

Navigate to: **Sentinel → Data Connectors**

Connect the following (all free with Azure AD):

### 4a — Azure Active Directory
```
Data Connectors → Search: "Azure Active Directory"
→ Open connector page
→ Check: ✅ Sign-in Logs
→ Check: ✅ Audit Logs
→ Apply Changes
```

### 4b — Azure Activity
```
Data Connectors → Search: "Azure Activity"
→ Open connector page
→ Connect
→ Select subscription → Review + Create
```

### 4c — Microsoft Defender for Cloud (Optional)
```
Data Connectors → Search: "Microsoft Defender for Cloud"
→ Open connector page
→ Connect (requires Defender to be enabled on subscription)
```

> ⏳ Logs begin flowing within 15–30 minutes after connector setup.

---

## Step 5 — Verify Data is Flowing

```
Sentinel → Logs
→ Run this query to check Sign-in Logs:

SigninLogs
| take 10

→ Run this to check Audit Logs:

AuditLogs
| take 10
```

If results appear, your setup is working correctly.

---

## Step 6 — Deploy Detection Rules

```
Sentinel → Analytics → + Create → Scheduled Query Rule
```

For each rule in `/detection-rules/`:
1. Name the rule (e.g., "Brute Force Login Detection")
2. Paste the KQL into the Rule Query box
3. Set Frequency: Every 5 minutes
4. Set Lookup period: Last 30 minutes
5. Map entities (Account, IP) as specified in rule comments
6. Set severity (High / Critical)
7. Add MITRE ATT&CK technique tags
8. Enable and Save

---

## Cost Estimate

| Resource | Free Tier Limit | Lab Usage |
|----------|----------------|-----------|
| Log Analytics ingestion | 5 GB/day free | ~10–50 MB/day |
| Sentinel | Free for first 31 days | Covered |
| Azure AD logs | Free | Free |

> ✅ This lab runs entirely within the Azure free tier. No charges expected.

---

## Cleanup (After Lab)

To avoid any charges after your free credit expires:

```
Azure Portal → Resource Groups → sentinel-lab-rg → Delete resource group
```

This deletes all lab resources in one step.
