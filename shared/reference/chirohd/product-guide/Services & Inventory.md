---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/billing/charges
---

# Services & Inventory

> Services are the billable items that get charged to patients (adjustments, exams, therapies, x-rays). Inventory items are physical products sold to patients (supplements, DME, supplies). Both are managed at the system level and inherited by locations.

---

## Services

### Access

System Settings → Services

### Service List

The service catalog is organized by CPT code groupings. Each service row shows:

| Column | Description |
|--------|-------------|
| Name | Display name (e.g., "Wellness Adjustment") |
| CPT Code | Standard billing code with optional modifiers (e.g., "98940", "98941 AT") |
| Billed Units | Default number of units per charge (usually 1) |
| Default Price | Cash price for this service |
| Taxable | Whether sales tax applies |
| Status | Active or Inactive |
| Category | Service classification for reporting |

### Service Categories

| Category | Description | Examples |
|----------|-------------|---------|
| Adjustment (Chiropractic) | Spinal and extremity adjustments | 98940, 98941, 98943 |
| Therapy (Rehab) | Therapeutic modalities and exercises | 97110, 97014, 97140 |
| Massage (Rehab) | Massage therapy services | 97124 |
| X-Ray (Chiropractic) | Diagnostic radiology | 72040, 72020, 72100 |
| Active (Rehab) | Active rehabilitation services | Decompression |
| DME (Rehab) | Durable medical equipment | Cervical traction, laser |
| Functional Medicine | Nutritional and functional medicine | Consultations |
| Service Misc (Other) | Uncategorized services | Block charges, misc fees |

### Key Services

| Service | CPT Code | Price | Charge Frequency |
|---------|----------|-------|-----------------|
| Wellness Adjustment | 98940 | $45.00 | Every Visit |
| Adj - 1-2 Regions | 98940 | $45.00 | Every Visit |
| Adj - 3-4 Regions | 98941 | $95.00 | Every Visit |
| Adj - Extra Spinal | 98943 | $60.00 | Every Visit |
| New Patient Exam | 99213 | $125.00 | Time of Purchase |
| Re-exam | 99213 | $50.00 | Time of Purchase |
| NP Brief | 99212 | $51.00 | Time of Purchase |
| NP Detailed | 99214 | $150.00 | Time of Purchase |
| Therapeutic Exercise | 97110 | $45.00 | Per session |
| E-Stim | 97014 | $28.00 | Per session |
| MX - 30 min | 97124 | $36.00 | Per session |
| MX - 1 Hour | 97124 (4 units) | $180.00 | Per session |
| Decompression | S9090 | $95.00 | Per session |
| Laser | — | $52.00 | Per session |

### EOB Practice Services

Services with "$0.00" price and "EOB Practice" in the name are used for insurance posting — they represent the payer-allowed amount for a service rather than the cash price. Example: "Adj 1-2 Regions - EOB Practice" at $0.00.

### Care Packages

Pre-paid visit bundles:

| Package | Price | Description |
|---------|-------|-------------|
| 32 Visit Care Package | $1,940.00 | Includes 98941 adjustments |
| 36 Visit CP | $1,500.00 | 36-visit care plan |
| 10 Adj Package | $190.00 | 10-adjustment package |

### Membership Plans

Recurring subscription services:

| Plan | Price | Frequency |
|------|-------|-----------|
| Membership Monthly Charge | $100.00 | Monthly |
| Monthly Fee (LC) | $350.00 | Monthly |
| Wellness Plan Family | $295.00 | Monthly |
| VIP Monthly Charge | $272.00 | Monthly |

### Actions

- **Create Service** — Add a new service to the catalog
- **Merge Services** — Combine duplicate services
- **System Defaults** — Reset to ChiroHD default service catalog
- **Pending Requests** — Review service requests from locations (when moderation is enabled)
- **Price Evaluation** — Evaluate pricing across services
- **Setup Wizard** — Guided service configuration

### Moderation Workflow

When "Moderate Location Services" is enabled in System Configuration:
1. Location admin requests a new service
2. Request appears in "Pending Requests"
3. System admin reviews and approves/rejects
4. Approved services become available at the requesting location

---

## Inventory

### Access

System Settings → Inventory

### Inventory List

Physical products available for sale to patients. Each item shows:

| Column | Description |
|--------|-------------|
| Name | Product name |
| Default Retail Price | Selling price |
| Taxable | Whether sales tax applies (most inventory is taxable) |
| Default Cost | Wholesale/acquisition cost |
| Category | Product classification |
| CPT Code | Optional billing code |
| UPC | Optional barcode |

### Inventory Categories

| Category | Examples |
|----------|---------|
| Supplement (Nutrition) | Catalyn, Cataplex B, Omega-3, Probiotics, D3, CBD |
| DME (Rehab) | Cervical Traction Device, Exercise Ball, Posture Pump |
| Inventory Misc (Other) | Face Paper, Shaker Bottles |
| Weight Loss (Nutrition) | Cleanse kits, weight loss supplements |

### Sample Inventory Items

80+ items in the default catalog including:
- **Supplements:** Catalyn 360T ($40), Cataplex B 360T ($42), D3 2000 ($13.70), Children's Omega DHA ($41.43), CBD Cream 2oz ($100)
- **DME:** Cervical Traction Device ($45, cost $20), Exercise Ball ($24.99), Chiroflow Pillow ($45.99)
- **Essential Oils:** doTERRA products ($5-$39)
- **Cleanse/Detox:** Clear Change Cleanse ($114.50), Detox Renewal Kit ($119)

### Actions

- **Create Inventory Item** — Add a new product
- **Merge Inventory Items** — Combine duplicates
- **CSV Import** — Bulk import inventory from spreadsheet
- **System Defaults** — Reset to default catalog
- **Pending Requests** — Review inventory requests from locations

### Tax Configuration

Taxes on inventory are configured at the location level (Settings → Office → Office Info):
- Tax rate (e.g., 7.25%)
- Tax name (e.g., "NC Sales Tax")
- Applicable categories (Supplement, DME, Weight Loss, Inventory Misc, Lab Test)
- Mode: Normal or Pre-Tax

---

## Related

- [[Transactions & Ledger]] — How services and inventory charges appear on the patient ledger
- [[Fee Schedules]] — How service prices are overridden for specific payers or cash plans
- [[System Settings & Configuration]] — Service and inventory moderation settings
- [[SOAP Notes]] — How "Every Visit" services auto-post when SOAP notes are submitted
