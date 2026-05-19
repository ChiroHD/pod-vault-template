---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/setup/location
---

# Location Setup

> Every ChiroHD location has its own configuration, users, providers, and operational settings. This note documents the Settings menu structure inside a location, covering all sub-pages under **Settings** in the location-level top nav.

---

## Settings Menu Structure

Accessed via the **Settings** dropdown in the location top nav:

| Menu Item | Sub-items | Purpose |
|-----------|-----------|---------|
| **Users** | — | Location-level user management |
| **Providers** | — | Provider billing credentials (NPI, Tax ID) |
| **Appointment Settings** | Appointment Types, Sub Types | Location-level appointment configuration |
| **Inventory** | — | Location inventory (inherits from system, tracks on-hand counts) |
| **Services** | — | Location service catalog (inherits from system, location can request additions) |
| **Office** | Office Info, Office Configuration, Historical Billing | Location identity, toggles, billing history |
| **Initial Setup** | Treatment Areas, Tags, Integrations, Reminder Options, Scheduled Reminders, Reminder Templates, Email Notifications, Email Templates, Patient Alert Templates, SOAP Macros | Operational configuration |
| **System Settings** | — | Returns to system-level settings |

---

## Users

Two user categories at the location level:

### Calendar Users (Location-Level)

Users who have been assigned to this location and may have schedulable calendars.

| Field | Description |
|-------|-------------|
| Username | Display name |
| Email | Login email |
| Role | LOCATION_ADMIN or LOCATION_USER |
| Provider Type | Chiropractor, Massage Therapist, Nutritionist, Stretch Therapist, Administrative Only |
| Calendar | Enabled/Disabled with order number |
| Status | Active / Inactive |
| MFA | Multi-factor authentication status |

### Organization Users (Active Users)

Users with organization-wide access who can access this location (e.g., system admins, organizational staff).

### User Detail Fields

When editing a user:
- Title (Dr., Mr., Mrs., etc.)
- First Name, Last Name
- Email, Cell Phone
- Provider Type
- Permission Level
- Calendar Settings:
  - Enable Calendar toggle
  - Calendar Order (determines tab position)
  - Tab Color
- Feature Permissions:
  - Text Reminders
  - Insurance
  - Patient App
  - Edit Schedule
  - Clear All

---

## Providers

Provider records link calendar users to their billing identity. **Required for insurance billing.**

| Field | Description |
|-------|-------------|
| Name | Auto-populated from linked user |
| Title/Suffix | Dr., LMT, etc. |
| NPI Number | 10-digit National Provider Identifier (individual) |
| Tax ID | Practice or individual tax identification number |
| Tax ID Type | EIN (employer) or SSN (individual) |
| Taxonomy Code | Provider specialty code (e.g., 111N00000X for Chiropractic, 171400000X for Massage) |

---

## Office Info

Location identity and default settings. Accessed via Settings → Office → Office Info.

### Identity Fields

| Field | Description |
|-------|-------------|
| Location Name | Display name (appears on calendar, reports) |
| Short Name | Abbreviated name |
| Location Email | Contact email |
| Open Date | When the location went live |
| Is Test Location? | Flag for test/demo locations |
| Phone, Fax | Contact numbers |
| Timezone | Location timezone (DO NOT change without support) |
| Physical Address | Street, City, State, Zip |
| Billing Address | Can be "Same as Physical" or different |

### Default Assignments

**These three fields are critical — must be set before scheduling or billing works:**

| Field | Purpose |
|-------|---------|
| Default Owning User | The primary user associated with the location |
| Default Calendar Provider | The provider whose calendar opens by default |
| Default Billing Provider | The provider used as default on claims/billing |

### Additional Fields

| Field | Description |
|-------|-------------|
| Location Slug | URL path for the Web Scheduler |
| Website, Facebook, Twitter, YouTube, Instagram | Social media URLs (used in email template macros) |
| Marketing Company | External marketing company name |
| Location Logo | Uploadable image |
| Legal Name | Official legal entity name |
| Reports Show | Whether reports display the legal name |
| Group NPI (Type 2) | Location-level NPI (vs. individual provider NPIs) |
| NPI Number | Location NPI |
| NPI Taxonomy | Location-level taxonomy code |

### Tax Configuration

| Field | Description |
|-------|-------------|
| Tax Rate | Percentage (e.g., 7.25%) |
| Tax Name | Display name (e.g., "NC Sales Tax") |
| Applicable Categories | Which inventory categories are taxed |
| Mode | Normal or Pre-Tax |

---

## Initial Setup Sub-Pages

### Treatment Areas

Physical spaces patients move through during visits. Used by the Calling Module.

| Field | Description |
|-------|-------------|
| Area Name | Display name (e.g., "Table 1", "Exam Room") |
| Phonetical Name | Pronunciation for audio announcements (e.g., "Table One") |
| Order | Display order on the Flow screen |
| Enabled | Whether the area is active |

### Tags (Location-Level)

Patient classification labels specific to this location. Each tag has:
- Name, Color (hex code), Show in Queue toggle, Status (Active/Inactive)

### Integrations

Available integration categories (modal selector):

| Integration | Description |
|-------------|-------------|
| **Merchant Services** | Payment processing (Fortis, Square, etc.) |
| **Twilio** | SMS/voice communications (text reminders, 2-way messaging, virtual waiting room) |
| **Webhook / Zapier** | External automation connections |
| **X-Rays / Posture** | Diagnostic imaging integrations |
| **SKED** | Patient scheduling app integration |
| **Web** | Web Scheduler configuration |
| **Clearinghouse** | Electronic claims submission |
| **Stripe** | Payment processing alternative |

### Reminder Options

| Setting | Description |
|---------|-------------|
| Auto Mark Missed | Minutes before an appointment is auto-marked as missed (default: 30) |
| Missed Appointment Text | When to send missed appointment reminders |
| Virtual Waiting Room | Enable/disable virtual waiting room notifications |

### Scheduled Reminders

Configure automated text reminders sent before appointments. **Requires Twilio.**

### Reminder Templates

23 system default templates for various communication scenarios (appointment confirm, missed appointment, family messages, virtual waiting room, etc.). Each template has English and Spanish variants.

### Email Notifications

Configure email alerts for staff when specific events occur (e.g., appointment booked via Web Scheduler).

### Email Templates

Location-specific email templates (inherit from system, can create location-specific overrides).

### Patient Alert Templates

Location-level alert templates (inherit from system templates).

### SOAP Macros

Location-level SOAP macro management:
- **Use System Macros** — inherit macros from the system level (read-only at location)
- **Switch to Location Specific** — create location-specific macros that override system macros
- **Test Mode** — test macros without affecting patient records

---

## Historical Billing

Accessible via Settings → Office → Historical Billing.

Used for importing historical billing data from a prior system (e.g., during migration from Platinum or another EHR). Contains historical transaction records that predate the ChiroHD go-live date.

---

## Related

- [[Office Configuration]] — Feature toggles (separate from Office Info)
- [[System Settings & Configuration]] — System-level settings that locations inherit
- [[Calendar & Scheduling]] — How providers and hours are configured
- [[Williams Chiropractic — Test Location Audit]] — Example of a fully documented location setup
