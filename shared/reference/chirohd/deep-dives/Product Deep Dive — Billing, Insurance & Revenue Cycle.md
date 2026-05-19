---
type: reference
title: "Product Deep Dive — Billing, Insurance & Revenue Cycle"
date: 2026-03-30
source: claude
tags:
  - type/reference
  - topic/product
  - topic/billing
  - topic/insurance
  - source/claude
---

# Product Deep Dive — Billing, Insurance & Revenue Cycle

> Part of the [[ChiroHD Product Briefing]] series.

---

## Overview

Billing and insurance is ChiroHD's **most complex and most painful module**. It works well for cash-pay charge posting and care package management, but insurance claims processing is the #1 source of support burden — responsible for **27% of all help center content** (57 articles) with **40 distinct Greensheet error types**, each requiring a different fix in a different location.

**Primary persona:** Billing / Admin Staff
**Secondary persona:** Practice Owner (revenue visibility)
**Maturity:** Moderate (charge posting) to Immature (insurance claims)

---

## What Problem Does This Solve?

Chiropractic billing has unique patterns:
- **Care packages** (prepay and monthly auto-debit) are the dominant payment model, not fee-for-service
- **High-frequency visits** mean charge posting happens 40-80+ times/day per provider
- **Insurance complexity** — chiropractic has specific code requirements, visit limits, and payer rules
- **Split payment models** — some patients pay cash + insurance, or family members share payments
- **Write-offs** are routine (professional courtesy, prepay discounts)

The billing module handles everything from charge entry to payment processing to insurance claim generation.

---

## Cash / Direct-Pay Features

### Transaction Module

The central payment and charge interface. Accessible from multiple entry points:

| Entry Point | Navigation |
|-------------|-----------|
| **Quick button (calendar)** | Calendar → Appointment tile → Dollar Icon ("Transaction") |
| **Patient profile** | Patient Profile → New Transaction (gray button) |
| **Post-SOAP auto-trigger** | Automatic when default charges are configured |

### Charge Posting

| Method | How It Works | When Used |
|--------|-------------|-----------|
| **Default charges (auto)** | Pre-configured on patient's case; auto-post when SOAP note submitted | Established patients with active care plan — this is the routine path |
| **Manual charges** | Staff selects services in Transaction Module | New patient Day 1, special services, ad-hoc charges |
| **Presets** | Pre-configured charge groupings (e.g., "Day 2", "Facebook Special") | Standardized visit types, promotions |

### Default Charges (The Automation Engine)

This is the key to ChiroHD's fast checkout flow. When configured correctly:

1. Provider submits SOAP note
2. System checks: does this patient's case have Default Charges with "Every Visit" frequency?
3. If yes → charges auto-post to the ledger with appropriate write-offs
4. Combined with Autocomplete → the appointment marks complete with zero manual billing steps

**Configuration:** Patient Profile → Cases → Default Charges Tab → Add service → Set write-off amount + reason + charge frequency → Save

**Key fields:**
- **Charge Frequency** — "Every Visit" for auto-posting; other options for periodic charges
- **Write-Off Amount** — difference between standard rate and discounted package rate
- **Write-Off Reason** — label for reporting (e.g., "Prepay", "Professional Write-Off")

### Payment Processing (Fortis Integration)

**Fortis** is ChiroHD's primary integrated payment processor.

| Feature | Description |
|---------|-------------|
| **Card payments** | Swipe, tap, or manual entry within ChiroHD |
| **Card Vault** | Save patient's card on file for future transactions |
| **Split Payment** | Distribute a single payment across multiple patient accounts |
| **Transfer** | Redirect payment to a different patient's account within same transaction |

**Limitations:**
- Fortis Financial Tab is **read-only** — refunds must be processed through the Fortis merchant portal
- Fortis Paylinks (payment links) don't auto-link back to patient records in ChiroHD
- Split payment sync can fail — documented workaround is to use ChiroHD's Transfer function instead

**Alternative:** Cash Practice Systems (second payment processor option)

### Patient Ledger

The running financial record for each patient:
- Shows all charges, payments, write-offs, and credits
- **Prepay credit** = positive balance when patient paid upfront (decreases each visit)
- End state: ledger balance = $0 when total services = total payments
- **Blue Patient Notes Box** on Patient Snapshot used for payment status notes visible at check-in

---

## Care Packages

Care packages are the **dominant billing model** in chiropractic. ChiroHD supports two payment structures, each configurable via two methods:

### Payment Structures

| Structure | How It Works | Ledger Behavior |
|-----------|-------------|----------------|
| **Prepay** | Patient pays full discounted amount upfront | Creates a credit that decreases each visit until $0 |
| **Monthly Auto-Debit** | Same pricing but collected in monthly installments | Fluctuates between credit and balance depending on payment timing |

### Configuration Methods

| Method | Setup Path | When to Use |
|--------|-----------|------------|
| **Default Charges** | Patient Profile → Cases → Default Charges Tab | Simple packages; one-off pricing per patient |
| **Fee Schedules** | System Settings → Fee Schedules → Build → Attach at patient case | Standardized packages across patients (recommended for consistency) |

### Fee Schedules

System-level pricing templates that override standard service rates.

**How to build one:**
1. System Level Dashboard → System Settings → Fee Schedules → Create New
2. Name descriptively (e.g., "12-Visit Prepay Package — $600")
3. Add services — two methods:
   - **Import All Services** (then edit relevant rates) — bulk approach
   - **Add Included Services Only** (recommended) — simpler, less error-prone
4. Set write-off amounts and reasons per service
5. Save at system level

**How to attach:**
1. Patient Profile → Cases → Default Charges Tab → Fee Schedule dropdown
2. Select the fee schedule
3. Set charge frequency to "Every Visit"
4. Collect payment

**Key detail:** Fee schedules must be built at the system level before they can be attached to patient cases. This is a one-time setup per package type.

---

## Insurance Claims

### The Problem

Insurance billing is the most frustrating workflow in ChiroHD. The evidence:
- **57 help center articles** dedicated to insurance (27% of all content)
- **40 distinct Greensheet error types** with individual fix guides
- **ERA (Electronic Remittance Advice)** in ambiguous beta status
- **No pre-submission validation** — errors discovered only after generation
- **No denial workflow** — denied claims require manual rebuild and resubmit

### Insurance Versions (Active Simultaneously)

| Version | Status | Notes |
|---------|--------|-------|
| **Insurance V1** | Legacy | Older claim generation |
| **Insurance V2** | Active | Current standard |
| **Insurance V3** | Newer | Being rolled out |
| **ERA** | Beta (334 clinics) | Electronic remittance |
| **New paradigm** | Emerging | Latest approach |

> [!warning] Fragmentation
> Having 3-4 insurance versions active simultaneously creates confusion for users, support staff, and documentation. Which version a practice is on affects their workflow and available features.

### Claim Submission Flow

| Step | What Happens | Friction Level |
|------|-------------|---------------|
| 1. Verify eligibility | Beta feature; incomplete; must also call insurer to confirm | **High** |
| 2. Generate claim | Click "Generate" on ledger → system creates X12 file | Moderate |
| 3. Review Greensheet | Check generated claim for errors | **Variable** |
| 4. Fix Greensheet errors | Decode error → navigate to correct settings → fix → regenerate | **Very High** (40 error types, each in different location) |
| 5. Submit to clearinghouse | Click "Send to Clearinghouse" (SFTP upload) | Low (if SFTP configured) |
| 6. Finalize in clearinghouse portal | Log into Office Ally/TriZetto → finalize → transmit to payer | **High** (separate system, separate login) |
| 7. Post ERA/EOB | ERA pre-fills data; manual review + approval | Moderate |

### Greensheet Errors

The "green sheets" are the billing/claim output documents. When claim data is incomplete or incorrect, errors appear on the green sheet. There are **40 documented error types**, each requiring a different fix in a different area of the system.

**Example errors:**
- Unknown Provider → provider not linked to provider record in Settings → Providers
- Missing NPI → MPI field empty on provider record
- Missing EIN → no tax ID on provider record
- Zero vs. blank in insurance fields → silently changes billing behavior

**The core problem:** There is no pre-submission validation. Users generate a claim, discover errors, look up the fix in help articles, navigate to the correct settings area, fix the data, regenerate, and repeat. A pre-submission wizard would prevent the majority of these errors.

### Clearinghouse Integrations

| Clearinghouse | Integration Type | Notes |
|---------------|-----------------|-------|
| **Office Ally (Service Center)** | SFTP upload of X12 files | Requires separate portal login to finalize; SFTP credentials ≠ portal login (common confusion) |
| **TriZetto** | SFTP upload | Alternative clearinghouse |
| **Availity** | SFTP upload | Third option |

**Key detail:** Claims don't transmit directly to payers from ChiroHD. They go ChiroHD → clearinghouse (SFTP) → clearinghouse portal (manual finalization) → payer. This multi-step process adds friction.

### ERA (Electronic Remittance Advice)

**What:** Electronic payment/denial notifications from payers that pre-fill into ChiroHD for review and posting.

**Status:** Beta — 334 clinics enabled as of March 2026. Ambiguous GA status.

**How it works:**
1. Payer sends ERA to clearinghouse
2. ChiroHD pulls ERA data
3. Data pre-fills into review screen
4. Staff reviews line items and approves posting

**Critical limitations:**
- ERA doesn't auto-post — requires manual review and approval for every line item
- **Posted EOBs cannot be edited** — if a line item is posted incorrectly, the entire EOB must be deleted and re-entered from scratch
- Derek Roemhildt is the single point of knowledge for ERA

### New Patient Insurance Setup

Setting up a new insurance patient requires multiple steps across multiple screens:

| Step | Fields/Actions | Time |
|------|---------------|------|
| Create insurance case | Case type, name, start date | ~1 min |
| Add DX codes | Search/select ICD-10 codes, set start dates | ~2 min |
| Create insurance policy | 15+ fields: payer, member ID, group ID, relationship, coverage dates, deductible, copay, max visits, qualifier | ~5 min |
| Set default services | Add services, set frequencies, attach fee schedule | ~3 min |
| Validate data | Manual check against insurance card / payer requirements | ~2 min |

**Total: 10-15 minutes per new insurance patient.** At 20+ new patients/week, this is a significant burden.

**Missing:** Insurance card OCR, payer auto-detection, smart defaults based on payer.

---

## Key Strengths

1. **Default charges auto-posting** — eliminates manual billing on routine visits
2. **Care package flexibility** — supports both prepay and monthly auto-debit
3. **Fee schedules** — standardized pricing across patients
4. **Fortis integration** — in-app payment processing
5. **Presets** — fast charge entry for standardized visit types
6. **Write-off tracking** — clean ledger management for package discounts

## Key Gaps

| Gap | Severity | Impact |
|-----|----------|--------|
| **No pre-submission claim validation** | Critical | 40 error types discovered only after generation |
| **ERA in ambiguous beta** | High | 334 clinics; no clear GA timeline; competitive gap |
| **Posted EOBs non-editable** | High | Hours of rework for single-line errors |
| **No denial workflow** | High | Manual rebuild and resubmit for every denial |
| **Insurance eligibility incomplete** | High | Beta feature explicitly told "also call insurer" |
| **No insurance card OCR** | Medium | 10-15 min manual entry per new insurance patient |
| **Fortis Financial Tab read-only** | Medium | Refunds require separate merchant portal |
| **No deductible auto-tracking** | Medium | Manual tracking of patient deductible status |
| **No auto-coding from SOAP** | Medium | SOAP notes don't suggest appropriate billing codes |

---

## Related Academy Lessons

- Section 2, Lessons 4-6: Care packages (Default Charges and Fee Schedules)
- Section 2, Lesson 9: How to Run a Payment
- Section 3, Lessons 10, 17: Historical Billing Info, Merchant Services
- Section 8: Location Settings (Services, DX Codes)
- Section 11, Lessons 6-7, 9: Rebill Utility, Insurance Renewal, Monthly Payments
- Section 12, Lessons 5-6, 11-15: Cases, Default Services, Ledger, Insurance History, Bill History, Financial Tab
- Section 15-18: Insurance V1, V2, V3 (full sections)
- Help Center: 57 insurance articles, 40 Greensheet error guides

---

*Part of the [[ChiroHD Product Briefing]] series. See also: [[ChiroHD Feature Map]] for navigation paths.*
