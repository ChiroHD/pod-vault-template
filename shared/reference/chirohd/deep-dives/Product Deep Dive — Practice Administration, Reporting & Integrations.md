---
type: reference
title: "Product Deep Dive — Practice Administration, Reporting & Integrations"
date: 2026-03-30
source: claude
tags:
  - type/reference
  - topic/product
  - topic/admin
  - topic/reporting
  - topic/integrations
  - source/claude
---

# Product Deep Dive — Practice Administration, Reporting & Integrations

> Part of the [[ChiroHD Product Briefing]] series.

---

## Overview

This deep dive covers three related areas:
1. **Practice administration** — system and location setup, user/provider management, and the foundational configuration that everything else depends on
2. **Reporting and analytics** — practice metrics, financial reports, and operational visibility
3. **Integrations and ecosystem** — how ChiroHD connects to external systems

Administration is **complex but functional** (71 setup lessons). Reporting is **immature** (dual migration, no benchmarking). Integrations are **limited** (no public API, few third-party connections).

**Primary personas:** Practice Owner, IT/Admin, Office Manager
**Maturity:** Complex (admin), Immature (reporting), Limited (integrations)

---

## Practice Administration

### Two-Tier Architecture

ChiroHD operates on two levels that control access, settings, and capabilities:

#### System Level

**What:** Top-tier franchise/network admin view. Manages all locations from a single dashboard.

**Contains:**
- Services (CPT codes)
- Inventory
- Appointment Types
- SOAP Macros (global)
- Users (system-level)
- Fee Schedules
- Patient Alert Templates
- Task Lists
- Triggers
- Reporting Configuration
- Third-Party Payers
- DX Codes
- Email Templates
- Tags (global)
- Reputation Management
- System Configuration

**Key constraint:** System-level users **cannot have calendars enabled**. Calendar setup must happen at the location level.

#### Location Level

**What:** Individual clinic view. The daily operational interface — calendar is the default landing screen.

**Contains:**
- Calendar (scheduling)
- Providers (local)
- Office Info
- Office Configuration
- Adjusting Areas
- Integrations
- Text Reminders
- Tags (local, can override system)
- Email Templates (local)
- SOAP Macros (local override)
- Patient Alert Templates (local)
- Check-in Device
- Web Scheduler

**Relationship:** System-level settings cascade down to locations. Location-level settings can override system defaults for macros, tags, email templates, and alert templates.

### User Management

ChiroHD has a specific user hierarchy:

| User Type | Where Created | Calendar? | Login? | Use Case |
|-----------|--------------|-----------|--------|----------|
| **System-level user** | System Level → Users | No (cannot) | Yes | Franchise admin, system config |
| **Location-level user (with calendar)** | Location → Settings → Users | Yes | Yes | Providers who schedule patients |
| **Location-level user (no calendar)** | Location → Settings → Users | No | Yes | Front desk, billing staff |
| **Placeholder/fake user** | Location → Settings → Users | Yes | No (no credentials) | Workaround: enables calendar for a provider who only has system-level login |

> [!warning] Antipattern
> The "placeholder/fake user" is a **documented workaround**, not a designed feature. It exists because system-level users can't have calendars, but multi-location providers need system-level access AND a calendar at each location. This is a known architectural limitation being addressed in the Provider/Calendar Data Model Redesign.

### Provider Setup

Linking users to billing credentials for insurance claims:

| Field | Description | Required? |
|-------|-------------|-----------|
| **User** | The calendar user this provider record links to | Yes |
| **NPI (MPI)** | 10-digit National Provider Identifier | Yes (for insurance) |
| **EIN** | 9-digit Employer Identification Number | Yes (most common tax ID) |
| **SSN** | Social Security Number (alternative to EIN) | Alternative |
| **Taxonomy Code** | Provider specialty code | Optional |
| **Provider Type** | Chiropractor, Stretch Therapist, etc. | Yes |

**Navigation:** Settings → Providers → Create New Provider

**Critical:** If a calendar user isn't linked to a provider record, insurance claims show "Unknown Provider" error on green sheets. This is one of the most common setup errors.

**Provider Type matching:** Appointments can only be scheduled with a provider whose type matches the appointment type's provider type requirement. Mismatch = can't schedule.

### Office Info (Location Configuration)

The foundational settings page for each location:

| Setting | Purpose | Note |
|---------|---------|------|
| **Location Name** | Display name throughout system | |
| **Legal Name** | Official business name; toggleable for reports | |
| **Short Name** | Abbreviated name | |
| **Address** | Physical location | |
| **Billing Address** | Toggle to auto-populate from physical address | |
| **Location Email** | Tied to welcome emails | |
| **Opening Date** | Record-keeping | |
| **Default Billing Provider** | Required for billing | Must be set |
| **Default Calendar Provider** | Required for scheduling | Must be set |
| **Default Owning User** | Required for scheduling/billing | Must be set |
| **NPI Number Type 2** | Location-level NPI for claims | Required for insurance |
| **Office Ally X12 Enabled** | Ensures X12 format for clearinghouse | Toggle |
| **Logos** | Light background + dark background variants | |
| **Test Location Flag** | Marks as sandbox | |
| **Time Zone** | Must be changed by ChiroHD support only | Incorrect settings corrupt appointment times |

### Office Configuration

Practice-level preferences and operational settings:

| Setting | What It Controls |
|---------|-----------------|
| **Office Options** | General practice preferences |
| **Scheduled Text Reminders** | Automated reminder timing and behavior |
| **Reminder Options** | Reminder delivery preferences |
| **Reminder Templates** | Message templates for reminders |
| **Email Notifications** | Automated email settings |
| **New Macro Layout** | SOAP macro display configuration |
| **New Patient Email Template** | Auto-sent when first appointment scheduled |

### Onboarding & Setup Complexity

The initial setup process is one of ChiroHD's most significant friction points:

**ChiroHD Academy Setup Course:** 71 lessons across 7 chapters
**Estimated time:** 6+ hours of video training before a practice is operational

**Recommended setup sequence:**
1. System Level Setup (system settings, insurance, payers)
2. Location Level Setup (integrations, texting, providers, utilities)
3. Location Setup: Practice Info
4. Location Setup: Providers
5. Calendar setup (hours, users, order)
6. CPT codes
7. Fee schedules / care packages
8. Inventory setup
9. Automations (task lists, triggers, patient alert templates)
10. Team logins and user access
11. Calendar preferences

**The problem:** Dozens of interdependent settings must be configured in exact sequence. Errors cascade downstream. The Academy explicitly tells users to "pause and complete each step before moving on."

**Competitor benchmark:** Jane App and ChiroFusion advertise same-day setup. ChiroHD's 6+ hours is a significant competitive disadvantage and a board-level priority to address.

---

## Automations & Workflow Tools

### Triggers

**What:** Automated rules that fire when specific criteria are met.
**Level:** System or location
**Navigation:** System Level Dashboard → System Settings → Triggers

**Example:** When Appointment Type = "Day Two" AND Status = "Arrived" → Auto-attach Day Two Task List

**How they reduce manual work:** Without triggers, front desk would need to remember to attach the correct task list for every patient at every milestone. At high volume, this fails.

### Task Lists

**What:** Reusable checklists of action items attached to patients.
**Level:** System level (built), patient level (attached)
**Navigation:** System Level Dashboard → System Settings → Task Lists

**Example "Day Two Task List":**
- [ ] Take payment
- [ ] Add visit tracker
- [ ] Attach patient alert template
- [ ] Bulk schedule future appointments
- [ ] Review default charges

**Visibility:** Tasks appear on the patient profile and on the notification bar on the home screen.

### Autocomplete Appointments

**What:** Automatically marks appointments complete when all three conditions are met (appointment + charges + SOAP note).
**Navigation:** Settings → Office → Office Configuration
**Effect:** Eliminates the manual "Complete Appointment" step, saving a click on every visit.

### Automation Chain (Full Example)

Here's how all automation tools work together for an established patient:

1. Patient checks in at kiosk → **Arrive** status triggers
2. **Trigger** detects: Appointment Type = "Adjustment" AND Status = "Arrived" → attaches relevant task list
3. Provider sees patient in **Arrival Queue** → documents with macros → submits SOAP note
4. SOAP submission → **Default charges auto-post** to ledger
5. **Autocomplete** detects: appointment + charges + SOAP all present → marks complete
6. **Visit Tracker** increments → if target reached, **Patient Alert** fires

All of this can happen with zero manual intervention beyond the provider clicking macros.

---

## Reporting & Analytics

### Current State

ChiroHD's reporting is in a **dual migration** state — legacy and beta report systems coexist, creating confusion about which is authoritative.

| Report System | Status | Notes |
|---------------|--------|-------|
| **Legacy reports** | Active | Older format; some users prefer familiar interface |
| **Beta reports** | Active | Newer format; not all legacy reports have been migrated |
| **Health Check** | Active | Useful but limited practice health overview |
| **System Level Reporting** | Active | Cross-location reporting for multi-location practices |

### Available Report Types

Based on Academy training and help center documentation:

| Report Category | Examples |
|----------------|---------|
| **Financial** | Collections, charges, write-offs, aging |
| **Patient** | New patients, demographics, referral sources |
| **Scheduling** | No-shows, appointment volume, provider utilization |
| **Clinical** | Visit counts, visit tracker status |
| **Insurance** | Claims submitted, ERA/EOB posting, billing history |
| **Operational** | Recurring payments, monthly payments |

### Health Check

**What:** A practice health overview dashboard.
**Strengths:** Provides a quick snapshot of practice performance.
**Limitations:** Limited metrics, no benchmarking against peers, no trend analysis.

### Key Reporting Gaps

| Gap | Impact |
|-----|--------|
| **Dual report systems** | Users confused about which reports are current/authoritative |
| **No practice benchmarking** | Can't compare performance against similar practices |
| **No predictive analytics** | No revenue forecasting, patient churn prediction, or capacity planning |
| **No custom reports** | Limited ability to build ad-hoc reports |
| **No unified dashboard** | Practice health requires checking multiple report areas |
| **"Exclude from Report" is permanent** | No undo — accidentally excluded data is gone from reports forever |
| **No data export / BI integration** | Can't feed data into external analytics tools |
| **No product metrics infrastructure** | ChiroHD itself has no analytics on how the product is used (every field in Metrics Dashboard is "TBD") |

---

## Integrations & Ecosystem

### Current Integration Landscape

| Category | Integration | Type | Notes |
|----------|-----------|------|-------|
| **Payments** | Fortis | Embedded | Primary payment processor; Financial Tab read-only |
| **Payments** | Cash Practice Systems | Embedded | Alternative payment processor |
| **Clearinghouse** | Office Ally (Service Center) | SFTP | X12 file upload; separate portal for finalization |
| **Clearinghouse** | TriZetto | SFTP | Alternative clearinghouse |
| **Clearinghouse** | Availity | SFTP | Third clearinghouse option |
| **Texting** | Twilio | API | Paid add-on; $10/mo + per-message |
| **Scheduling** | SKED | Owned (acquired) | Mobile-first scheduling, no-show reduction |
| **Patient engagement** | Spark | Owned (acquired) | AI chatbot, reactivation campaigns |
| **CRM** | Go High Level (GHL) | Proprietary sync | Marketing automation integration |
| **X-ray** | PostureCo | Integration | Posture analysis |
| **X-ray** | MyoVision | Integration | Surface EMG |
| **X-ray** | Insight Subluxation Station | Integration | Neurological scans |
| **General** | Zapier | API | Connects 8,000+ apps; being deprecated |

### Integration Architecture

- All clearinghouse integrations use **SFTP-based X12 file upload** (batch, not real-time)
- Payment integrations are **embedded** but have read-only limitations
- No **public API** exists
- No **integration marketplace**
- No **self-service integration activation** — most require manual setup or support involvement

### Missing Integrations (Competitive Gaps)

| Integration Type | What's Missing | Competitor Example |
|-----------------|---------------|-------------------|
| **Public API** | No developer API for custom integrations | Jane App, DrChrono have APIs |
| **Webhook support** | No event-driven integration capability | Standard in modern SaaS |
| **Lab results** | No lab integration | DrChrono, Kareo have lab integrations |
| **Pharmacy** | Not applicable to chiro (but supplement ordering could be) | N/A |
| **Telehealth** | No native video visit capability | DrChrono, Tebra have telehealth |
| **Patient communication** | Native texting (not Twilio-dependent) | Jane App has built-in messaging |
| **Revenue cycle management** | No full RCM partner integration | Tebra, ChiroTouch have RCM |
| **Analytics/BI** | No data export to external BI tools | Standard in enterprise SaaS |

---

## Training & Onboarding Resources

### ChiroHD Academy

**Platform:** Thinkific (chirohdacademy.thinkific.com)
**Structure:**

| Course | Lessons | Chapters | Focus |
|--------|---------|----------|-------|
| **Main Course** | 162 | 18 | Full platform training, clinical workflows |
| **Setup Course** | 71 | 7 | System and location configuration |

**Prerequisite playlists (Vimeo):**
- System Level Setup — system settings, insurance, third-party payers
- Location Level Setup — integrations, texting, providers, utilities, mobile app

### Help Center

- **210 articles** across 14 categories
- **Quality concern:** 60-70% of articles have empty or partial content (blank numbered lists, incomplete instructions)
- **Insurance articles** = 57 (27% of all content) — signals where users struggle most
- **3+ broken training videos** confirmed

### Release Notifications

**How users learn about new features:**
- Full-screen popup on first login after a release (one-time only; dismissing = permanent)
- Release Notification Dashboard (Version Bar → bottom-left corner → click) lists all past releases
- No email release notes, no in-app changelog beyond this

---

## Key Strengths

1. **Two-tier architecture** — genuine support for franchise/multi-location management
2. **Automation chain** — triggers + task lists + alert templates + autocomplete = powerful zero-touch workflows
3. **ChiroHD Academy** — thorough training content (when video works)
4. **Chiropractic-specific configuration** — appointment types, provider types, care plan structures all domain-native
5. **SKED + Spark acquisitions** — expanding engagement capabilities

## Key Gaps

| Gap | Severity | Impact |
|-----|----------|--------|
| **Onboarding friction (71 lessons, 6+ hours)** | High | Slows acquisition; board-level priority |
| **Placeholder user antipattern** | High | Architectural limitation causing confusion |
| **Reporting dual migration** | High | Users confused; no single source of truth |
| **No public API** | Medium-High | Closed system; can't extend or integrate |
| **No practice benchmarking** | Medium | Owners can't compare against peers |
| **Help center content quality** | Medium | 60-70% incomplete; drives support tickets |
| **Broken training videos** | Medium | Erodes trust in Academy |
| **"Exclude from Report" permanent** | Medium | Creates trust/compliance issues |
| **Zapier being deprecated** | Medium | Removes general integration capability |
| **Integration read-only limitations** | Medium | Fortis, ERA incomplete |

---

## Related Academy Lessons

**Setup Course (71 lessons):**
- System Level Setup playlist
- Location Level Setup playlist
- Location Setup (31 lessons, Section 3)
- System Settings (Section 7)
- Location Settings (Section 8)

**Main Course:**
- Section 1: Welcome, Academy intro, release notifications
- Section 5: Users, Providers, Calendars (6 lessons)
- Section 6: Preferences, Subscription
- Section 7: System Settings, System Level Reporting
- Section 8: Location Settings, Services, DX Codes, Referral Sources
- Section 10: Integrations Overview
- Section 11: Location Utilities (9 lessons)

---

*Part of the [[ChiroHD Product Briefing]] series. See also: [[ChiroHD Feature Map]] for navigation paths, [[ChiroHD Platform Reference]] for technical details.*
