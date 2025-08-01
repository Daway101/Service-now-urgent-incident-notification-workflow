# 🚨 Incident Notification Workflow – ServiceNow Project

## 🧩 System Overview

This ServiceNow project implements an automated incident notification system designed to prevent SLA breaches by ensuring critical priority incidents are routed to the right team, fast.

When a Priority 1 (Critical) incident with Category: Network is created, an email notification is automatically triggered and sent to the Networking Operations team. This ensures that incidents are not missed, even during off-hours or when dashboards aren't actively monitored.

Role-based security and access controls are in place to restrict who can view, respond to, and receive notifications for these incidents—ensuring only authorized personnel handle sensitive network issues.

---

## ⚙️ Implementation Steps

### 1. Create Flow Using Flow Designer

- Navigate to: **Flow Designer > New**
- **Flow Name**: `Critical Network Incident Notification Flow`
- **Trigger**: `Incident Created` on **Incident Table**
- **Conditions**:
  - `Impact is 1 - High`
  - `Urgency is 1 - High`
  - `Category is Network`
- **Action**:
  - **Send Notification** to `Networking Operations` group
  - Include dynamic data: Incident number, short description, Priority, Caller Name

> 🔐 *Security Note*: Only users with `flow_designer` or `admin` roles can access or modify flows. Flow data should not expose sensitive fields unless required by business rules.

---

### 2. Configure Notification Settings

- Navigate to: **System Notification > Email > Notifications**
- Create a new notification:
  - **Table**: `Incident`
  - **When to Send**: `Triggered`
  - **Who will receive**: `Networking Operations Group`
  - **What it will contain**: `${number}`, `${short_description}`, `${priority}`

---

### 3. Verify Email Delivery via Outbox

- Go to: **System Mailboxes > Outbound > Outbox**
- Confirm:
  - Emails are generated only for matching conditions
  - Correct group members received notifications

---

### 4. Test the Notification

- Create a test incident with:

    - Urgency: 1 - High
    - Impact: 1 - High
    - Category: Network

Confirm that the notification is received by impersonating a user in the Network Operations group

---

### 5. Apply Security and Access Controls

- **ACLs (Access Control Rules)**:
  - Only users with itil or network_ops can access incident records and receive notifications
- **Group Security**:
  - Ensure the `Network Operations` group only includes authorized personnel
- **Notification Access**:
  - Validate recipient list to avoid exposure of incident data to unintended users

---

In a global enterprise, critical incidents can occur outside business hours when senior engineers are unavailable. The current static escalation model slows response times by ignoring factors like expertise, time zone, and workload. 

AI-Powered Routing Agent could enhance this incident notification system by evaluating the nature of the incident (e.g., network outage, database failure, latency spike) and cross-references it with an internal knowledge graph of engineers' past tickets, certifications, and specialty areas. Incidents are routed to the engineer or team most qualified based on technical expertise and not just group membership.

The agent takes time zones and business hours, current workload, on-call rosters and recent escalations into account. This ensures the selected engineer is awake, responsive, and not overloaded when assigned a critical task.

It continuously learns from past incidents by analyzing who resolved similar incidents fastest and most effectively, which paths led to SLA compliance, what actions prevented escalation. This smart routing boosts operational efficiency, ensures 24/7 coverage, and delivers faster incident recovery across global teams.

✅ This solution ensures real-time visibility into critical issues while maintaining security and operational efficiency.
