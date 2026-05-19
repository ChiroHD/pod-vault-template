---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/admin/utilities
---

# System Templates & Automation

> ChiroHD provides several template systems and an automation engine (Triggers) for standardizing workflows, automating patient communications, and managing care plans across locations.

---

## Email Templates

### Access
System Settings → Email Templates

### Template Types

| Type | Purpose |
|------|---------|
| **New Patient Email** | Welcome email with intake paperwork link, sent on patient creation |
| **General Email** | Multi-purpose emails (educational content, care instructions, etc.) |
| **Marketing Email** | Promotional/outreach emails |
| **Event Email** | Event-specific communications (workshops, classes) |

### Template Macros

Templates support dynamic variables that auto-populate with patient and location data:

| Macro Code | Description |
|-----------|-------------|
| `{{PATIENT_FIRST_NAME}}` | Patient's first name |
| `{{PATIENT_LAST_NAME}}` | Patient's last name |
| `{{APPOINTMENT_DATE_AND_TIME}}` | Formatted appointment time (e.g., "Tuesday, April 7th at 10:00am") |
| `{{LOCATION_DEFAULT_PROVIDER}}` | Default provider in "Title First Last" format |
| `{{DIGITAL_PAPERWORK_LINK}}` | Link to digital intake forms |
| `{{LOCATION_NAME}}` | Office name |
| `{{LOCATION_ADDRESS}}` | Office address |
| `{{LOCATION_ADDRESS_2}}` | Office address line 2 |
| `{{LOCATION_CITY}}` | Office city |
| `{{LOCATION_STATE}}` | Office state |
| `{{LOCATION_ZIP}}` | Office zip code |
| `{{LOCATION_PHONE}}` | Office phone number |
| `{{LOCATION_WEBSITE}}` | Website URL |
| `{{LOCATION_FACEBOOK}}` | Facebook URL |
| `{{LOCATION_TWITTER}}` | Twitter URL |
| `{{LOCATION_YOUTUBE}}` | YouTube URL |
| `{{LOCATION_INSTAGRAM}}` | Instagram URL |

---

## Care Plan Templates

### Access
System Settings → Care Plan Templates

Pre-configured care plan structures that can be applied to patients. Each template defines:
- Plan name
- Number of visits
- Visit frequency
- Duration
- Associated services

### Default Templates

| Template | Description |
|----------|-------------|
| 12 Week Care Plan - Wellness | Short-term wellness maintenance |
| 24 Visit Care Plan | Medium-term corrective care |
| 36 Visit Care Plan - Restoration | Long-term restoration program |
| 72 Visit Care Plan - Long Term | Extended care plan |

Care plans integrate with:
- **Visit Tracking** — counts visits against the plan target
- **Bulk Scheduler** — auto-schedules the plan's visits
- **Patient Alerts** — triggers alerts at visit milestones

---

## Task List Templates

### Access
System Settings → Task List Templates

Workflow checklists that can be applied to patients or used for office processes. Each template contains a series of actionable items.

### Default Templates

| Template | Purpose |
|----------|---------|
| Day of NP Appointment | Tasks for staff on the day a new patient arrives |
| ROF Follow Up | Report of findings follow-up checklist |
| Pay Per Visit | Tasks for managing pay-per-visit patients |
| Admin Task List | General administrative tasks |
| Call to get back on schedule | Reactivation outreach tasks |
| Welcome to ChiroHD - OFFICE Onboarding | Staff onboarding checklist |
| PATIENT Onboarding Task List | New patient onboarding steps |
| Every Visit | Recurring per-visit tasks |
| Reactivate | Patient reactivation workflow |
| Patient Reactivation Onboarding Tasks | Extended reactivation process |
| Case Closed Tasks | Tasks when closing a patient's case |

Task lists can be:
- Assigned to individual patients
- Triggered automatically by Triggers (BETA)
- Tracked for completion
- Customized per location

---

## Patient Alert Templates

### Access
System Settings → Patient Alert Templates

Configurable alerts that display on patient records at specific milestones or conditions.

### Alert Trigger Types

| Trigger | Description |
|---------|-------------|
| Visit count | Alert after N visits (e.g., "12 visits reached — time for re-exam") |
| Date-based | Alert on a specific date or after N days |
| Case status | Alert when a case status changes |
| Insurance | Alert for insurance-related events (renewals, auth expiry) |
| Custom | Ad-hoc alerts applied manually |

### Default Alert Templates

| Template | Purpose |
|----------|---------|
| Corrective Care | Milestone alerts for corrective care plans |
| Pregnancy Adjustment | Safety protocols for pregnant patients |
| ABN Renewal | Advanced Beneficiary Notice renewal reminders |
| Massage Alerts | Massage-specific visit milestones |
| Pay Per Visit | Alerts for pay-per-visit billing management |
| 12/18/36 Visit CP | Care package visit countdown alerts |
| NP Patient Alerts | New patient onboarding milestones |
| Insurance | Insurance-related reminders |
| Evaluation | Re-evaluation timing alerts |

Alerts display as:
- Banner on the patient profile
- Notification in the sidebar alert panel
- Pop-up when opening the patient record (configurable)

---

## Triggers (BETA)

### Access
System Settings → Triggers (BETA)

An automation engine that performs actions when specific conditions are met. Triggers are event-driven and can automate communications, task assignment, and status changes.

### Trigger Types

| Type | Fires When | Example |
|------|-----------|---------|
| **Days Since Last Visit** | Patient hasn't visited in N days | "Send reactivation text after 14 days" |
| **Patient Tag** | A tag is added or removed from a patient | "Assign task list when 'VIP' tag is applied" |
| **Patient Case Status** | A case is opened, closed, or changes status | "Send care plan completion email when case closes" |
| **Patient Status** | Patient becomes active, inactive, or terminated | "Send reactivation outreach when patient becomes inactive" |
| **Invoice Item** | A specific service is charged | "Apply alert when decompression is charged" |
| **Visit Count** | Patient reaches N visits | "Send re-exam reminder after 12 visits" |
| **Location Restriction** | Trigger only fires at specific locations | "Only apply at Location A, not Location B" |
| **Appointment Status** | Appointment is scheduled, arrived, missed, cancelled, rescheduled | "Send text when appointment is missed" |

### Trigger Configuration

Each trigger has:
- **Name** — descriptive label
- **Active Date** — when the trigger activates
- **Conditions** — the event(s) that fire the trigger
- **Actions** — what happens when conditions are met (send text, send email, assign task list, apply tag, create alert)

---

## Transaction Presets

### Access
System Settings → Transaction Presets

Pre-configured service bundles for quick billing entry. Instead of adding services one at a time, front desk staff can apply a preset that adds multiple services with predefined prices and write-offs.

### Example Presets

| Preset | Services Included |
|--------|------------------|
| Adjustment + Hydrotherapy | Wellness Adjustment ($45) + Hydrotherapy ($7, write-off $5) |
| Facebook Special Day One | Cervical X-ray ($24.50, write-off $24.50) + Lumbar X-ray ($24.50, write-off $24.50) + NP Brief ($50, write-off $29) |
| Employee Discount | Wellness Adjustment ($30) + Write Off ($30) |
| Family Monthly Plan (2-4) | Wellness Adjustment ($75) |
| Bag of Bones | Multiple X-rays + Therapeutic Exercise + Nutritional Consult + Re-exam + more |

Each preset line item can include:
- Service name and price
- Write-off amount and reason
- Modifiers
- Units

---

## Forms

### Access
System Settings → Forms

Digital form builder for intake paperwork, consent forms, and custom forms.

### Form Types

| Type | Purpose |
|------|---------|
| **Intake** | Patient intake paperwork (demographics, health history, consent) |
| **Composite** | Multi-section forms combining multiple form types |
| **Custom** | Any custom data collection form |

### Key Features

- **Drag-and-drop builder** — visual form editor ("Open Builder" button)
- **Form macros** — same macro system as email templates for dynamic content
- **Publish/Unpublish** — control form visibility on the patient-facing landing page
- **Landing Page visibility** toggle — whether the form appears on the patient's digital paperwork page
- **Per-location assignment** — forms can be network-wide or location-specific

### Default Forms

| Form | Macro Code | Type |
|------|-----------|------|
| LEGACY INTAKE FORM | LEGACY_INTAKE | Intake |
| Patient Intake Form | CHD_PATIENT_INTAKE | Intake |
| Pediatric Intake Form | CUSTOM_CHIRO_PEDIATRIC_INTAKE | Intake |
| New Patient Intake | CLS_PATIENT_INTAKE | Intake |

---

## Referral Sources

### Access
System Settings → Referral Sources

Categories for tracking how patients find the practice.

### Fields per source

| Field | Description |
|-------|-------------|
| Name | Source name (e.g., "Facebook", "Patient Referral", "Walkin") |
| Category | Internal, External, or Digital |
| Type | Patient Referral or Walk In |
| Status | Active / Inactive |

### Default Sources

| Source | Category | Type |
|--------|----------|------|
| Patient Referral | Internal | Patient Referral |
| Facebook | Internal | Walk In |
| Instagram | Digital | Walk In |
| Walkin | — | Walk In |
| Gym | External | Patient Referral |

Referral sources feed into:
- Patient creation (required field when "Referral Type Required" is ON)
- Lead tracking pipeline
- Referral Tree reporting dashboard
- Source-of-patient analytics

---

## Related

- [[System Settings & Configuration]] — Moderation settings, system configuration
- [[Calendar & Scheduling]] — How care plans and task lists appear in the scheduling workflow
- [[Patient Records]] — How alerts and task lists appear on patient profiles
- [[Office Configuration]] — Toggles that enable/disable automation features
