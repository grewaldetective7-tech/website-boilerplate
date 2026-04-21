# TOPS QRT Automation System — Final Best Recommendation

---

## 1. Project Overview

**TOPS (Total Operations Patrol System)** is a security agency CRM and automation platform designed to manage:

- Night patrolling QRT (Quick Response Team) operations
- Client accounts and service records
- Real-time emergency alerts via WhatsApp
- Payment tracking and follow-ups
- Field agent reporting via mobile forms

---

## 2. ✅ Final Recommended Architecture

```
Field Agent (Mobile)
       │
       ▼
Fillout Form  ──────────────────────────────────────────────┐
       │                                                     │
       ▼                                                     ▼
Google Sheet (Central CRM DB)                         Zite / Airtable (Structured CRM)
       │                                                     │
       └────────────────┬────────────────────────────────────┘
                        │
                        ▼
                  Make (Automation Engine)
                        │
              ┌─────────┼──────────────┐
              ▼         ▼              ▼
        WhatsApp    Email Alert    AppSheet
        (Alerts)   (Management)  (Mobile App)
```

**Recommended CRM Tool: Google Sheets + AppSheet**

| Criteria            | Google Sheets + AppSheet | Airtable     | Zoho CRM     |
|---------------------|--------------------------|--------------|--------------|
| Cost                | Free / Low               | Medium       | Medium–High  |
| Mobile Ready        | ✅ (via AppSheet)         | ✅            | ✅            |
| WhatsApp Integration| ✅ (via Make)             | ✅ (via Make) | Limited      |
| Offline Support     | Partial                  | No           | Yes          |
| Custom Forms        | ✅ (Fillout)              | ✅            | ✅            |
| Ease of Setup       | ⭐⭐⭐⭐⭐               | ⭐⭐⭐⭐      | ⭐⭐⭐        |
| **Recommendation**  | ✅ **BEST FIT**           | Alternative  | Enterprise   |

---

## 3. CRM Module Breakdown

### Module 1 — QRT Night Patrol Form (Fillout)
- Agent fills form at start and end of patrol shift
- Fields: Agent Name, Location, Time In, Time Out, Incident Report, GPS Tag
- Auto-submitted to Google Sheet

### Module 2 — CRM Database (Google Sheets)
Maintain the following sheets (tabs):

| Sheet Name       | Purpose                                      |
|------------------|----------------------------------------------|
| `Clients`        | Client name, address, contact, contract type |
| `Patrols`        | Daily patrol logs per site                   |
| `Agents`         | Agent roster, shifts, contact details        |
| `Incidents`      | Incident reports with severity level         |
| `Payments`       | Payment status, due dates, amounts           |
| `Alerts_Log`     | History of all WhatsApp/email alerts sent    |

### Module 3 — Automation Engine (Make)
Triggers and actions:

| Trigger                        | Action                                      |
|--------------------------------|---------------------------------------------|
| New patrol form submitted      | Log to Patrols sheet + WhatsApp confirmation |
| Incident severity = HIGH       | Instant WhatsApp alert to supervisor         |
| Payment overdue > 7 days       | WhatsApp + email reminder to client          |
| Agent did not check in on time | Alert to QRT manager                        |
| New client added               | Welcome message via WhatsApp                |

### Module 4 — WhatsApp Alert System
- Provider: **CallMeBot** (free) or **Twilio WhatsApp** (production)
- Messages sent via Make HTTP module
- Template categories:
  - 🚨 Emergency / Incident
  - 💰 Payment Reminder
  - ✅ Patrol Confirmation
  - 📋 Daily Summary Report

### Module 5 — Mobile App (AppSheet)
- Built directly on top of Google Sheets
- Views: Patrol Log, Client List, Incident Report, Agent Dashboard
- Offline-capable for field use
- Role-based access: Agent / Supervisor / Admin

---

## 4. Setup Checklist

### Phase 1 — Database Setup
- [ ] Create Google Sheet with all 6 tabs (see Module 2 above)
- [ ] Define column headers and data validation rules
- [ ] Add sample client and agent data for testing

### Phase 2 — Form Integration
- [ ] Build Fillout patrol form with all required fields
- [ ] Connect Fillout form to Google Sheet via webhook or direct integration
- [ ] Test form submission → sheet row creation

### Phase 3 — Make Automation
- [ ] Create Make account and connect Google Sheets module
- [ ] Set up `Watch Rows` trigger on Patrols sheet
- [ ] Build Router with conditions (incident, payment, check-in)
- [ ] Add WhatsApp HTTP module to each branch
- [ ] Test all automation scenarios end-to-end

### Phase 4 — WhatsApp API
- [ ] Register phone number with CallMeBot or Twilio
- [ ] Store API key securely (not in codebase)
- [ ] Configure message templates for each alert type
- [ ] Test message delivery for each trigger

### Phase 5 — AppSheet Mobile App
- [ ] Connect AppSheet to Google Sheet
- [ ] Configure views and forms for each user role
- [ ] Enable offline sync
- [ ] Distribute app link to agents and supervisors

### Phase 6 — Go Live
- [ ] Run full end-to-end test with real agents
- [ ] Train agents on form submission
- [ ] Monitor Make automation logs for first 7 days
- [ ] Review and tune alert thresholds

---

## 5. Security & Data Guidelines

- Never store API keys or WhatsApp credentials in this repository
- Use Google Sheet "Protected Ranges" to prevent accidental edits to critical columns
- Restrict AppSheet access by email domain or role
- Backup Google Sheet weekly (Google Drive version history is automatic)
- Audit the `Alerts_Log` sheet monthly to check for missed alerts

---

## 6. Recommended Third-Party Tools Summary

| Tool        | Purpose                   | Cost          | Link                         |
|-------------|---------------------------|---------------|------------------------------|
| Fillout     | Smart patrol forms        | Free tier     | fillout.com                  |
| Google Sheets | Central CRM database    | Free          | sheets.google.com            |
| Make        | Automation engine         | Free (1k ops) | make.com                     |
| AppSheet    | Mobile field app          | Free tier     | appsheet.com                 |
| CallMeBot   | WhatsApp alerts (testing) | Free          | callmebot.com                |
| Twilio      | WhatsApp alerts (prod)    | Pay-as-you-go | twilio.com                   |

---

## 7. Status

| Phase              | Status              |
|--------------------|---------------------|
| Architecture       | ✅ Finalized         |
| Database Design    | ✅ Defined           |
| Form Integration   | 🔄 In Progress       |
| Make Automation    | 🔄 In Progress       |
| WhatsApp API       | 🔄 In Progress       |
| AppSheet App       | ⏳ Pending           |
| Go Live            | ⏳ Pending           |

---

*Last updated: April 2026 — TOPS QRT Automation Final Recommendation*
