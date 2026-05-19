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

# Utilities & Tools

> The Utilities menu provides access to specialized tools for patient management, insurance operations, and communication features that don't fit neatly into the core Calendar/SOAP/Insurance workflow.

---

## Utilities Menu

Accessed via the **Utilities** dropdown in the location-level top nav:

| Utility | Purpose |
|---------|---------|
| **Documents / X-Rays** | Manage patient documents, images, and diagnostic imaging files |
| **Insurance Eligibility (BETA)** | Verify patient insurance eligibility in real-time |
| **Intake Link** | Generate and manage the patient-facing intake form URL |
| **Mass Notification** | Send bulk text/email communications to patient groups |
| **Patient Paperwork** | View and manage digital intake form submissions |
| **Rebill Utility (BETA)** | Re-generate and re-submit insurance claims in bulk |
| **Insurance Renewal** | Track and manage insurance coverage renewals |
| **Merge Patients** | Combine duplicate patient records |
| **Monthly Payments** | Manage recurring payment plans and subscriptions |

---

## Documents / X-Rays

Central document repository for patient-attached files:
- Upload and categorize documents (PDF, images)
- Link to specific patients and cases
- Attach X-ray images and diagnostic imaging
- Integration point for X-ray/Posture systems (if configured)

---

## Insurance Eligibility (BETA)

Real-time insurance eligibility verification:
- Select a patient and their insurance case
- Query the payer for current coverage status
- Returns: eligibility status, effective dates, copay amounts, deductible status, benefits remaining
- Requires clearinghouse integration

---

## Intake Link

Generate the URL for the patient-facing digital intake form landing page:
- Displays the location's published intake forms
- Patients complete paperwork online before their appointment
- Submissions appear in Patient Paperwork for staff review
- The link includes the location slug (e.g., `/intake/williams-chiropractic`)

---

## Mass Notification

Send bulk communications to groups of patients:
- Filter recipients by: tag, status, last visit date, provider, appointment type
- Choose communication type: Text (requires Twilio) or Email
- Select or compose message
- Preview recipient list before sending
- Track delivery status

---

## Patient Paperwork

View and process digital intake form submissions:
- List of submitted forms with patient name, date, and status
- Review individual submissions
- Approve and link to patient records
- Flag incomplete submissions

---

## Rebill Utility (BETA)

Bulk claim resubmission tool:
- Select claims that need to be rebilled (denied, incorrect, updated)
- Regenerate GreenSheets with corrected information
- Resubmit in batch to the clearinghouse
- Track resubmission status

---

## Insurance Renewal

Track insurance coverage that is approaching or past its renewal date:
- Lists patients with coverage nearing expiration
- Allows updating coverage dates and benefits
- Triggers alerts for patients who need to provide updated insurance cards

---

## Merge Patients

Combine duplicate patient records:
- Search for patients to merge
- Select the "winning" record (primary)
- Review what will be merged (appointments, notes, charges, documents)
- Execute merge — transfers all data from the duplicate to the primary record
- The duplicate is deactivated

**Warning:** Merging is irreversible. Verify the correct records before executing.

---

## Monthly Payments

Manage patients on recurring payment plans:
- View active monthly payment arrangements
- Track payment history and upcoming charges
- Process failed payments
- Requires Merchant Services or Stripe integration

Related to Membership Clinic features (requires toggle in Office Configuration).

---

## Related

- [[Insurance Home & Claims]] — Full insurance workflow (claims, EOBs, ERA)
- [[Patient Records]] — Patient profile and document management
- [[Office Configuration]] — Toggles that enable/disable utility features
- [[Integrations Overview]] — Required integrations for eligibility, texting, and payments
