---
type: reference
title: "Product Deep Dive — Clinical Documentation"
date: 2026-03-30
source: claude
tags:
  - type/reference
  - topic/product
  - topic/soap-notes
  - source/claude
---

# Product Deep Dive — Clinical Documentation (SOAP Notes)

> Part of the [[ChiroHD Product Briefing]] series.

---

## Overview

Clinical documentation is **ChiroHD's strongest module** and core differentiator. The macro-driven SOAP note system enables providers to document a routine chiropractic visit in approximately 20 seconds — genuinely competitive speed that lets high-volume chiropractors (40-80+ patients/day) keep up with their patient flow without documentation backlog.

**Primary persona:** Clinical Provider (Associate DC), Practice Owner
**Maturity:** Mature (macro system)

---

## What Problem Does This Solve?

Documentation burden is the **#1 pain point across all personas**:
- **Providers** drown in it — 20-50 SOAP notes per day, each for a 15-20 minute visit
- **Practice owners** hate it — eats into patient time and personal life
- **Billing staff** deal with the consequences — when notes are incomplete or inconsistent, claims get denied

In chiropractic specifically:
- Visits are **high-frequency** (2-3×/week per patient) — documentation volume is 3-5× other specialties
- Notes follow **repeatable patterns** — same adjustment types, same body regions, same objective findings for a given patient's care plan
- **Compliance requirements** demand specific documentation elements even when clinical findings are routine

ChiroHD's macro system solves this by letting providers build reusable note components that auto-populate with a click.

---

## Features

### SOAP Note Macro System

This is the heart of ChiroHD's clinical value.

**What macros are:** Pre-configured text blocks that auto-populate sections of a SOAP note. Instead of typing "Patient presents with moderate cervical pain, reduced range of motion in flexion and extension, tenderness at C5-C6...", the provider clicks a macro called "Cervical Routine" and the text populates instantly.

**How they work:**
1. Macros are configured at the system level (apply to all locations) or location level (overrides)
2. Each macro maps to a SOAP section (Subjective, Objective, Assessment, Plan)
3. Providers can chain multiple macros together in a single note
4. **"Reuse Last Note"** — copies the previous visit's note as a starting point, then the provider modifies only what changed

**Configuration:** Settings → System Settings → SOAP Macros (system level) or Location Settings → SOAP Macros (location override)

**Result:** A routine established patient visit can be documented in ~20 seconds. Staff become proficient within one day.

### SOAP Note Entry Points

ChiroHD has **four different ways** to create/view SOAP notes:

| Entry Point | Navigation | Use Case |
|-------------|-----------|----------|
| **Provider Mode** | SOAP Notes screen → Provider Mode | Primary workflow — shows Arrival Queue, work through patient queue |
| **Patient Mode** | SOAP Notes screen → Patient Mode | Focused view of a single patient's chart |
| **Patient Chart (beta)** | Patient Profile → Patient Chart | Newer interface; beta status |
| **Visits Tab** | Patient Profile → Visits | Historical note review |

> [!warning] UX Issue
> Four entry points for the same function creates confusion about which to use. This is a known fragmentation issue — newer interfaces ship without sunsetting older ones.

### Provider Mode

**What:** The primary clinical workflow view. Shows the arrival queue of all waiting patients for the logged-in provider.

**Flow:**
1. Patient checks in → appears in Arrival Queue
2. Provider clicks patient name → SOAP note opens
3. Provider selects macros / edits note → submits
4. Submission triggers: calendar tile turns green (Intruded Status), default charges auto-post

**Key detail:** Each provider sees only their own queue. This is by design — keeps high-volume providers focused.

### Patient Mode

**What:** Alternative view focused on a single patient's record rather than the queue. Used when accessing a specific patient's chart directly.

### SOAP Note Submission & Side Effects

When a provider submits a SOAP note, several things happen automatically:

1. **Calendar tile turns green** (Intruded Status) — visual signal to front desk that documentation is complete
2. **Default charges auto-post** to the patient's ledger (if configured on the case)
3. **Treated Symbol** appears on the calendar tile
4. **Autocomplete triggers** (if enabled) — if charges + SOAP + appointment all exist, the appointment marks complete and grays out

This chain of events is what makes the ~70-second check-in-to-checkout flow possible.

### Table Talk

**What:** A feature that allows providers to display uploaded x-rays during patient consultation within the SOAP screen.

**Use case:** During the visit, the provider pulls up the patient's x-ray imagery on screen to discuss findings with the patient while simultaneously documenting the visit.

**Related integrations:** PostureCo, MyoVision, Insight Subluxation Station (x-ray integrations)

### Note Completion Indicators

| Indicator | Meaning |
|-----------|---------|
| **Green tile (Intruded Status)** | SOAP note submitted |
| **SOAP Note Icon** (sticky note) | SOAP note entered for that visit |
| **Treated Symbol** | Provider has documented |
| **Grayed-out tile** | Complete: appointment + charges + SOAP all present |

### Incomplete Queue

When a SOAP note is submitted but charges haven't posted (due to misconfigured default charges, missing case setup, etc.), the appointment appears in the **Incomplete Queue**. This serves as a safety net to catch visits that need billing attention.

---

## Clinical Documentation Workflow by Visit Type

### Day 1 (New Patient)

| Step | What Happens | Time |
|------|-------------|------|
| Patient completes intake paperwork | Digital submission populates Documents tab | Pre-visit |
| Front desk creates patient record | Demographics, referral source | ~2 min |
| Provider performs exam | History, examination, possibly x-rays | 30-45 min |
| Provider documents | SOAP note with exam findings, initial DX codes | 3-5 min (more thorough than routine) |
| Charges posted | Manual for Day 1 (often unique charges) | ~1 min |

### Day 2 (Report of Findings)

| Step | What Happens | Time |
|------|-------------|------|
| Provider reviews findings with patient | Table Talk for x-ray display | 15-20 min |
| Care plan presented | Visit frequency, duration, expected outcomes | Part of appointment |
| Bulk scheduling | Front desk creates all future appointments | ~2 min |
| Case and default charges configured | Fee schedule attached, charge frequency set | ~3 min |
| SOAP note documented | Macro-driven, includes care plan documentation | ~1 min |

### Day 3+ (Established Patient / Routine Visit)

| Step | What Happens | Time |
|------|-------------|------|
| Patient checks in | Kiosk or front desk | ~30 sec |
| Provider sees patient in queue | Click patient in Arrival Queue | ~5 sec |
| Provider documents | Select macros → auto-populate | **~20 sec** |
| Charges auto-post | Default charges fire on SOAP submission | ~0 sec (auto) |
| Appointment auto-completes | Autocomplete triggers | ~0 sec (auto) |
| **Total** | | **~70 sec** |

This is where ChiroHD shines. The established patient workflow is genuinely fast.

---

## Key Strengths

1. **20-second macro notes** — genuinely competitive documentation speed for routine visits
2. **Reuse Last Note** — speeds up documentation for patients with consistent treatment plans
3. **Auto-posting default charges** — eliminates a manual step on every visit
4. **Arrival Queue** — clean provider workflow; work through patients systematically
5. **Staff proficiency in one day** — low training burden for the macro system
6. **Chiropractic-specific terminology** — macros use adjustment, subluxation, spinal region language natively

## Key Gaps

| Gap | Impact | Competitor Benchmark |
|-----|--------|---------------------|
| **No AI-assisted documentation** | Existential competitive threat | ChiroTouch Rheo AI: 8.5 min → 42 sec ambient scribe; SpryPT, Pabau, splose all have AI |
| **No ambient capture** | Providers must still interact with the screen during/after the visit | Freed, DeepCura, ChiroScript offer ambient AI that listens and generates notes |
| **No real-time compliance checking** | Incomplete notes can be saved without validation | Emerging in competitors |
| **Template rigidity** | Macros are powerful but don't adapt to unusual presentations | AI-driven systems generate flexible notes from conversation |
| **Four entry points** | User confusion about which SOAP screen to use | Should consolidate to one clear path |
| **No cross-visit trending** | Can't easily see how a patient's findings change over time | Would be valuable for care plan management |
| **Red/blue indicators confuse users** | Note status indicators aren't intuitive | Documented in help center as a confusion point |

### The AI Threat

This deserves special emphasis. ChiroHD's SOAP note speed (20 seconds) is built on a **manual macro system** — providers click pre-configured text blocks. Competitors are shipping **AI-driven documentation** where:

- **Ambient AI scribes** listen to the patient encounter and auto-generate the SOAP note
- The provider reviews and approves rather than constructing
- Documentation happens **during** the visit, not after

ChiroTouch's Rheo AI claims to reduce documentation from 8.5 minutes to 42 seconds — comparable to ChiroHD's macro speed, but with zero provider interaction needed.

Point solutions like ChiroScript, DeepCura, and Freed plug into **any EHR**, meaning a ChiroHD practice could add an AI scribe without switching platforms — which directly unbundles ChiroHD's documentation advantage.

**ChiroHD's opportunity:** Leverage the chiropractic-specific data corpus from 5,000+ practices to build AI documentation that's more accurate for chiropractic than generic solutions. The specialty training data is the moat — but only if leveraged before competitors commoditize AI documentation.

---

## Related Academy Lessons

- Section 2, Lessons 1-3: Basic workflows, Day 1/2/3 walkthrough, automations
- Section 3, Lesson 15: Office Configuration — New Macro Layout
- Section 3, Lesson 29: Location-Specific SOAP Macros
- Section 14: SOAP Notes Screen (full section)
- Section 14: SOAP Note Macros (full section)

---

*Part of the [[ChiroHD Product Briefing]] series. See also: [[ChiroHD Feature Map]] for navigation paths.*
