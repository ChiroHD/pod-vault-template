---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/patient/profile
---

# Patient Records

> Patient records are the central data object in ChiroHD. Every scheduling, clinical, and billing workflow connects back to the patient record. This note covers the patient list, patient profile, and patient creation workflow.

---

## Patient List

### Access
Reporting → Patient List (location level) or `/app/patient-list`

### List View

| Column | Description |
|--------|-------------|
| First Name | Patient first name |
| Last Name | Patient last name |
| Email | Contact email |
| Cell Phone | Mobile number |
| Home Phone | Home number |
| Work Phone | Work number |
| Status | Active, Inactive, or Terminated |

### Controls
- **Order By:** Last Name, First Name, or Status
- **Status filter:** Active, Inactive, Terminated, or All
- **CSV export** button
- **Pagination** for large lists

---

## Patient Profile

The comprehensive record for an individual patient. Accessed by clicking a patient name from the patient list, calendar, or search.

### Profile Header
- Patient name, photo (if uploaded)
- Tags (color-coded labels like VIP, Cash Patient, Insurance Patient)
- Status indicator (Active/Inactive/Terminated)
- Quick action buttons

### Profile Tabs

| Tab | Content |
|-----|---------|
| **Demographics** | Personal info, contact details, emergency contact, employer |
| **Cases** | Billing cases (Cash, Insurance, PI, WC), each with DX codes, services, coverage |
| **Appointments** | Appointment history and upcoming appointments |
| **Transactions** | Ledger view — all charges, payments, adjustments, balances |
| **Documents** | Uploaded files, intake form submissions, X-rays |
| **Forms** | Digital paperwork status and submissions |
| **Notes** | Patient notes (persistent notes visible across visits) |
| **Alerts** | Active alerts and alert history |
| **Tasks** | Task list assignments and completion status |
| **Messaging** | Text conversation history (requires Twilio) |

---

## Demographics

| Field Group | Fields |
|-------------|--------|
| **Identity** | First Name, Last Name, Nickname, Gender, Birth Date, SSN (last 4), Preferred Language |
| **Contact** | Cell Phone, Home Phone, Work Phone, Email, Text Notification opt-in |
| **Address** | Street, City, State, Zip |
| **Emergency Contact** | Name, Phone, Relationship |
| **Employer** | Employer name, Phone, Address |
| **Referral** | Referral Source, Referring Patient, Referring Provider |
| **Insurance ID** | External patient identifiers |

---

## Creating a Patient

Two methods:

### 1. From Calendar (Appointment + Patient)
Click an empty time slot → New Appointment dialog creates patient AND schedules in one step. See [[Calendar & Scheduling]] for full field list.

### 2. From Create Patient Button
Click "Create Patient" in the calendar top bar → Patient creation form without scheduling.

**Required fields:** First Name, Last Name
**Recommended fields:** Phone, Email, Gender, Birth Date, Referral Source

---

## Cases

Each patient has one or more billing cases. Cases are the container for all billing activity.

### Case Types

| Type | Purpose | Key Differences |
|------|---------|----------------|
| **Cash** | Self-pay patients | No insurance, direct patient billing |
| **Insurance** | Insurance-covered visits | Requires payer, coverage, DX codes; generates GreenSheets |
| **Personal Injury (PI)** | Injury/accident cases | Attorney/lien-based billing, different AR tracking |
| **Workers Compensation** | Workplace injury cases | Employer/WC carrier billing |

### Case Components

Each case contains:

| Component | Description |
|-----------|-------------|
| **Treatment / DX** | Diagnosis codes (ICD-10) associated with this case |
| **Defaults / Patient Rates / Recurring Charges** | Default services with pricing, charge frequency, and write-off rules |
| **Coverage** (Insurance cases) | Insurance payer, policy details, benefits, copay/coinsurance |
| **Fee Schedule** | Applied fee schedule that overrides default service pricing |

### Default Services

Services added to a case's Defaults tab auto-populate for billing:

| Charge Frequency | Behavior |
|-----------------|----------|
| **Every Visit** | Auto-posts when SOAP note is submitted |
| **Time of Purchase** | Appears as reference; must be manually entered |
| **Monthly** | Auto-charges on a recurring monthly basis |
| **Weekly** | Auto-charges weekly |

Each default service row shows: Service Name, Charge Amount, Write Off Amount, Write Off Reason, Units, DX Code Mapping, Modifiers, Charge Frequency.

---

## Insurance Coverage

On an Insurance case, the Coverage section captures:

### Payer & Dates

| Field | CMS-1500 Box | Description |
|-------|-------------|-------------|
| Insurance Company | — | Select from payer directory |
| Nickname | — | Auto-fills from payer |
| Priority | — | Primary, Secondary, Tertiary |
| Coverage Start Date | — | When coverage begins |
| Renews On | — | Annual renewal date |
| Coverage End Date | — | When coverage expires |

### Identifiers

| Field | CMS-1500 Box | Description |
|-------|-------------|-------------|
| ID / Claim Number | Box 1a | Patient's insurance ID |
| Group Identifier | Box 11 | Group number |
| Plan Identifier | Box 11c | Plan code |
| Prior Auth Number | Box 23 | Prior authorization if required |

### Benefits

| Field | Description |
|-------|-------------|
| Relationship to Insured | Self, Spouse, Child, Other (Box 6) |
| Remaining Deductible | Manually tracked deductible balance |
| Copay Visits | Number of visits with copay (blank = no limit; 0 = full patient responsibility) |
| Copay $ Amount | Per-visit copay amount |
| Coinsurance % | Patient's percentage responsibility after deductible |
| Max Visits | Visit limit (blank = no limit; 0 = no visits allowed) |

### Claim Settings

| Field | CMS-1500 Box | Description |
|-------|-------------|-------------|
| Date of Current Illness | Box 14 | Onset date |
| Onset Qualifier | — | Qualifier code |
| Box 19 | Box 19 | Additional claim info |
| Accept Assignment | Box 27 | Provider accepts allowed amount |
| Referring Provider | Box 17 | Referring physician |

---

## Patient Alerts

Alerts are configurable notifications that display on a patient's record at key milestones.

**Display locations:**
- Banner on patient profile
- Sidebar notification panel (when Notification Area is enabled)
- Pop-up when opening the patient record
- Flow screen card annotations

**Alert actions:**
- Close (dismiss without completing)
- Close and open patient snapshot
- Mark alert as complete

---

## Patient Tags

Color-coded labels for patient classification:
- Visible in the arrival queue and patient list
- Filterable in Advanced Patient Search
- Can trigger automations via Triggers (BETA)
- System-level tags (shared) + location-level tags (location-specific)

---

## Related

- [[Calendar & Scheduling]] — Appointment creation and patient scheduling
- [[SOAP Notes]] — Clinical documentation linked to patient records
- [[Insurance Home & Claims]] — Claims and billing from insurance cases
- [[Reporting & Analytics]] — Patient analytics and Advanced Patient Search
- [[System Templates & Automation]] — Alerts, task lists, and triggers on patient records
