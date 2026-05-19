---
type: reference
date: 2026-04-03
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/billing/insurance
---

# Fee Schedules

> Fee schedules override the default service prices for specific payers (insurance) or specific patient arrangements (cash care plans). They define what a payer allows or what a patient pays for each service, enabling accurate billing and automated write-off calculations.

---

## Access

System Settings → Fee Schedules

---

## Two Types

### Insurance Fee Schedules

Define the **allowed amount** each insurance payer will pay for each service. When an insurance fee schedule is applied to a patient's case, the system knows:
- What to bill the payer (standard charge)
- What the payer will actually pay (allowed amount)
- What to write off (difference = contractual adjustment)

**Examples from the system:**
- BCBS 2024, BCBS 2025 — Blue Cross Blue Shield fee schedules by year
- Medicare — Medicare-allowed amounts
- Various test schedules

### Cash Fee Schedules

Define **discounted pricing** for care packages, membership plans, or special arrangements. When a cash fee schedule is applied to a patient's case:
- The charge posts at the fee schedule price (not the standard price)
- The difference between standard and fee schedule price is automatically written off
- The write-off reason is populated from the fee schedule configuration

**Examples from the system:**
- 12 Visit - Monthly A/D ($600) — 12-visit package with monthly payments
- 12 Visit - Prepay ($600) — 12-visit package paid in full
- 24 Adjustment Care Plan (PIF) $3,000 — 24-visit plan, pay-in-full
- 36 Wellness 10% Pre-Pay — 36 visits with 10% prepay discount
- Monthly Plan — recurring monthly membership pricing
- BCBS (Patient Responsibility Services) — insurance patient's copay/coinsurance rates

---

## Fee Schedule Structure

Each fee schedule contains a list of services with overridden pricing:

| Field | Description |
|-------|-------------|
| **Service** | The service being priced (linked to system service catalog) |
| **CPT Code** | Auto-populated from the service |
| **Allowed Amount** | What the payer pays (insurance) or what the patient pays (cash) |
| **Write-Off Amount** | Difference between standard price and allowed amount |
| **Write-Off Reason** | Category (e.g., "Contractual", "Professional Courtesy", "Prepay Discount") |
| **Units** | Default units for this service under this schedule |

---

## How Fee Schedules Are Applied

### To a Patient Case

1. Open the patient profile → Cases tab
2. Select the case (Insurance or Cash)
3. Go to the **Defaults / Patient Rates / Recurring Charges** tab
4. Select a **Fee Schedule** from the dropdown
5. The fee schedule overrides the default prices for all services in the case

When a fee schedule is applied:
- Default charges show the fee schedule's allowed amount instead of the standard price
- Write-offs are automatically calculated
- The ledger reflects the discounted pricing

### To Insurance Claims

When an insurance fee schedule (e.g., "BCBS 2025") is applied to an insurance case:
- Claims are submitted at the standard charge amount (what you bill)
- When the EOB comes back, the fee schedule's allowed amount is used to validate the payer's response
- Discrepancies between expected and actual allowed amounts are flagged

---

## Fee Schedule Management

**Actions:**
- **Create System Fee Schedule** — build a new fee schedule
- **Show Inactive** — view deactivated fee schedules
- **Deactivate** — retire a fee schedule without deleting (existing patient cases retain the pricing)

**Best practices:**
- Create year-specific insurance fee schedules (e.g., "BCBS 2025", "BCBS 2026") as allowed amounts change annually
- Use descriptive names that include the payer and year
- Keep care package fee schedules separate from insurance fee schedules
- Deactivate old fee schedules rather than deleting them — existing patients may still be on the old pricing

---

## Relationship to Other Billing Features

```
Service Catalog (standard prices)
        ↓
Fee Schedule (overrides prices for specific payers/plans)
        ↓
Patient Case Defaults (applies fee schedule to a patient)
        ↓
SOAP Note Submission (auto-posts charges at fee schedule price)
        ↓
Ledger (shows charges, write-offs, and patient balance)
```

---

## Related

- [[Services & Inventory]] — Standard service pricing that fee schedules override
- [[Transactions & Ledger]] — How fee schedule pricing appears on the ledger
- [[Insurance Home & Claims]] — How insurance fee schedules affect claim processing
- [[Patient Records]] — Applying fee schedules to patient cases
