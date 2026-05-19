---
type: reference
date: 2026-04-03
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/hub
  - source/claude
  - topic/product
---

# ChiroHD Product Guide

> A comprehensive reference for every feature, screen, and setting in ChiroHD. Built from direct UI inspection of the test instance (v6.1.0-6.2.0, March-April 2026) via automated browser exploration, cross-referenced against ChiroHD Academy training content (447 notes).
>
> This guide complements the Academy-sourced knowledge base with current, first-hand product documentation based on what the application actually does today.

---

## How a Chiropractic Practice Works

Before diving into the product, it helps to understand what happens in a chiropractic office on a typical day.

**The people:** A chiropractic practice typically has a Doctor of Chiropractic (DC) who treats patients, a Chiropractic Assistant (CA) who handles front desk duties (scheduling, check-in, billing), and may have additional providers like a Licensed Massage Therapist (LMT) or another DC. The CA is the primary user of the scheduling, billing, and patient management features. The DC primarily uses the SOAP note system.

**The patient visit:**

```
Schedule → Remind → Arrive → Check In → Route to Treatment Area → Provider Treats (SOAP Note) → Charges Post → Collect Payment → Schedule Next Visit
```

1. **Scheduling:** Patient books via phone, walk-in, or online scheduler. The CA creates the appointment on the calendar.
2. **Reminders:** Automated text/email reminders go out 24-48 hours before the visit (requires Twilio).
3. **Arrival:** Patient checks in at the front desk or via a self-service check-in device (enters phone number).
4. **Routing:** In busy offices, the Calling Module announces which treatment area (Table 1, Exam Room, etc.) the patient should go to.
5. **Treatment:** The DC examines and adjusts the patient. The provider documents the visit in a SOAP note (Subjective, Objective, Assessment, Plan).
6. **Billing:** Submitting the SOAP note automatically posts charges to the patient's ledger. For insurance patients, a claim (GreenSheet) is later submitted to the insurance company.
7. **Payment:** The CA collects copay or payment at checkout. Insurance patients have claims processed later.
8. **Follow-up:** The CA schedules the next appointment, often using a care plan (e.g., 3x/week for 4 weeks).

**New patient progression:**
- **Day 1:** History intake, examination, possibly X-rays, first adjustment
- **Day 2:** Report of Findings (ROF) — the DC presents the diagnosis and care plan. Patient accepts the plan, and the CA bulk-schedules all visits.
- **Day 3+:** Ongoing adjustments per the care plan, with periodic re-exams (every 12-24 visits)

**Two revenue models:** Most practices operate on a mix of cash-pay patients (flat fee per visit or prepaid care packages) and insurance patients (claims submitted to payers, with copay/coinsurance collected from the patient).

---

## How This Guide Is Organized

ChiroHD operates at two levels:

1. **System Level (Organization)** — Settings that apply across all locations in a network: services, inventory, appointment types, macros, payers, and configuration toggles. Think of this as "corporate headquarters."
2. **Location Level** — Each practice location has its own calendar, providers, patients, billing, and operational settings. Think of this as "an individual office."

---

## Common Workflows

| Workflow                               | Start Here                                       | Then                                                       |
| -------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------- |
| Schedule a new patient                 | [[Calendar & Scheduling]] (Appointment Creation) | [[Patient Records]] (Creating a Patient)                   |
| Process a visit (check-in to checkout) | [[Calendar & Scheduling]] (Status Workflow)      | [[SOAP Notes]] (Provider Mode) → [[Transactions & Ledger]] |
| Set up a new location                  | [[Location Setup]]                               | [[Office Configuration]] → [[Integrations Overview]]       |
| Submit an insurance claim              | [[Insurance Home & Claims]] (Claim Workflow)     | [[Fee Schedules]]                                          |
| Process an insurance payment           | [[Insurance Home & Claims]] (EOB Processing)     | [[Transactions & Ledger]] (Insurance Payment Posting)      |
| Configure care packages                | [[Fee Schedules]] (Cash Fee Schedules)           | [[Transactions & Ledger]] (Care Package Billing)           |

---

## Product Areas

### System Administration
- [[System Settings & Configuration]] — Organization info, system configuration toggles, allowed domains, moderation settings
- [[Services & Inventory]] — CPT-coded services, pricing, categories, inventory items, moderation workflow
- [[Appointment Settings]] — Appointment types, sub-types, schedule templates, setup wizard
- [[Fee Schedules]] — Insurance and cash fee schedules, service-level allowed amounts
- [[System Templates & Automation]] — Email templates, care plan templates, task list templates, patient alert templates, triggers (BETA), forms, transaction presets, referral sources

### Scheduling & Patient Flow
- [[Calendar & Scheduling]] — Calendar view, Flow screen, Calling module, appointment creation, provider schedule management, exception hours, events, patient queue, status workflow, treatment areas

### Patient Management
- [[Patient Records]] — Patient list, patient profile, demographics, cases, insurance coverage, create patient workflow, patient snapshot, family management, alerts, tags, advanced search

### Clinical Documentation
- [[SOAP Notes]] — Provider Mode, Patient Mode, Patient Chart (BETA), macro system (97 building blocks), note submission, charge auto-posting, electronic signature, submit-and-assign workflow

### Billing & Insurance
- [[Insurance Home & Claims]] — Insurance dashboard, accounts receivable, claim creation, claim history, EOB processing (legacy + current), ERA auto-allocation, payer management, reason code behaviors
- [[Transactions & Ledger]] — Transaction entry, payment types, ledger view, write-offs, adjustments, care package billing, pre-set transactions

### Reporting & Analytics
- [[Reporting & Analytics]] — Reporting dashboards (8 tabs), All Reports, Simple Reporting, Advanced Patient Search, Patient List, Lead Tracking, Recurring Payments (BETA)

### Location Settings
- [[Office Configuration]] — Office options, BETA options, ChiroHD options (35 toggles documented)
- [[Location Setup]] — Office info, providers, users, treatment areas, tags, reminder options, email notifications, integrations, historical billing

### Integrations
- [[Integrations Overview]] — Merchant services (Fortis), Twilio, Clearinghouse, SKED, Web Scheduler, Stripe, Zapier/Webhook, X-Rays/Posture, GoHighLevel, CareCredit

### Utilities
- [[Utilities & Tools]] — Documents/X-Rays, Insurance Eligibility (BETA), Intake Link, Mass Notification, Patient Paperwork, Rebill Utility (BETA), Insurance Renewal, Merge Patients, Monthly Payments

---

## Glossary

Essential terms for understanding ChiroHD and chiropractic practice management.

### Clinical Terms

| Term | Definition |
|------|-----------|
| **Adjustment** | The core chiropractic treatment — a manual manipulation of the spine or joints to correct alignment. Billed using CPT codes 98940-98943. |
| **Subluxation** | A misalignment of vertebrae that chiropractors diagnose and treat. Documented in SOAP notes using M99.0x ICD-10 codes. |
| **SOAP Note** | The standard clinical documentation format: **S**ubjective (patient's complaint), **O**bjective (provider's findings), **A**ssessment (diagnosis), **P**lan (treatment plan). |
| **Re-exam** | A periodic progress evaluation (every 12-24 visits) where the DC reassesses the patient and updates the care plan. Required for insurance documentation of continued medical necessity. |
| **ROF (Report of Findings)** | The Day 2 presentation where the DC explains diagnosis and recommended care plan to the patient. |
| **CA (Chiropractic Assistant)** | Front desk / administrative staff. Maps to the "Administrative Only" provider type in ChiroHD. |
| **LMT** | Licensed Massage Therapist. Maps to the "Massage Therapist" provider type. |
| **Modality** | A passive therapeutic treatment done to the patient (e.g., electrical stimulation, ultrasound, hot/cold packs). CPT codes 97010-97028. |
| **Therapeutic Procedure** | An active treatment requiring patient participation (e.g., therapeutic exercise, manual therapy). CPT codes 97110-97542. |
| **Medical Necessity** | The clinical and legal standard that every visit must demonstrate why treatment is needed. Required for insurance reimbursement. |

### Billing & Insurance Terms

| Term | Definition |
|------|-----------|
| **CPT Code** | Current Procedural Terminology — standardized 5-digit codes for billing medical services. Every service in ChiroHD maps to a CPT code (e.g., 98940 = chiropractic adjustment, 1-2 regions). |
| **ICD-10 / DX Code** | International Classification of Diseases — diagnosis codes that justify why a service was performed (e.g., M54.5 = low back pain). Required on insurance claims. |
| **NPI** | National Provider Identifier — a 10-digit number required for any healthcare provider who bills insurance. Type 1 = individual provider, Type 2 = organization/practice. |
| **CMS-1500** | The standard paper claim form used to bill insurance. ChiroHD generates these digitally as "GreenSheets." |
| **GreenSheet** | ChiroHD's term for a CMS-1500 claim form. Named after the green paper historically used for insurance claims. |
| **ERA** | Electronic Remittance Advice — the electronic version of an EOB, delivered via SFTP in X12 835 format. Enables automated insurance payment posting. |
| **EOB** | Explanation of Benefits — the insurance company's response to a claim, showing what was paid, what was adjusted, and what the patient owes. |
| **Clearinghouse** | An intermediary company that routes electronic claims from the practice to insurance payers and sends payment info back. |
| **Payer** | The insurance company responsible for paying claims. |
| **Modifier** | A two-character code appended to a CPT code for additional billing information. Key chiropractic modifiers: AT (active treatment — required by Medicare), GA (ABN on file), 25 (significant separate E/M). |
| **Write-off** | The portion of a charge that is forgiven. Can be contractual (insurance requires it), professional courtesy, or care package discount. |
| **Copay** | A fixed dollar amount the patient pays per visit (e.g., $25/visit). |
| **Coinsurance** | A percentage the patient pays after their deductible is met (e.g., 20%). |
| **Deductible** | The amount a patient must pay out-of-pocket before insurance begins covering services. |
| **AR (Accounts Receivable)** | Money owed to the practice that hasn't been collected yet. Tracked by aging buckets (0-30, 31-60, 61-90, 91+ days). |
| **ABN** | Advance Beneficiary Notice — a Medicare-required form warning patients they may be responsible for charges that Medicare may not cover (e.g., maintenance/wellness care). |
| **DME** | Durable Medical Equipment — physical items like braces, pillows, and traction devices. Sold as inventory in ChiroHD. |
| **Timely Filing** | Insurance payers require claims to be submitted within a deadline (typically 90-365 days). Claims past the deadline are automatically denied. |
| **Accept Assignment** | Whether the provider agrees to accept the insurance-allowed amount as full payment (Box 27 on CMS-1500). |

### Internal Tools

| Term | Definition |
|------|-----------|
| **Lumos** | Internal AI agent created by Matt Gore. Lives in the #lumos Slack channel. Answers questions about how ChiroHD is programmed for people without codebase access (product, support). Planned to become a self-service tool for end users eventually. *(Source: Dr. C product training, 2026-04-10)* |

### ChiroHD-Specific Terms

| Term | Definition |
|------|-----------|
| **Network / Organization** | The top-level entity containing all locations. System settings apply across the network. |
| **Location** | A single practice site with its own calendar, patients, providers, and settings. |
| **Calendar User** | A user assigned to a specific location who may or may not have an enabled calendar. Not all Calendar Users are providers. |
| **Provider Type** | Determines which appointment types a user can accept: Chiropractor, Massage Therapist, Nutritionist, Stretch Therapist, Administrative Only. |
| **Case** | A billing container on a patient record. Each case holds diagnosis codes, default services, and (for insurance) coverage details. Types: Cash, Insurance, Personal Injury, Workers Comp. |
| **Feature Flag** | Many features are behind toggles in Office Configuration. Some are labeled BETA. Locations can enable/disable features independently. |
| **Flow Screen** | The patient queue view showing appointment status progression through the visit. |
| **Calling Module** | Routes patients to treatment areas (tables, exam rooms) using configurable audio announcements. |
| **SOAP Macro** | A clickable building block that auto-populates sections of a SOAP note with structured clinical language. Organized in tabs (SOAP, CCS, Exam). |
| **Patient Snapshot** | A collapsible side panel showing a quick summary of a patient's status, alerts, balance, and upcoming appointments. Accessible from the calendar, search, and other screens. |

---

## Application Details

| Property | Value |
|----------|-------|
| URL | https://www.chirohd.com |
| Observed Versions | 6.1.0 (March 31 audit), 6.2.0 (April 3 observation) |
| Platform | Angular SPA, cloud-hosted |
| Release Cadence | Bi-weekly (Monday deployments, limited release first, then GA per [[2026-03-23 Release Process Proposal — Limited Release Protocol\|Limited Release Protocol]]) |
| Login Types | Regular, Check-In Device, Paperwork |
| Supported Browsers | Chrome (recommended), Safari, Firefox |
| Mobile | Responsive web (no native app — SKED is the patient-facing app) |

---

## Related Resources

- [[🎓 ChiroHD Academy Index]] — Original Academy-sourced knowledge base (447 notes from training videos + help center)
- [[ChiroHD Platform Reference]] — Comprehensive product analysis synthesized from Academy content
- [[ChiroHD Glossary]] — Full terminology reference from the Academy
- [[ChiroHD Feature Map]] — Feature name → navigation path → description lookup table
- [[Williams Chiropractic — Test Location Audit]] — Detailed audit of Jeff's test location
- [[Williams Chiropractic — Test Location Setup Guide]] — Step-by-step setup instructions
