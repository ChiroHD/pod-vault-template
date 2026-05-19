---
type: reference
title: "Product Deep Dive — Scheduling & Calendar"
date: 2026-03-30
source: claude
tags:
  - type/reference
  - topic/product
  - topic/scheduling
  - source/claude
---

# Product Deep Dive — Scheduling & Calendar

> Part of the [[ChiroHD Product Briefing]] series.

---

## Overview

The scheduling and calendar system is one of ChiroHD's **most mature modules** and a daily workhorse for front desk staff. It handles appointment booking, provider schedule management, patient check-in, and the visual command center for practice operations.

**Primary persona:** Front Desk / Admin
**Secondary personas:** Practice Owner, Provider (views their schedule)
**Maturity:** Mature

---

## What Problem Does This Solve?

Chiropractic practices have unique scheduling demands:
- **High volume** — 40-80+ patients/day per provider
- **High frequency** — patients seen 2-3x/week for weeks or months
- **Multiple providers** — need side-by-side visibility
- **Care plan adherence** — bulk scheduling entire treatment plans upfront
- **Walk-ins** — common in chiropractic; must fit into existing flow

The scheduling module manages all of this from a single visual calendar that doubles as the operational dashboard for the practice.

---

## Features

### Calendar Views

ChiroHD offers two calendar display modes:

| View | Layout | Drag-and-Drop | Best For |
|------|--------|--------------|----------|
| **Provider View** | All providers side by side in columns | Cross-provider rescheduling supported | Multi-provider offices; full schedule visibility |
| **Tab View** | One provider per tab | Same provider/same day only | Solo practitioners; focused single-provider view |

**How to switch:** Calendar → Calendar Settings → View Toggle

The calendar is the **default landing screen** when logging into a location. Everything flows from here.

### Appointment Booking

| Method | How | When to Use |
|--------|-----|------------|
| **Click to book** | Click empty time slot → select patient → confirm | Standard new appointment |
| **Drag-and-drop** | Drag patient from search to time slot | Quick booking |
| **Bulk Scheduler** | Patient Profile → Bulk Schedule | Schedule entire care plan (e.g., 18 visits 3×/week → 8 visits 2×/week) |
| **Web Scheduler** | Patient-facing widget on practice website | New patient self-scheduling only (not existing patients) |

### Bulk Scheduler

One of ChiroHD's **power features**. Schedules an entire care plan in one operation.

- **What it does:** Takes a Bulk Schedule Template (system-level) and creates all future appointments at once
- **Templates define:** Visit phases (e.g., Phase 1: 3×/week for 6 weeks, Phase 2: 2×/week for 4 weeks)
- **Templates can flag:** Specific visits as re-exams or x-ray appointments
- **Navigation:** Patient Profile → Scheduling → Bulk Schedule

**Problem solved:** Without bulk scheduling, front desk would need to manually book 20-50+ future appointments for each new care plan patient. At 20 new patients/week, that's 400-1,000 manual bookings.

**Limitation:** Limited ability to modify bulk-scheduled appointments after creation (user complaint from reviews).

### Rescheduling

Two methods:

| Method | Navigation | Capability |
|--------|-----------|-----------|
| **Drag-and-drop** | Click and hold → drag to new slot | Provider View: cross-provider. Tab View: same provider/same day only |
| **Drop-down menu** | Appointment tile → Down Arrow → Reschedule → Select Day → Select Time → Confirm | Any day, any provider; use when drag-and-drop is insufficient |

### Appointment Notes

- **What:** Free-text field attached to a specific appointment
- **Create:** Calendar → New Appointment → Appointment Note field → Save
- **Edit:** Appointment tile → Down Arrow → Edit Appointment → Note field → Save
- **Visual indicator:** Blue bar on left side of appointment tile signals a note exists
- **Preview:** Hover over tile to see note content without opening editor
- **Scope:** Appointment-specific; does not carry across visits

### Appointment Status & Visual Indicators

The calendar uses color and icon coding to communicate status at a glance:

| Status | Visual | Meaning |
|--------|--------|---------|
| **Scheduled** | Default tile color | Appointment booked, patient hasn't arrived |
| **Arrived** | Status change on tile | Patient checked in; appears in provider's Arrival Queue |
| **Intruded (Green)** | Green tile | SOAP note submitted by provider |
| **Completed (Gray)** | Grayed-out tile | Appointment + charges + SOAP note all present |

**Calendar icons** (individually toggleable per user):
- Appointment Status icon (scheduled/completed)
- SOAP Note icon (sticky note = note entered)
- Walk-In icon (figure = walk-in patient)
- Reminder Status (thumbs up = confirmed, X = denied)
- Language Preference icon
- Blue bar (appointment note exists)

> [!warning] UX Gap
> 8 appointment status icons with no in-app legend. Users must memorize color/icon meanings. Color-coded billing states (purple/yellow/blue/green) also have no guide.

---

## Provider Schedule Management

### Provider Hours (Recurring Schedule)

**What:** Defines a provider's normal, ongoing available time slots.
**Navigation:** Calendar → Gear Icon (on provider column/tab) → Provider Schedule tab

Key settings:
- **Columns** — number of simultaneous patients bookable per time slot
- **Minute Time Blocks** — increment for building hours (10/15/30 min); affects schedule-building only
- **Hour Intervals** — time labels on calendar left side; **all providers at a location must share the same setting**
- **Tab Color** — optional color coding per provider tab
- **Templated Hours** — system-level templates that auto-populate provider schedules

### Exception Hours (One-Day Override)

**What:** Override a provider's schedule for a single day (holiday, early closure, etc.).
**Navigation:** Calendar → Gear Icon → Exception Hours tab
**Scope:** Affects only the specified date; all other days remain unchanged.

### Future Schedule (Permanent Change)

**What:** A new schedule configured to replace the current one on a future date.
**Navigation:** Calendar → Gear Icon → Provider Schedule tab → Date Picker (change start date)
**Use case:** Provider changing from 4-day to 5-day weeks starting next month.
**Behavior:** Replaces the current schedule from the activation date forward.

### Calendar Tab Order

**What:** Controls left-to-right position of provider tabs.
**Navigation:** Live Location → Settings → Users → Calendar Users → Provider → Calendar Settings → Calendar Order
**Key detail:** Must be set from the live location view (not system level). Without manual ordering, defaults to calendar enable date.

**Push Pin:** A per-user override that pins a specific provider tab as the default view, regardless of Calendar Order.

---

## Patient Check-In Flow

### Check-In Device (Kiosk)

**What:** Self-service tablet/kiosk where patients check themselves in.
**Flow:** Enter phone number → Confirm identity → Check in → Status changes to "Arrived"
**Requirement:** Chrome browser; daily re-login required.

### Manual Arrival

**Navigation:** Calendar → Appointment tile → Down Arrow → Arrive
**Effect:** Marks patient as present; triggers visibility in provider's Arrival Queue.

### Arrival Queue

**What:** Provider-specific list of arrived/waiting patients.
**Where:** SOAP Notes screen (Provider Mode)
**Key detail:** Each provider sees only their own queue. Drives the provider's workflow — they pull patients from this queue.

### Patient Snapshot

**What:** Summary panel appearing when an appointment is clicked.
**Shows:** Current status, alerts, charges, payment info, upcoming appointments, Blue Patient Notes Box
**Used by:** Front desk at check-in to quickly assess patient state.

> [!note] UI Quirk
> When the Patient Snapshot panel is open, it blocks access to the ChiroHD Version Bar in the bottom-left corner. Must be closed first.

---

## Automations Related to Scheduling

- **Autocomplete:** When enabled, appointments auto-complete when calendar + charges + SOAP note are all present (no manual step needed)
- **Triggers:** Automated rules that fire on scheduling events (e.g., Appointment Type = "Day Two" AND Status = "Arrived" → attach Day Two Task List)
- **Patient Alert Templates:** Auto-remind staff at care plan milestones (e.g., visit 5: schedule scan; visit 12: discuss wellness)

---

## Key Strengths

1. **Bulk Scheduler** — genuinely powerful for chiropractic care plan scheduling
2. **Provider View** — multi-provider side-by-side visibility with cross-provider drag-and-drop
3. **Check-in device** — self-service kiosk reduces front desk bottleneck
4. **Arrival Queue** — clean handoff from check-in to provider workflow
5. **Calendar icons** — rich status communication at a glance (when learned)

## Key Gaps

| Gap | Impact | Competitor Benchmark |
|-----|--------|---------------------|
| **No smart scheduling / AI suggestions** | Front desk manually scans for openings | Jane App has intelligent slot suggestions |
| **No waitlist management** | Can't capture demand for full slots | Standard in Jane, ChiroTouch |
| **Web Scheduler is new-patient-only** | Existing patients can't self-schedule | Most competitors allow all patients |
| **No no-show prediction** | Reactive to no-shows, not preventive | SKED (now owned) has some capability |
| **Bulk scheduling modification limited** | Hard to adjust after creation | User complaint in reviews |
| **Check-in device requires Chrome, daily re-login** | Inconvenience for practices | Should be frictionless |

---

## Related Academy Lessons

- Section 2: ChiroHD Basics Workflow Tips (9 lessons)
- Section 3: Location Setup — Calendar lessons (lessons 3-6)
- Section 5: Users, Providers, and Calendars (6 lessons)
- Section 12: Patient Profile — Bulk Scheduler (lesson 4)

---

*Part of the [[ChiroHD Product Briefing]] series. See also: [[ChiroHD Feature Map]] for navigation paths.*
