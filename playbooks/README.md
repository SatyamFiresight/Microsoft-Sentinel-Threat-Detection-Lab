# 🤖 Playbooks (Automated Response)

Sentinel Playbooks are Azure Logic Apps that automate incident response actions.

## auto-block-ip-playbook

**Trigger:** When a High/Critical Sentinel incident is created  
**Action:** Automatically extracts the malicious IP from the incident and adds it to a named location blocklist in Azure AD Conditional Access

### Use Cases
- Auto-block IPs from brute force incidents
- Send Teams/Email notification on privilege escalation alerts
- Create ServiceNow/Jira ticket for every Critical incident

### How to Deploy

```
Sentinel → Automation → + Create → Playbook with incident trigger
→ Name: Auto-Block-Malicious-IP
→ Subscription: Azure subscription 1
→ Resource Group: sentinel-lab-rg
→ Create and continue to designer
```

**Logic App Designer Steps:**
1. Trigger: `Microsoft Sentinel Incident`
2. Action: `Parse JSON` — extract IP entities from incident
3. Action: `Azure AD — Update Named Location` — add IP to blocklist
4. Action: `Add comment to incident` — log the action taken

### Attach Playbook to Analytics Rule

```
Sentinel → Analytics → [Edit rule] → Automated response
→ + Add → Select: Auto-Block-Malicious-IP
→ Save
```

Now every incident from that rule will automatically trigger the playbook.
