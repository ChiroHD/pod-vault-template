---
type: reference
date: 2026-04-03
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/billing/charges
  - feature/billing/payments
---

# Transactions & Ledger

> The transaction system is how charges are created, payments are collected, and the patient's financial record (ledger) is maintained. Transactions can be initiated from the calendar, the patient profile, or triggered automatically by SOAP note submission.

---

## Access Points

| Entry Point | How to Get There | Use Case |
|-------------|-----------------|----------|
| **Calendar Dollar Icon** | Calendar → Click appointment → Dollar icon ($) | Quick payment collection at checkout |
| **Patient Profile** | Patient Profile → New Transaction (gray button) | Full transaction entry from within a patient record |
| **SOAP Note Submit** | SOAP → Submit note | Auto-posts default "Every Visit" charges |
| **Bulk from Preset** | Transaction screen → Apply preset | Apply a pre-configured bundle of charges |

---

## Transaction Screen

The transaction module is a multi-section interface:

### Charge Entry

| Element | Description |
|---------|-------------|
| **Service search** | Type a service name or CPT code to add a charge |
| **Inventory search** | Search inventory items to sell |
| **Preset selector** | Apply a Transaction Preset (pre-configured service bundle) |
| **Line items** | Each charge shows: Service, CPT Code, Units, Charge Amount, Write-Off, DX Codes, Modifiers |

### Payment Entry

| Element | Description |
|---------|-------------|
| **Payment Type** | Dropdown: Cash, Check, Fortis (card), Care Credit, ACH, Insurance Check, etc. |
| **Amount** | Payment amount |
| **Card Processing** | If Fortis selected: integrated card terminal or keyed entry |
| **Split Payment** | Distribute payment across multiple patients or accounts |
| **Transfer** | Redirect payment to a different patient's ledger |

### Transaction Actions

| Action | Description |
|--------|-------------|
| **Save** | Post the transaction to the ledger |
| **Print Receipt** | Generate a printable receipt |
| **Void** | Reverse a completed transaction (creates offsetting entry) |
| **Refund** | Process a payment refund (via merchant services) |

---

## Patient Ledger

The ledger is the complete financial history for a patient, visible in the patient profile under the Transactions tab.

### Ledger Entries

Each entry shows:

| Column | Description |
|--------|-------------|
| Date | Transaction date |
| Service/Item | What was charged or paid |
| CPT Code | Billing code |
| Provider | Which provider the charge is for |
| Charge | Amount billed |
| Payment | Amount paid |
| Adjustment | Write-offs, discounts, insurance adjustments |
| Balance | Running balance |
| Case | Which case the charge belongs to |

### Ledger Behaviors

| Behavior | Description |
|----------|-------------|
| **Auto-posting** | "Every Visit" default charges auto-post when SOAP is submitted |
| **Prepay credits** | Upfront payments create a positive balance drawn down by future charges |
| **Monthly auto-debit** | Membership charges post monthly (if configured) |
| **Insurance posting** | EOB/ERA processing posts insurance payments and adjustments |
| **Write-offs** | Can be contractual (insurance adjustment), professional courtesy, or care package discount |

---

## Payment Types

From the system-level Payment Types configuration:

| Type | Included in Cash Reports | Notes |
|------|------------------------|-------|
| ACH | Yes | Bank transfer |
| BridgePay | Yes | Alternative processor |
| Care Credit | Yes | Patient financing |
| Cash | Yes | Physical cash |
| Check | Yes | Paper check |
| Fortis | Yes | Primary card processor (terminal or keyed) |
| HFA | Yes | Healthcare financing |
| Insurance - VCC | Yes | Insurance virtual credit card |
| Insurance Check | Yes | Insurance paper check |
| Office Transfer | Yes | Internal account transfer |
| Payment on File | Yes | Stored card charge |
| Square | Yes | Alternative card processor |
| Visa | Yes | Direct card entry |
| Gift Certificate | No | Not counted as cash revenue |
| Plan | No | Care plan/membership payment |

---

## Transaction Presets

Pre-configured service bundles for one-click billing. Created at System Settings → Transaction Presets.

Each preset defines:
- A set of services with pre-filled prices
- Optional write-off amounts per service
- Optional write-off reasons

**Example presets:**
- "Adjustment + Hydrotherapy" — Wellness Adjustment ($45) + Hydrotherapy ($7, write-off $5)
- "Employee Discount" — Wellness Adjustment ($30) + Write Off ($30)
- "Facebook Special Day One" — X-rays + NP Brief with promotional write-offs

---

## Care Package Billing

Two models for prepaid care:

### Prepay (Pay-in-Full)

1. Patient pays full amount upfront (e.g., $1,500 for 36 visits)
2. Payment creates a positive ledger credit
3. Each visit auto-posts a charge against the credit
4. Credit decreases with each visit until exhausted
5. Write-off applied per visit (difference between standard price and package price)

### Monthly Auto-Debit

1. Patient agrees to monthly payment plan (e.g., $125/month for 12 months)
2. Monthly charge auto-posts via Stripe or stored card
3. Visits are charged at discounted rate
4. Ledger may show positive or negative running balance depending on payment timing vs. visit timing

---

## Insurance Payment Posting

Insurance payments flow from EOB/ERA processing into the ledger:

1. **Claim submitted** → Charges appear on ledger as billed amounts
2. **EOB/ERA received** → Insurance payment + adjustments posted
3. **Reason codes applied** → CO (write-off), PR (patient responsibility), OA (other)
4. **Remaining balance** → Transferred to patient responsibility or appealed

See [[Insurance Home & Claims]] and [[Insurance Home & Claims#ERA Auto-Allocation|ERA Management]] for the full insurance billing workflow.

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Charge** | A debit entry — services rendered or products sold |
| **Payment** | A credit entry — money received from patient or insurance |
| **Write-Off** | A reduction — contractual adjustment, discount, or professional courtesy |
| **Balance** | Net of charges minus payments minus write-offs |
| **Prepay Credit** | Positive balance from upfront payment, drawn down by future charges |
| **Void** | Reversal of a transaction (creates an offsetting entry, doesn't delete) |
| **Refund** | Return of payment to patient (requires merchant services for card refunds) |

---

## Related

- [[Services & Inventory]] — Service catalog and pricing
- [[Fee Schedules]] — Price overrides for specific payers or care plans
- [[Insurance Home & Claims]] — Insurance claim and payment workflow
- [[SOAP Notes]] — Auto-posting of charges on note submission
- [[Patient Records]] — Ledger view in patient profile
