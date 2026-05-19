---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/clinical/soap
---

# SOAP Notes

> The SOAP (Subjective, Objective, Assessment, Plan) note system is the core clinical documentation tool in ChiroHD. Providers use it to document each patient visit, and submitted notes trigger downstream billing workflows.

---

## Access

Three entry points via the **SOAP** dropdown in the location-level top nav:

| Mode | Purpose | URL |
|------|---------|-----|
| **Provider Mode** | Primary clinical workflow — provider works through arrival queue, opens notes, uses macros | Default SOAP view |
| **Patient Mode** | Search for any patient and view/edit their notes | Patient-centric note access |
| **Patient Chart (BETA)** | Redesigned patient chart interface | New UI for comprehensive patient view |

> [!warning] Deprecation Notice
> **Provider Mode is being deprecated** in favor of Patient Chart Beta. Migration is ongoing but stalled — no timeline or plan has been stated. Patient Chart Beta is the recommended default (pin Notes view). *(Source: Dr. C product training, 2026-04-10)*

---

## Provider Mode

The primary clinical workflow. Layout:

**Left sidebar:**
- Patient search dropdown
- Treatment area selector (Table 1, Exam Room, Table 2, X-ray Room — matches configured treatment areas)
- Arrival queue — patients grouped by status:
  - **New Patients** — first visit
  - **Scheduled** — booked but not yet arrived
  - **Arrived** — checked in, waiting
  - **In Progress** — SOAP note started
- "Select a patient from the left" prompt when no patient is selected

**Main area (when patient selected):**
- **SOAP note editor** — large text area on the left where note content accumulates
- **Macro panel** — center/right area with clickable macro building blocks organized in tabs
- **Top-right panel:**
  - Chief Complaint field
  - Care Notes
  - Patient Notes
  - Re-use Last Note button (respects case boundaries as of v6.1.0)
  - Re-use Last Note - Same Provider option (new in v6.1.0)
- **Bottom panel:**
  - Charge entry area (default services auto-populate)
  - Tabs: X-Rays, Thermal Images, EMGs, HRV, Posture, Documents, Forms

**Actions:**
- **Save as Draft** — preserves work without triggering charges or completion
- **Submit** — finalizes the note, electronically signs it, triggers charge auto-posting and appointment status change

---

## The Macro System

SOAP macros are the productivity engine of clinical documentation. They are clickable building blocks that auto-populate sections of the note with structured clinical language.

### Macro Tabs

The macro panel is organized into tabs:

| Tab | Content |
|-----|---------|
| **SOAP** | Primary macro set — Subjective, Objective, Assessment, Plan building blocks |
| **CCS** | Chief Complaint / Subjective-focused macros |
| **Exam** | Examination-specific macros |
| **SOAP (2)** | Additional SOAP macros (overflow) |
| **X-Ray** | Radiology-specific documentation macros |
| **test / custom tabs** | User-created tabs (system admin can add custom tabs) |

### Key Macro Building Blocks (SOAP tab)

97 building block slots available. Key macros:

**Subjective (patient's complaint):**
- Subjective 1 — Primary subjective narrative
- Subjective - Child — Pediatric-specific subjective
- Subjective 2 — Secondary subjective detail
- Subjective - Improvement — Document improvement since last visit
- Subjective 3 — Additional subjective information
- Subjective - Medicare PDQ — Medicare-compliant subjective documentation
- Subjective - Exacerbation — Document worsening symptoms
- Update Evaluation - Subjective — Re-evaluation subjective update

**Objective (provider's findings):**
- Spinal Palpation — Palpatory findings by spinal region
- Extremity Palpation — Non-spinal palpation findings
- Trigger Points — Myofascial trigger point documentation
- Postural Analysis — Postural observations
- Spinal ROM (Visual) — Range of motion findings
- Spinal Ortho Tests — Orthopedic test results
- Neuro — Neurological examination findings

**Assessment & Plan:**
- Assessment — Clinical assessment/impression
- Plan — Treatment plan documentation
- Tx Plan Update — Care plan progress update
- Prognosis — Prognostic statement

**Treatment Documentation:**
- Adjustment - Regional — Regional adjustment documentation (cervical, thoracic, lumbar)
- Adjustment - Segmental — Specific vertebral segment adjustments
- Therapies — Therapeutic modalities (E-stim, ultrasound, traction, etc.)
- Response — Patient response to treatment

**Other:**
- Additional Notes — Free-form additional documentation
- Referral - MRI — MRI referral documentation
- Referral - Specialist — Specialist referral documentation
- Electronic Signature / Electronic Signature - Dr. X — Digital signature blocks
- FULL SOAP — Complete SOAP note in one macro (all sections)
- Massage Note — Massage-specific documentation
- Wellness SOAP 1/2/3 — Quick wellness visit documentation
- Pronoun? — Gender pronoun selection macro

### How Macros Work

1. Provider clicks a macro building block (e.g., "Subjective 1")
2. A modal or inline panel presents clickable options/prompts (e.g., body region, pain type, severity)
3. Provider clicks through the options
4. The macro generates formatted clinical text and appends it to the note area
5. Provider can edit the generated text before moving to the next section
6. Multiple macros can be used to build up the complete note

### Macro Management

- **System macros** — shared across all locations, managed at the system level (read-only at location level)
- **Location macros** — location-specific macros that override or supplement system macros
- **Test Mode** — macros can be tested without affecting patient records
- **Macro Builder** — system admin interface for creating and editing macro building blocks
- **"Click here to switch to location specific macros"** — toggle between system and location macros

> [!warning] Critical Implication
> When a clinic switches to a Location Macro, they are **silently disconnected from all future system-level improvements**, including the upcoming **AI SOAP Note Builder**. There is no mechanism to notify or re-engage these clinics. This is an adoption risk — especially for enterprise accounts (like franchises) where some locations may unknowingly opt themselves out of the best features. *(Source: Dr. C product training, 2026-04-10)*

---

## Charge Auto-Posting

When a SOAP note is **submitted** (not just saved as draft):

1. The system checks the patient's case for **default services** with "Every Visit" charge frequency
2. Those services are automatically posted as charges to the patient's ledger
3. The appointment status changes to reflect completion
4. The note is electronically signed and locked for editing

**"Time of Purchase"** services are NOT auto-posted — they appear as reference prices but must be manually entered.

---

## Patient Mode

A patient-centric view for accessing notes outside the arrival queue workflow:

1. Search for a patient by name
2. View all of their SOAP notes chronologically
3. Open any note to view or edit (if not yet submitted)
4. Create a new note for a specific date

---

## Patient Chart (BETA)

A redesigned comprehensive patient interface that combines:
- Patient demographics
- SOAP notes
- Appointments
- Billing/transactions
- Insurance
- Documents

Patient Chart (BETA) provides an alternative, consolidated view of patient data. It is currently in BETA testing and available as an option alongside Provider Mode and Patient Mode.

---

## Spinal Listings

*(Source: Dr. C product training, 2026-04-10)*

An interactive vertebral chart used to record subluxation findings during examination.

**Vertebral segments (top to bottom):**
- **Occiput** (base of skull)
- **Cervical:** C1 through C7
- **Thoracic:** T1 through T12
- **Lumbar:** L1 through L5
- **Sacrum**
- **ILIUM** (left and right)

**How it works:**
- Each vertebra is clickable on both the **left and right sides** to record subluxation findings
- Standard listing abbreviations: **PR** (Posterior Right), **PRI** (Posterior Right Inferior), **PRS** (Posterior Right Superior), **AR** (Anterior Right), **ARI** (Anterior Right Inferior), **ABS** (Anterior Body Superior)
- Color-coded for visual reference during adjustments
- Used to flag contraindications (e.g., spondylolisthesis displayed in red)
- Listings feed directly into the SOAP note exam documentation (Objective section)

---

## AI SOAP Note Builder (Beta)

*(Source: Dr. C product training, 2026-04-10)*

- Currently in **beta testing** as of April 2026
- Only available to clinics using the **System Macro** (not Location Macro) — clinics that have switched to Location Macros are silently excluded (see [[SOAP Notes#Macro Management|Macro Management warning]])
- Dr. C was working to get access to begin testing during the April 10 training session
- Will auto-generate SOAP notes from exam findings, reducing documentation time for providers

---

## Key Behaviors

| Behavior | Description |
|----------|-------------|
| **Note locking** | Submitted notes are locked and cannot be edited. Draft notes can be edited until submitted. |
| **Charge posting** | Only happens on Submit, not on Save as Draft |
| **Case boundary** | Re-use Last Note respects case boundaries (v6.1.0+) — won't pull a note from a different case |
| **Provider assignment** | When SOAP Assignment Options is enabled, a note can be assigned to a different provider than the one who has the calendar appointment |
| **Draft Notes counter** | The sidebar shows a count of draft notes — visible to the entire location as a quality indicator |
| **Incomplete Appts counter** | Shows appointments that lack a submitted SOAP note |

---

## Related

- [[SOAP Notes#The Macro System|SOAP Macros (this note)]] — System-level macro management and building blocks
- [[Calendar & Scheduling]] — How appointments feed into the SOAP workflow
- [[Transactions & Ledger]] — How charge auto-posting affects the patient ledger
- [[Office Configuration]] — Toggles: Use System Macros, SOAP Assignment Options, Show Patient Notes, New Macro Layout
