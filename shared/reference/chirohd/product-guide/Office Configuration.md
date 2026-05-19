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

# Office Configuration

> Location-level feature toggles that control which features are enabled, which BETA features are active, and how the location behaves. Accessed via **Settings → Office → Office Configuration** inside a location.
>
> This is one of the most important screens in ChiroHD — it controls 35+ toggles that fundamentally change what users see and what the product can do at each location.

> [!info] Planned Changes (confirmed by Dr. C, April 2026)
> The Office Configuration screen is being fully revamped. ~80% of current toggles will be removed by end of 2026. Remaining toggles will become simple on/off "office options" (e.g., Calling Module, ERA auto-allocation). Goal: reduce UI clutter for practices that don't need certain features. *(Source: Dr. C product training, 2026-04-10)*

---

## Office Options

Core feature toggles that most locations will configure during setup.

| Toggle | Description | When ON |
|--------|-------------|---------|
| **Nutritionist** | Enable nutritionist provider type and related workflows | Locations with nutritionists on staff |
| **Massage Therapist** | Enable massage therapist provider type and appointment routing | Locations with LMTs on staff |
| **Stretch Therapist** | Enable stretch therapist provider type | Locations with stretch therapy services |
| **New Patient Emails** | Auto-send welcome email with intake paperwork link when a new patient is created | Most locations (use system email template) |
| **Use System Macros** | Use organization-level SOAP macros (vs. location-specific) | Default for most locations |
| **Show Patient Notes on SOAP Screen** | Display patient notes panel on the SOAP note entry screen | Useful for providers who want context during notes |
| **2-Way Patient Messaging** | Enable two-way text conversations with patients (requires Twilio) | Locations with Twilio integration |
| **Move appts with MX to end of queue** | Massage appointments go to the end of the arrival queue | Standard for practices with massage |
| **Enable auto-texting of leads** | Automatically send text messages to new leads | Locations with active lead tracking + Twilio |
| **Enable Calling Module** | Enable the patient routing / calling system with treatment areas | Locations using physical treatment areas |
| **Enable SOAP Assignment Options** | Allow multi-provider visit routing (assign SOAP to different provider than calendar) | Multi-provider locations |
| **Default Office Language** | Default language for patient-facing communications | English (default), Spanish available |

---

## BETA Options

Features in active development or limited testing. These may change or be removed in future releases.

| Toggle | Description | Notes |
|--------|-------------|-------|
| **Slide Outs BETA** | Enhanced side panel UI for patient details | Replaces modal popups with slide-out panels |
| **Zapier Integration BETA** | Enable Zapier webhook integration | Connects ChiroHD to external apps via Zapier |
| **Insurance V2 BETA** | Next-generation insurance processing interface | Major insurance workflow redesign |
| **Patient Statement BETA** | New patient statement generation | Updated statement layout and delivery |
| **Auto-Complete Appointments BETA** | Automatically mark appointments complete when SOAP is submitted + charges post | Eliminates manual completion step |
| **Notification Area** | Show notification/alert panel in the sidebar | Displays active alerts, conversations, and office alerts |
| **New Macro Layout BETA** | Redesigned SOAP macro interface | Updated visual layout for macro building blocks |

---

## ChiroHD Options

Advanced toggles typically managed by ChiroHD support or system admins. Many of these control feature flags for features in various stages of development.

| Toggle | Description | Notes |
|--------|-------------|-------|
| **Membership Clinic** | Enable membership-based billing model | For cash-only or hybrid practices using subscription plans |
| **Membership Clinic Hybrid** | Enable hybrid membership + insurance billing | Combines membership with traditional insurance billing |
| **Membership Charge at Checkin** | Auto-charge membership fee when patient checks in | Rather than monthly auto-billing |
| **Use Macro Services** | Link SOAP macros to specific services for auto-charge | Auto-posts charges based on which macros are used |
| **New Insurance Paradigm** | Redesigned insurance workflow (experimental) | Major insurance processing change |
| **New Copay Paradigm** | Updated copay handling logic | Changes how copays are calculated and applied |
| **Visit Tracking** | Enable visit count tracking per care plan | Tracks visits against care plan targets |
| **ERA BETA** | Enable Electronic Remittance Advice auto-processing | SFTP-based automated insurance payment posting |
| **Enable Go High Level Integration** | Connect to GoHighLevel marketing platform | External marketing automation |
| **Use Provider Calendar Mode by Default** | Default to the provider-centric calendar view | Changes the default calendar layout |
| **Reputation Management** | Enable review request and reputation tracking | Sends review requests to patients post-visit |
| **Enable Care Credit Integration** | Connect to CareCredit patient financing | External payment financing |
| **Enable Production Report** | Enable the production/provider performance report | Financial performance by provider |
| **Insurance V3 BETA** | Next iteration of insurance processing | Further insurance workflow refinement |
| **New Reporting Dashboards** | Enable the redesigned reporting dashboard set | Metrics, Records, Trends, and specialty dashboards |
| **Billing user on chd_user (TEMPORARY)** | Temporary toggle for billing user migration | Infrastructure migration — will be removed |
| **High Volume Calendar BETA** | Calendar optimized for high-volume practices | Handles more concurrent appointments per time slot |

---

## Configuration Impact Map

Understanding which toggles affect which features:

| Feature Area | Controlling Toggles |
|-------------|-------------------|
| **Calendar display** | High Volume Calendar, Provider Calendar Mode |
| **Patient flow** | Calling Module, SOAP Assignment Options, Auto-Complete |
| **SOAP notes** | Use System Macros, New Macro Layout, Show Patient Notes, Use Macro Services |
| **Insurance billing** | Insurance V2, Insurance V3, New Insurance Paradigm, New Copay Paradigm, ERA BETA |
| **Patient messaging** | 2-Way Messaging, Auto-texting of Leads |
| **Membership/subscriptions** | Membership Clinic, Membership Hybrid, Membership Charge at Checkin |
| **Reporting** | New Reporting Dashboards, Enable Production Report, Visit Tracking |
| **Integrations** | Zapier, Go High Level, Care Credit, Reputation Management |
| **Provider types** | Nutritionist, Massage Therapist, Stretch Therapist |

---

## Important Notes

- **Changes may require a hard refresh** (Cmd+Shift+R / Ctrl+Shift+R) to take effect — some toggles modify the Angular application's loaded modules
- **BETA toggles can be unstable** — features behind BETA flags may have known issues or incomplete implementations
- **Some toggles interact with each other** — e.g., ERA BETA requires Insurance V2 or V3 for full functionality; Membership Hybrid requires Membership Clinic to be ON
- **Toggle changes apply immediately** to the location — there is no preview or staged rollout at the toggle level

---

## Related

- [[System Settings & Configuration]] — System-level configuration that applies across all locations
- [[Calendar & Scheduling]] — Features controlled by calendar-related toggles
- [[SOAP Notes]] — Features controlled by SOAP-related toggles
- [[Insurance Home & Claims]] — Features controlled by insurance-related toggles
