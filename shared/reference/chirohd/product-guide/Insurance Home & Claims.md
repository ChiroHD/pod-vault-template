---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/billing/insurance
---

# Insurance Home & Claims

> The Insurance module is the most complex area of ChiroHD, handling claims generation, EOB processing, ERA auto-allocation, accounts receivable tracking, and payer management. Accessed via the **Insurance** top nav item at the location level.

---

## Insurance Menu Structure

| Menu Item | Purpose |
|-----------|---------|
| **Insurance Home** | Dashboard with pending EOBs, ERA status, and accounts receivable summary |
| **Create Claim** | Generate a new GreenSheet (CMS-1500 claim form) from patient charges |
| **Claim History** | View and manage previously submitted claims |
| **Process EOB (LEGACY)** | Older EOB processing interface (being replaced) |
| **Process EOB** | Current EOB processing interface for posting insurance payments |
| **EOB History** | View previously processed EOBs |
| **Settings** | Insurance-specific settings (Printer Configs, Overrides, Insurance/Third-Party Payers, Reason Code Behaviors) |

---

## Insurance Home Dashboard

The landing page of the Insurance module. Contains three primary sections:

### Pending EOBs

| Tab | Description |
|-----|-------------|
| **All Pending** | All EOBs awaiting processing |
| **Ready** | EOBs parsed and ready for review |
| **Incorrect** | EOBs with parsing errors that need manual review |
| **Show Manually Entered** | Toggle to include manually entered EOBs |

When no EOBs are pending: "You have no pending EOBs"

### ERAs (Electronic Remittance Advice)

| Tab | Description |
|-----|-------------|
| **Failed** | ERA files that could not be parsed or matched |
| **Processed** | Successfully processed ERA files |
| **Show Manually Entered** | Toggle to include manually uploaded ERAs |
| **Upload ERA** | Button to manually upload an ERA file (X12 835 format) |

ERA flow: SFTP download → Parse → Match to patients/claims → Auto-allocate payments → Review and Post

### Accounts Receivable

Aging summary broken into two categories:

**General Insurance Total:**

| Aging Bucket | Description |
|-------------|-------------|
| 0-30 days | Current receivables |
| 31-60 days | Aging receivables |
| 61-90 days | Overdue receivables |
| 91+ days - 2 Years | Significantly overdue |
| > 2 Years | Very old receivables |
| Addressable Unbilled | Charges that haven't been claimed yet |

**Personal Injury / Workers Comp Total:**

| Aging Bucket | Description |
|-------------|-------------|
| 0-30 days | Current |
| 31-60 days | Aging |
| 61-90 days | Overdue |
| 91+ days | Significantly overdue |

---

## Insurance Settings

Located under Insurance → Settings at the location level.

### Sub-pages:

| Setting | Purpose |
|---------|---------|
| **Printer Configs** | Configure claim form printing (paper CMS-1500 layout) |
| **Overrides** | Location-specific billing overrides |
| **Insurance / Third-Party Payers** | Manage payer directory at the location level |
| **Reason Code Behaviors** | Configure how insurance reason codes (CO45, PR1, etc.) affect patient responsibility, write-offs, and account balances |

### Insurance / Third-Party Payers

Manages the payer directory. Four tabs:

| Tab | Description |
|-----|-------------|
| **Payers** | All payers accessible to this location |
| **Payer Groups** | Groupings of payers for aggregate management |
| **System Payers** | Payers available at the system level (can be added to location) |
| **Location Payers** | Payers specifically configured for this location |

**Per-payer fields:**
- Payer name
- Payer type
- Electronic submission info
- Clearinghouse routing
- Address

### Reason Code Behaviors

Configures how insurance adjustment reason codes affect billing:
- **CO (Contractual Obligation)** codes → typically write-off
- **PR (Patient Responsibility)** codes → transfer to patient balance
- **OA (Other Adjustment)** codes → configurable behavior
- Each reason code can be mapped to: Write-off, Patient Responsibility, or Manual Review

---

## Claim Workflow

### Creating a Claim (GreenSheet)

1. Navigate to Insurance → Create Claim
2. Select the patient
3. Select the insurance case
4. Select date range for charges to include
5. Review the auto-populated CMS-1500 fields:
   - Box 1a: Insurance ID number
   - Box 6: Relationship to insured
   - Box 11: Group number
   - Box 14: Date of current illness
   - Box 17: Referring provider
   - Box 21: Diagnosis codes (from case DX)
   - Box 23: Prior authorization number
   - Box 24: Line items (services, dates, charges, DX pointers, modifiers)
   - Box 27: Accept assignment
6. Submit claim (electronic via clearinghouse, or print paper CMS-1500)

### Claim History

View previously submitted claims with filters:
- Date range
- Status (Submitted, Accepted, Rejected, Paid, Denied)
- Payer
- Patient

---

## EOB Processing

### Process EOB (Current)

For posting insurance payments against claims:

1. Select the EOB from the pending queue (or search by patient/claim)
2. Match the EOB to the original claim
3. For each line item, review:
   - Billed amount
   - Allowed amount
   - Paid amount
   - Adjustment amounts by reason code
   - Patient responsibility (copay, coinsurance, deductible)
4. Post the payment to the patient's ledger
5. Determine remaining balance: patient responsibility, write-off, or appeal

### ERA Auto-Allocation

When ERA (BETA) is enabled:
1. ERA files are downloaded automatically via SFTP from the clearinghouse
2. The system parses the X12 835 file
3. Matches ERA line items to existing claims by claim number, patient, and dates of service
4. Auto-allocates payments, adjustments, and patient responsibility based on reason codes
5. Presents a **Review and Post** interface for human verification before finalizing
6. Billing staff can review auto-allocated amounts and override if needed
7. Post to finalize

**ERA status indicators:**
- **Ready** — Parsed and matched, ready for review
- **Failed** — Could not parse or match
- **Processed** — Successfully posted

---

## Key Insurance Concepts

| Concept | Description |
|---------|-------------|
| **GreenSheet** | ChiroHD's term for a CMS-1500 claim form. Named after the green paper historically used for insurance claims. |
| **ERA** | Electronic Remittance Advice — the electronic version of an EOB, delivered via SFTP in X12 835 format |
| **EOB** | Explanation of Benefits — the insurance company's response to a claim, showing what was paid and what the patient owes |
| **Clearinghouse** | Intermediary that routes electronic claims between the practice and insurance payers |
| **Reason Code** | Standardized codes explaining why an insurance payment differs from the billed amount (e.g., CO45 = contractual write-off) |
| **Accept Assignment** | Whether the provider accepts the insurance-allowed amount as full payment (Box 27 on CMS-1500) |
| **Addressable Unbilled** | Charges on insurance cases that haven't been submitted as claims yet |

---

## Related

- [[Insurance Home & Claims#ERA Auto-Allocation|ERA Management (this note)]] — Detailed ERA workflow and configuration
- [[Insurance Home & Claims#EOB Processing|EOB Processing (this note)]] — Step-by-step EOB posting process
- [[Patient Records#Cases|Cases & Coverage]] — How insurance cases and coverage are set up on patient records
- [[Fee Schedules]] — How allowed amounts are determined per payer
- [[Transactions & Ledger]] — How payments post to the patient ledger
