---
type: reference
date: 2026-04-03
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/calendar/scheduling
---

# Appointment Settings

> Appointment types and sub-types define the kinds of visits a practice can schedule. These are managed at the system level and inherited by locations. Each appointment type has properties that control scheduling behavior, duration, provider eligibility, and billing defaults.

---

## Access

System Settings → Appointment Settings

### Tabs

| Tab | Purpose |
|-----|---------|
| **Appointment Types** | Define the kinds of visits (New Patient, Adjustment, Re-exam, Massage, etc.) |
| **Appointment Sub Types** | Refinements within a type (e.g., "30 min" and "60 min" sub-types under Massage) |
| **Schedule Templates** | Pre-built weekly schedule patterns that can be applied to providers |

**Setup Wizard** button is also available for guided appointment type configuration.

---

## Appointment Types

Each appointment type defines:

| Field | Description |
|-------|-------------|
| **Name** | Display name (e.g., "New Patient", "Adjustment", "Re-exam") |
| **Duration** | Default length in minutes |
| **Provider Type** | Which provider types can accept this appointment (Chiropractor, Massage Therapist, etc.) |
| **Color** | Calendar display color for this appointment type |
| **Category** | Grouping for reporting and filtering |
| **Default Case Type** | Auto-selected case type when this appointment is scheduled (Cash, Insurance, etc.) |
| **Status** | Active or Inactive |

### Standard Appointment Types

Based on Academy training and system defaults:

| Type | Typical Duration | Provider Type | Purpose |
|------|-----------------|--------------|---------|
| **New Patient** | 30-60 min | Chiropractor | First visit — exam, history, initial treatment |
| **Adjustment** | 15-20 min | Chiropractor | Standard chiropractic adjustment visit |
| **Re-exam** | 30 min | Chiropractor | Progress evaluation — typically every 12-24 visits |
| **Massage** | 30-60 min | Massage Therapist | Massage therapy session |
| **Decompression** | 15-30 min | Chiropractor | Spinal decompression therapy |
| **Consultation** | 15-30 min | Any | Pre-treatment consultation |
| **X-Ray** | 15 min | Chiropractor | Diagnostic imaging appointment |

### Provider Type Matching

The Provider Type assigned to an appointment type must match the Provider Type assigned to the user. If a user is set to "Chiropractor" provider type, they can only accept appointment types designated for Chiropractors.

**Common issue:** If scheduling produces a spinning wheel or error, check that the appointment type's provider type matches the calendar user's provider type.

---

## Appointment Sub Types

Sub-types provide further specificity within an appointment type:

| Field | Description |
|-------|-------------|
| **Name** | Sub-type label (e.g., "30 minute", "60 minute") |
| **Parent Type** | Which appointment type this belongs to |
| **Duration Override** | Optional — overrides the parent type's default duration |

**Use case:** A "Massage" appointment type might have sub-types for "30 Minute Massage", "60 Minute Massage", and "90 Minute Massage", each with different durations.

Sub-types appear as a secondary dropdown in the appointment creation dialog after the appointment type is selected.

---

## Schedule Templates

Pre-built weekly hour patterns that can be applied to provider schedules:

| Field | Description |
|-------|-------------|
| **Name** | Template label |
| **Hour Grid** | Weekly schedule pattern (Mon-Sun with morning/afternoon blocks) |
| **Column Count** | Number of simultaneous appointment columns |
| **Time Block Size** | Grid resolution (15, 30, or 60 minutes) |

**Use case:** A "Standard DC Schedule" template might define Mon/Wed/Fri 8am-12pm & 2pm-6pm, Tue/Thu 9am-1pm. Instead of configuring each provider manually, apply the template.

Templates are applied via: Calendar → Provider Gear Icon → Provider Schedule → Apply Template.

---

## Appointment Lifecycle

```
Schedule → Remind → Arrive → Assign → Treat (SOAP) → Complete → Follow-up
```

| Stage | What Happens | Triggered By |
|-------|-------------|-------------|
| **Schedule** | Appointment created on calendar | Front desk, Web Scheduler, or SKED |
| **Remind** | Text/email reminder sent | Twilio (automated based on reminder schedule) |
| **Arrive** | Patient checks in | Front desk, check-in device, or text check-in |
| **Assign** | Patient routed to treatment area | Calling module or manual drag on Flow screen |
| **Treat** | Provider opens and submits SOAP note | Provider in SOAP Provider Mode |
| **Complete** | Visit finalized, charges posted | Auto-complete (BETA) or manual completion |
| **Follow-up** | Next appointment scheduled | Front desk or bulk scheduler |

---

## Related

- [[Calendar & Scheduling]] — How appointment types appear on the calendar
- [[SOAP Notes]] — Clinical documentation triggered by appointments
- [[Services & Inventory]] — Services linked to appointment types for billing
- [[System Templates & Automation]] — Care plan templates that reference appointment types
