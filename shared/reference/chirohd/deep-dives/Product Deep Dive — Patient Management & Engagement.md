---
type: reference
title: "Product Deep Dive — Patient Management & Engagement"
date: 2026-03-30
source: claude
tags:
  - type/reference
  - topic/product
  - topic/patient-management
  - topic/patient-engagement
  - source/claude
---

# Product Deep Dive — Patient Management & Engagement

> Part of the [[ChiroHD Product Briefing]] series.

---

## Overview

Patient management covers the core record-keeping infrastructure — patient profiles, cases, demographics, family linking, visit tracking, and alerts. This module is **mature and functional**. Patient engagement, on the other hand, covers how practices communicate with and serve patients outside the clinic — texting, the patient app, web scheduling, and automated campaigns. This area is **significantly weaker** and relies heavily on paid add-ons and acquired products (SKED, Spark).

**Primary personas:** Front Desk / Admin (management), Patient (engagement)
**Maturity:** Mature (management) to Weak (engagement)

---

## Patient Management Features

### Patient Profile

The central record for all patient data. Acts as the hub for every downstream function.

**Tabs and sections:**

| Tab/Section | What It Contains |
|-------------|-----------------|
| **Info** | Demographics, contact info, referral source, language preference |
| **Family Management** | Link family members; view family group |
| **Cases** | Care episodes with case type, default charges, fee schedules |
| **Scheduling / Bulk Scheduler** | View and create appointments; bulk schedule care plans |
| **Default Services** | Pre-configured charges for automatic posting |
| **Alerts** | Patient alert templates attached to this patient |
| **Tags** | Categorization labels for filtering and reporting |
| **Visit Tracker** | Adjustment count vs. care plan target |
| **Visits** | SOAP note history |
| **Transaction Ledger** | Financial record: charges, payments, write-offs |
| **Insurance History** | Insurance policies and claim history |
| **Bill History** | Generated bills and claim documents |
| **Documents** | Digital intake submissions, uploaded files |
| **Financial Tab** | Recurring payments and financial overview |

### Cases

**What:** A care episode tied to a patient — organizes visits, charges, and settings under a specific care context.

**Case types:**
- Cash
- Personal Injury (PI)
- Pediatric
- Insurance
- (Custom types configurable)

**Key detail:** Case type is selected at scheduling time and determines:
- Default appointment type
- Billing presets
- Default charges and fee schedule
- Insurance policy association

A patient can have **multiple cases** (e.g., an active insurance case and a prior closed cash case).

### Family Management

**What:** Links related patients (family members) together.
**Navigation:** Patient Profile → Family Management Tab
**Use case:** View all family members, shared scheduling, family-level billing visibility.
**Limitation:** Patient app doesn't support family scheduling (user complaint in reviews).

### Visit Tracker

**What:** A counter that tracks adjustment count against a care plan target.
**Navigation:** Patient Profile → Visit Tracker → Add Tracker
**Behavior:** Automatically increments with each visit; alerts staff when target is reached (e.g., 12 visits on a 12-visit plan).
**Status:** Beta feature.
**Related:** Patient Alert Templates (can trigger milestone actions at specific visit counts).

### Patient Alert Templates

**What:** Pre-built, reusable alert schedules tied to care plans.
**Built at:** System Level → System Settings → Patient Alert Templates
**Attached at:** Patient Profile → Alerts Tab → Create from Template

**Example 12-visit template:**
- Visit 5 → alert: schedule a scan
- Visit 11 → alert: second-to-last visit
- Visit 12 → alert: discuss wellness plan, transition care

**Problem solved:** Without automated alerts, staff must manually remember to take specific actions at specific visit milestones — unreliable at scale.

### Tags

**What:** Categorization labels applied to patients for filtering, reporting, and campaign targeting.
**Levels:** System-level tags (global) and location-level tags (local).
**Use case:** Tag patients by referral source, care plan type, marketing segment, etc.

### Digital Intake / Paperwork

**What:** Patient-completed intake forms submitted electronically before or at their visit.

| Method | How | Notes |
|--------|-----|-------|
| **Intake Link** | URL sent to patient pre-visit | Patient fills out on phone/computer |
| **Check-in device** | Patient completes on kiosk at office | During check-in |
| **Custom paperwork** | Custom forms designed by ChiroHD ($40 build + $1/mo) | For specialized intake needs |

**Destination:** Submissions populate into the patient's Documents tab.
**Limitation:** Patient paperwork editing is limited after submission (user complaint). Some practices use IntakeQ as a workaround.

### Patient Snapshot

**What:** Quick-view summary panel showing patient state at check-in.
**Shows:** Alerts, charges, payment info, upcoming appointments, Blue Patient Notes Box.
**Used by:** Front desk to assess patient state before/during check-in.
**The Blue Patient Notes Box:** Free-text field for persistent notes visible at every check-in (e.g., "On 12-visit prepay, visit 8 of 12").

### Merge Patients

**What:** Utility to merge duplicate patient profiles.
**Navigation:** Location Utilities → Merge Patients
**Use case:** When the same patient has been created twice (e.g., online booking created a new profile for an existing patient).

---

## Patient Engagement Features

### Text Messaging (Twilio Integration)

**What:** Two-way texting between the practice and patients.
**Dependency:** Requires Twilio integration (paid add-on: $10/month + $0.0125/message).

| Feature | Description |
|---------|-------------|
| **Scheduled text reminders** | Automated appointment reminders sent before visits |
| **Reminder templates** | Customizable message templates |
| **Manual texting** | Send individual texts to patients |
| **Two-way texting** | Patients can reply; conversations visible in ChiroHD |
| **Mass notification** | Send broadcast texts to patient groups |
| **Opt-in/opt-out** | Patients can text STOP to unsubscribe |

**Critical limitation: All-or-nothing opt-out.** When a patient texts STOP, it removes them from **all** text communications — not just marketing, but also appointment reminders. There's no granular preference management.

**A2P registration:** Required by carriers. Process involves IRS name matching and can take months. Twilio numbers cannot be released/changed without ChiroHD support intervention.

### Patient App (Mobile)

**What:** Native smartphone app for patients.
**Cost:** $50/month add-on per location.

**Known features:**
- Appointment scheduling (limited)
- Check-in
- Insurance card photo upload

**Documentation state:** 1 help center article. The app is either under-invested or under-documented — both are problems from a PM perspective.

**Known gaps:**
- No family scheduling through the app
- Insurance card photos don't auto-populate fields (uploaded as images only)
- No self-service billing or payment portal
- No care plan visibility for patients
- No self-service insurance information

### Web Scheduler

**What:** Patient-facing scheduling widget embeddable on a practice's website.
**Navigation:** Location Setup → Integrations → Web Scheduler
**Capability:** New patient self-scheduling only.

**Critical limitation:** Existing patients cannot self-schedule online. This is a significant competitive gap — most modern scheduling platforms (Jane App, Cliniko, etc.) allow all patients to self-schedule.

### SKED (Acquired September 2025)

**What:** Mobile-first scheduling and communication automation platform.
**Founded by:** Dr. Erik Kowalke; Inc. 5000 recognized.

**Capabilities:**
- Mobile-first patient scheduling
- Communication automation (campaigns, sequences)
- Digital intake
- No-show reduction tools

**Current integration:** Available as an integration within ChiroHD (Location Setup → Integrations → SKED).
**Status:** Being integrated more deeply into the ChiroHD ecosystem.

### Spark (Acquired September 2025)

**What:** AI-powered patient engagement platform.

**Capabilities:**
- **Automated chatbots** for practice website visitors
- **Reactivation campaigns** for lapsed patients
- **Retention flows** — automated engagement sequences
- **AI-powered** patient communication

**Significance:** Spark brings the closest thing to AI-native capabilities in the ChiroHD ecosystem, though it's focused on engagement/marketing rather than clinical documentation.

### Email Communication

| Feature | Status |
|---------|--------|
| **New patient welcome email** | Automated when first appointment scheduled (if enabled in Office Configuration) |
| **Location-specific email templates** | Configurable per location |
| **Reputation Management** | Beta — review solicitation |
| **Email campaigns** | Limited; email integration cited as a user complaint |

### Communication Channels Summary

| Channel | Status | Notes |
|---------|--------|-------|
| **Text (SMS)** | Functional but paid add-on | Twilio dependency; $10/mo + per-message |
| **In-app notifications** | Basic | Appointment reminders |
| **Email** | Limited | Welcome emails, templates; no robust campaign system |
| **Patient app** | Exists but under-documented | $50/mo add-on |
| **Phone** | No native support | Calling Module exists but is separate from patient engagement |
| **Chatbot (Spark)** | Acquired capability | AI website chatbot; reactivation campaigns |

---

## Calling Module

**What:** ChiroHD's built-in calling feature for practices.
**Academy lesson:** Section 5, Lesson 2: Calling Module
**Note:** A competitive parity item — Platinum System has an advanced in-app calling module. ChiroHD's Calling Module v2 is a documented strategic priority (see Problem Brief: "Calling Module v2 — Platinum Competitive Parity").

---

## Key Strengths

1. **Patient profile** — comprehensive, well-organized hub for all patient data
2. **Cases** — clean separation of care episodes with appropriate billing configuration
3. **Visit Tracker + Alert Templates** — automated care plan milestone management
4. **Bulk Scheduler** — schedule entire care plans from patient profile
5. **Digital intake** — paperwork before visits reduces check-in time
6. **SKED acquisition** — adds mobile-first scheduling and no-show reduction
7. **Spark acquisition** — adds AI chatbot and reactivation capabilities

## Key Gaps

| Gap | Severity | Impact |
|-----|----------|--------|
| **No patient portal** | High | Patients can't view care plans, billing, or manage their own information |
| **Web Scheduler new-patient-only** | High | Existing patients can't self-schedule online |
| **Twilio dependency for texting** | Medium-High | Core communication requires paid add-on; should be native |
| **All-or-nothing text opt-out** | Medium | Patients lose appointment reminders when opting out of marketing |
| **Patient app barely documented** | Medium | 1 help article; unclear feature set; no family scheduling |
| **No self-service billing** | Medium | Patients can't view or pay bills online |
| **Insurance card photos don't auto-populate** | Medium | Uploaded images require manual data entry |
| **Patient paperwork editing limited** | Medium | Can't easily modify submitted intake forms |
| **Email integration weak** | Medium | No robust email campaign system |

---

## Related Academy Lessons

- Section 5, Lesson 2: Calling Module
- Section 9: Texting (6 lessons — reminders, templates, two-way, opt-in/out)
- Section 11, Lessons 3-5: Intake Link, Mass Notification, Patient Paperwork
- Section 12: Patient Profile (15 lessons — comprehensive)
- Section 3, Lessons 21-22, 25: SKED, Web Scheduler, Check-in Device

---

*Part of the [[ChiroHD Product Briefing]] series. See also: [[ChiroHD Feature Map]] for navigation paths.*
