# 🧩 Setup Guide — MSP Operations Commander

This guide explains how to recreate the **MSP Operations Commander** agent in your own Microsoft 365 tenant. No client tenant access is required — everything runs in the MSP's own environment.

> ⚠️ The public demo uses **fictional** client and ticket data only. Do not commit real client data, tenant IDs, or secrets to this repository.

---

## 1. Prerequisites

- A Microsoft 365 tenant with **Microsoft Copilot Studio** access
- Permission to create **SharePoint lists**
- (Optional) A **VirusTotal** API key for threat-intelligence lookups
- (Optional) **Power Automate** for ticket-alert flows

---

## 2. Create the SharePoint Knowledge Sources

The agent is grounded in two structured SharePoint lists.

### 2.1 Client Profiles list

| Column | Type | Purpose |
|---|---|---|
| Client Name | Single line of text | Primary key used by the Client Context Switcher |
| Industry | Single line of text | e.g., Wealth Management, Executive Coaching |
| Primary Contact | Single line of text | Main client contact |
| Environment | Single line of text | e.g., Full Cloud, Hybrid |
| Compliance Notes | Multiple lines of text | Client-specific rules (e.g., "Do NOT delete accounts") |
| Archiving Tool | Single line of text | e.g., Global Relay, Smarsh, Redtail |
| Special Instructions | Multiple lines of text | Anything the engineer must know before acting |

### 2.2 Ticket Log list

| Column | Type | Purpose |
|---|---|---|
| Ticket Number | Single line of text | Unique ticket identifier |
| Client | Single line of text | Links the ticket to a client |
| Subject | Single line of text | Short description |
| Priority | Choice | Critical / High / Normal / Low |
| Status | Choice | Open / Waiting on Client / Resolved |
| Resolution | Multiple lines of text | What was done |

---

## 3. Build the Copilot Studio Agent

1. Create a **new agent** in Microsoft Copilot Studio.
2. **Disable general knowledge** so responses stay grounded in your verified SharePoint data.
3. Add the two SharePoint lists above as **knowledge sources**.

---

## 4. Add the Four Workflows (Topics)

| Workflow | Trigger phrase | Purpose |
|---|---|---|
| Client Context Switcher | "Working on \<client\>" | Loads the active client profile + ticket history |
| Compliance Guard | "Can I \<risky action\>?" | Checks the action against client compliance rules |
| Morning MSP Triage | "Start my day" | Builds a prioritized day plan |
| Evening MSP Review | "End my day" | Summarizes the day and previews tomorrow |

Use a `Global.ClientName` variable so the active client persists across all topics.

---

## 5. Enable Microsoft IQ

Turn on the following so the agent uses real Microsoft 365 work context:

- **Work IQ** — people and work context
- **Mail IQ** — email context
- **SharePoint IQ** — organizational documents and knowledge

---

## 6. Connect Integrations (Optional)

- **VirusTotal API** — add as a custom connector for phishing / link analysis
- **Power Automate** — ticket-alert flows with Adaptive Cards

---

## 7. Publish

Publish the agent to the **Microsoft 365 Copilot** and **Microsoft Teams** channels.

---

*Part of the MSP Operations Commander project — Agents League Hackathon, Enterprise Agents Battle, June 2026.*
