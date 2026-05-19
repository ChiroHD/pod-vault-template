---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/admin/utilities
---

# System Settings & Configuration

> System-level settings that apply across all locations in a ChiroHD network. Accessible via **System Settings** in the top navigation bar from the Organization Home screen.

---

## Organization Home (`/app`)

The landing page after login. Displays all locations in the network as cards in a grid layout.

**Each location card shows:**
- Location name and legal name
- GreenSheets count (pending insurance claims)
- Date Live
- Email, Phone, State
- Location ID

**Top bar elements:**
- ChiroHD logo
- System-level nav: Home, Insurance, Reporting, System Settings, Leads
- Dashboard widgets toggle (Metrics, Records, Trends, Insurance BETA)
- User profile (name, email, Logout)
- Clock with current time/date

**Dashboard widgets (when enabled):**
- Total Income chart (bar chart by location)
- Date range selector with case type filter
- CSV export
- "Include Inactive Locations" toggle

**Actions available:**
- "Click here to add a new location" button
- "Show inactive" toggle (reveals hidden/inactive locations)
- Patient Search (global search across all locations)
- Click any location card to enter that location

---

## System Settings Menu

Accessible via the **System Settings** dropdown in the top nav. Contains 20 sub-pages:

| Sub-Page | Purpose |
|----------|---------|
| System Configuration | Global toggles and behavioral settings |
| Organization Info | Network name, address, contact info |
| Add ICD Dx Code | Add custom diagnosis codes to the system |
| Users | System-level user management, allowed domains |
| Inventory | Product catalog (supplements, DME, supplies) |
| Services | CPT-coded service catalog with pricing |
| Appointment Settings | Types, sub-types, schedule templates, wizard |
| SOAP Macros | Clinical note building blocks |
| Referral Sources | Patient acquisition tracking categories |
| Payment Types | Accepted payment methods |
| Email Templates | System-wide email templates with macros |
| Care Plan Templates | Pre-built care plan configurations |
| Task List Templates | Workflow checklists for patient and office tasks |
| Triggers (BETA) | Automated actions based on patient/appointment events |
| Patient Alert Templates | Configurable alerts for patient visit milestones |
| Patient Subjective Settings | Subjective note configuration by case/appointment type |
| Fee Schedules | Insurance and cash pricing schedules |
| Acknowledgements | Staff acknowledgement documents with versioning |
| Tags | Patient classification labels (system-wide) |
| Transaction Presets | Pre-configured service bundles for quick billing |
| Forms | Digital intake forms with drag-and-drop builder |
| Metric Groups | Location groupings for aggregate reporting |

---

## System Configuration

Controls global behavioral settings for the entire network.

### Options (Toggles)

| Setting | Description | Default |
|---------|-------------|---------|
| **Lead Tracking** | Enable the lead tracking pipeline for all locations | ON |
| **Org Controls Referrals** | System admin manages referral sources (locations can't add their own) | ON |
| **Referral Type Required** | Require a referral source when creating new patients | ON |
| **Locations Can Add Payers** | Allow location admins to add insurance payers (vs. system admin only) | ON |
| **Moderate Location Services** | Require system admin approval when locations request new services | ON |
| **Moderate Location Inventory** | Require system admin approval when locations request new inventory | ON |
| **Show Metrics on System Screen** | Display dashboard widgets on the Organization Home | ON |
| **Enable Patient Membership Options** | Enable membership/subscription billing features | OFF |
| **Allow Transient Patients** | Allow patients to be shared across locations (multi-location visits) | OFF |
| **Require System Admin to Create Users** | Prevent location admins from creating users | OFF |
| **Enable Allowed Domains** | Restrict user email domains to an approved list | ON |

### Configuration Fields

| Field | Description |
|-------|-------------|
| **Discrepancy Threshold** | Percentage threshold for flagging financial discrepancies (set to 0%) |
| **Days Before Pending Inactive** | Days before a pending patient is marked inactive (set to 30) |
| **Service Moderation Message** | Text shown when a location requests a new service (customizable) |
| **Inventory Moderation Message** | Text shown when a location requests new inventory (customizable) |

---

## Organization Info

| Field | Description |
|-------|-------------|
| Organization Name | The network/company name (e.g., "Demo Environment") |
| General Email | Primary contact email for the organization |
| Admin Email | Administrative contact email |
| Office Phone | Main phone number |
| Address | Physical address (street, city, state, zip) |

---

## Users (System Level)

System-level users have access across all locations. The Users page shows:

**Sections:**
- **System Users** — Users with organization-wide access
- **Allowed User Domains** — Email domains permitted for new user accounts (e.g., chirohd.com, sked.life, gmail.com)

**Actions:**
- Create New User
- Edit existing users
- Show inactive users toggle

**User Fields:**
- First Name, Last Name, Title
- Email, Cell Phone
- Provider Type (Chiropractor, Massage Therapist, Nutritionist, Stretch Therapist, Administrative Only)
- Permission Level (System Admin, Location Admin, Location User)
- Calendar Order (tab position)
- Feature Permissions (Text Reminders, Insurance, Patient App, Edit Schedule, Clear All)
- MFA status
- Location Access assignments

---

## Payment Types

Pre-configured payment methods accepted across the system:

| Payment Type | Include in Cash Reports |
|-------------|----------------------|
| ACH | Yes |
| BridgePay | Yes |
| Care Credit | Yes |
| Cash | Yes |
| Check | Yes |
| Fortis | Yes |
| HFA | Yes |
| Insurance - VCC | Yes |
| Insurance Check | Yes |
| Office Transfer | Yes |
| Payment on File | Yes |
| Square | Yes |
| Visa | Yes |
| Gift Certificate | No |
| Plan | No |

---

## Metric Groups

Allows grouping locations for aggregate reporting. Each group contains a set of locations and can be used to filter dashboards and reports.

**Actions:** Create New Metric Group, assign locations, delete groups.

---

## Acknowledgements

Staff acknowledgement documents with version control. Used for policy acknowledgements, compliance documents, and internal communications that require staff sign-off.

**Fields per acknowledgement:**
- Name
- Version number
- Dismissable (Yes/No)
- Signatory Users (All, Location-specific)
- Actions: Set Draft, Add Version

---

## Related

- [[Services & Inventory]] — Detailed service and inventory catalog
- [[Appointment Settings]] — Appointment types and scheduling configuration
- [[SOAP Notes#The Macro System|SOAP Macros]] — Macro builder and building blocks
- [[System Templates & Automation]] — Email, care plan, task list, alert templates, and triggers
- [[Fee Schedules]] — Insurance and cash pricing schedules
