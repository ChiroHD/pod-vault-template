---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/integrations
---

# Integrations Overview

> ChiroHD supports integrations with external services for payment processing, communication, claims submission, scheduling, and marketing. Integrations are configured at the location level via Settings → Initial Setup → Integrations.

---

## Available Integrations

| Integration | Category | Purpose | Required For |
|-------------|----------|---------|-------------|
| **Fortis** | Merchant Services | Credit card payment processing via terminal or online | Processing card payments, storing cards on file |
| **Square** | Merchant Services | Alternative payment processing | Card payments (alternative to Fortis) |
| **Stripe** | Payment | Online/recurring payment processing | Monthly membership charges, recurring payments |
| **Twilio** | Communication | SMS/voice services | Text reminders, 2-way messaging, virtual waiting room, mass notifications, check-in device text |
| **Clearinghouse** | Insurance | Electronic claims submission and ERA receipt | Submitting insurance claims electronically, receiving ERA files via SFTP |
| **SKED** | Scheduling App | Patient-facing scheduling and engagement app | Online booking, patient app features, appointment confirmation |
| **Web Scheduler** | Web | Embeddable online scheduling widget | Patient self-scheduling from website |
| **Zapier** | Webhook | Connect ChiroHD to 5,000+ external apps | Custom automations, CRM sync, marketing automation |
| **X-Rays / Posture** | Diagnostic | Diagnostic imaging integration | Importing X-ray images, posture analysis |
| **GoHighLevel** | Marketing | Marketing automation platform | Lead nurturing, marketing campaigns (requires toggle) |
| **CareCredit** | Financing | Patient financing platform | Patient payment plans (requires toggle) |

---

## Merchant Services (Fortis)

The primary payment processing integration.

**Capabilities:**
- Process credit/debit card payments at point of sale
- Store cards on file for recurring charges
- Process refunds
- Terminal integration (physical card reader)
- Online/keyed-in payments

**Configuration:** Select "Fortis" from the Merchant Services integration, enter API credentials provided by Fortis.

---

## Twilio

Powers all SMS and voice communication features.

**Capabilities:**
- **Appointment Reminders** — automated text reminders before appointments
- **Two-Way Messaging** — real-time text conversations between practice and patients (requires toggle)
- **Virtual Waiting Room** — notify patients when it's time to enter the office
- **Mass Notifications** — bulk text to patient groups
- **Check-In Device** — patient text-based check-in
- **Missed Appointment Texts** — automated follow-up after missed appointments
- **Lead Auto-Texting** — automated text sequences for new leads

**Configuration:** Requires Twilio account credentials (Account SID, Auth Token) and a purchased Twilio phone number assigned to the location.

**Without Twilio:** Text-based features are non-functional but can be pre-configured. Voice calling module still works (uses browser audio, not Twilio).

---

## Clearinghouse

Electronic claims submission and ERA (Electronic Remittance Advice) receipt.

**Capabilities:**
- Submit GreenSheets (CMS-1500 claims) electronically to insurance payers
- Receive ERA files (X12 835 format) via SFTP for automated payment posting
- Claim status tracking
- Rejection/denial notifications

**Supported Clearinghouses:** Configuration allows selection of the clearinghouse provider. ChiroHD routes claims through the configured clearinghouse, which then forwards to individual payers.

---

## SKED Integration

Connects ChiroHD to the SKED patient app.

**Capabilities:**
- Patient-facing appointment scheduling
- Appointment confirmations and reminders via the SKED app
- Two-way sync between ChiroHD calendar and SKED availability
- Patient engagement features (push notifications, recall reminders)

**Note:** SKED is a separate product in the ADIO portfolio (alongside ChiroHD). The integration connects the two products for practices that use both.

---

## Web Scheduler

An embeddable scheduling widget for practice websites.

**Configuration:**
- Location Slug (URL path)
- Appointment Type restrictions
- Provider/Calendar restrictions
- Allow same-day appointments toggle
- Collect patient address toggle
- Redirect URL after booking

**Output:** A public URL that patients can use to self-schedule (e.g., `chirohd.com/schedule/williams-chiropractic`).

**Notifications:** When an appointment is booked via Web Scheduler, the location receives an email notification (configured under Email Notifications).

---

## Zapier / Webhook

Connects ChiroHD to external applications via webhooks.

**Capabilities:**
- Trigger external actions when ChiroHD events occur (new patient, appointment booked, payment received)
- Receive data from external systems
- Connect to CRM, marketing, accounting, and other tools

**Requires:** Zapier Integration BETA toggle enabled in Office Configuration.

---

## Integration Status by Feature Dependency

| Feature | Works Without Integration | Required Integration |
|---------|--------------------------|---------------------|
| Calendar/Scheduling | Yes | None |
| SOAP Notes | Yes | None |
| Patient Management | Yes | None |
| Cash Billing | Yes (manual entry) | Fortis/Square for card processing |
| Insurance Claims | Partially (paper claims) | Clearinghouse for electronic submission |
| ERA Processing | No | Clearinghouse + ERA BETA toggle |
| Text Reminders | No | Twilio |
| 2-Way Messaging | No | Twilio + toggle |
| Online Scheduling | No | Web Scheduler or SKED |
| Patient App | No | SKED |
| Recurring Payments | Partially | Stripe or Fortis |
| Marketing Automation | No | Zapier or GoHighLevel |
| X-Ray Import | No | X-Ray/Posture integration |

---

## Related

- [[Office Configuration]] — Toggles that enable/disable integration features
- [[Insurance Home & Claims]] — Claims submission via clearinghouse
- [[Calendar & Scheduling]] — Text reminders and Web Scheduler scheduling
- [[Location Setup]] — Integration configuration under Initial Setup
