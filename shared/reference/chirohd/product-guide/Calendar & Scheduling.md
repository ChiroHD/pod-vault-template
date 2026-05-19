---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/calendar/scheduling
---

# Calendar & Scheduling

> The calendar is the central hub of daily operations in ChiroHD. It's the default landing page when entering any location and provides three distinct views for managing patient flow.

---

## Three Views

The calendar area provides three view modes, accessible via tabs on the left side of the screen:

### 1. Calendar View

The traditional appointment calendar grid.

**Layout:**
- **Provider tabs** across the top — one tab per provider with an enabled calendar, color-coded. Tab order is configurable per provider.
- **Time slots** in 15-minute increments running vertically (configurable per provider: 15, 30, or 60 min blocks)
- **Day navigation** with date picker and "Go To Today" button
- **Display modes:** 1 Day (default), with day-of-week headers for multi-day views

**Each appointment shows:**
- Patient name (first name + last initial)
- Appointment type
- Time
- Status color coding (see Status Workflow below)

**Actions from Calendar:**
- Click empty slot → New Appointment / Create Patient dialog
- Click existing appointment → Appointment details slide-out
- Right-click appointment → Context menu (status changes, reschedule, cancel)
- Provider gear icon (⚙️) → Provider Schedule settings

**Top bar controls:**
- **Create Patient** button
- **Events** button (blocking events, holidays)
- **Text Reminders** button (manual reminder sends)
- **"Showing All+"** filter (filter by appointment type, status)
- **Display** selector (1 Day, etc.)

> [!info] Display Option: Expand Overbooked Intervals (Source: Dr. C product training, 2026-04-10)
> "Expand Overbooked Intervals" is available in **Tab View only** — NOT in Provider View. Provider View requires uniform time intervals across all providers, and expanding one row would misalign time markers across columns.

### 2. Flow Screen

The patient queue view showing the status progression of today's appointments.

> [!info] Usage Note (Source: Dr. C product training, 2026-04-10)
> The Flow tab is "probably the least used feature" according to Dr. C, despite being "the way it should be run." Most practices default to the Calendar view even though Flow provides a superior real-time patient status overview.

**Primary Columns (left to right):**

| Column | Meaning | Visual |
|--------|---------|--------|
| **Scheduled** | Appointment booked but patient hasn't arrived | Default |
| **Arrived** | Patient has checked in (via front desk, check-in device, or text) | Blue card |
| **Assigned** | Patient routed to a treatment area (Table 1, Exam Room, etc.) | Area-specific columns |
| **Treated** | Provider has performed the adjustment/treatment | Provider-confirmed |
| **Draft** | SOAP note started but not submitted | Yellow |
| **View Only / Cannot Drag or Drop** | Locked state — informational | Gray |
| **Completed** | Visit complete (SOAP submitted, charges posted) | Green |

**Exception Rows:**

| Row | Meaning |
|-----|---------|
| **Missed** | Patient did not show; auto-marked after configurable timeout |
| **Cancelled** | Appointment was cancelled before the visit |
| **Rescheduled** | Appointment was moved to a different date/time |
| **Deleted** | Appointment was permanently removed |

**Each patient card shows:**
- Patient name
- Appointment type
- Time
- Elapsed wait time (minutes since arrival)
- Provider assignment

**Drag-and-drop:** Patients can be dragged between columns to update their status (except View Only/Cannot Drag columns).

### 3. Calling Module

Routes patients to physical treatment areas using audio announcements.

**Configuration panel:**
- **Calling Device** — which browser tab/device plays audio announcements
- **Update Calling Device** button
- **Enable Calling** toggle
- **Auto Queuing** toggle
- **Speech Engine** selector (Neural)
- **Speech Style** (Text)
- **Voice** selector (e.g., "Salli - Female")
- **Test audible** buttons (test on this device, test on calling device)

**Appointment Areas:**
- List of treatment areas (Table 1, Table 2, Exam Room, X-ray Room, etc.)
- Each area has an Enabled/Disabled toggle
- "Create area" button to add new treatment areas

**Mappings:**
- **Appointment Type → Appointment Area Mapping** — Auto-route specific appointment types to specific areas
- **Calendar → Appointment Area Mapping** — Route patients based on which provider they're seeing

---

## Appointment Creation

Clicking an empty time slot on the calendar opens the **New Appointment** dialog, which can also create a new patient.

**Top Row — Appointment Settings:**

| Field | Options |
|-------|---------|
| Case Type | Cash, Insurance, Personal Injury, Workers Comp |
| Case Name | Auto-fills from Case Type (editable) |
| Appointment Type | New Patient, Adjustment, Re-exam, Massage, etc. (from system appointment types) |
| Sub Type | Optional refinement of appointment type |

**Patient Info Row 1:**

| Field | Description |
|-------|-------------|
| First Name | Required |
| Last Name | Required |
| Nickname | Optional |
| Gender | Male / Female / Other |
| Birth Date | Date picker |

**Patient Info Row 2:**

| Field | Description |
|-------|-------------|
| Phone | Cell phone number |
| Text Notifications | Checkbox — opt in to text reminders |
| Email | Patient email address |
| New Patient Email | Checkbox — send welcome email on creation |
| Referral Source | Dropdown (from system referral sources) |
| Referring Patient | Search existing patients |
| Preferred Language | English (default), Spanish, etc. |

**Notes:**

| Field | Description |
|-------|-------------|
| Patient Notes | Persistent notes visible on the patient profile |
| Appointment Notes | Notes specific to this appointment |

**Actions:**
- **Schedule Appointment** (green button) — creates patient AND schedules the appointment
- **Create Patient Only** — creates the patient without scheduling

---

## Provider Schedule Settings

Accessed via the gear icon (⚙️) on a provider's calendar tab.

### Provider Schedule Tab

Configures the recurring weekly schedule:

| Setting | Description |
|---------|-------------|
| Start Date | When this schedule takes effect |
| Number of Columns | How many patients can be seen simultaneously (e.g., 3 for a DC, 1 for massage) |
| Minute Time Blocks | Grid resolution (15, 30, or 60 minutes) |
| Hour Intervals | Time label spacing on the left side (must match across all providers at a location) |

**Schedule grid:** Click and drag on each day of the week to define available hours. Morning and afternoon blocks can be set separately.

### Exception Hours Tab

Overrides the recurring schedule for specific dates:
- **Block time** — Mark a provider as unavailable (vacation, holiday, conference)
- **Add hours** — Extend availability on a specific date
- Date picker + time range selector

---

## Events

Blocking events that apply across the calendar (not specific to a provider):
- Holiday closures
- Staff meetings
- Training days

**Actions:** Create Event, set date range, set recurrence, mark which providers are affected.

---

## Text Reminders

Send appointment reminders to patients with upcoming appointments.

**Requires:** Twilio integration configured at the location level.

**Reminder templates** are configured under Settings → Office → Reminder Templates. 23 system default templates are available, including:
- Appointment Confirm (various scenarios)
- Missed Appointment
- Virtual Waiting Room (Wait / Ready)
- Family Message (Pre/Post appointments)
- Appointment Deny

---

## Status Workflow

The lifecycle of an appointment from scheduling through completion. *(Source: Dr. C product training, 2026-04-10)*

```
Scheduled → Arrived → Assigned → Treated → SOAP Complete → Completed
```

| Status | Trigger | What Happens |
|--------|---------|-------------|
| **Scheduled** | Appointment created | Appears on calendar, patient gets reminders |
| **Arrived** | Front desk marks arrived, check-in device, or text check-in | Patient appears in arrival queue |
| **Assigned** | Dragged to treatment area (Flow) or auto-assigned (Calling Module) | Patient routed to Table/Room |
| **Treated** | Provider performs adjustment/treatment | Patient has been seen by the provider |
| **SOAP Complete** | Provider submits SOAP note | Note is finalized and electronically signed |
| **Completed** | SOAP note submitted + charges posted | Visit is finalized, appointment is done |

**Exception States:**

| Status | Trigger | What Happens |
|--------|---------|-------------|
| **Missed** | Auto-marked after configurable timeout (default: 30 min) | Triggers missed appointment text (if Twilio enabled) |
| **Cancelled** | Front desk or patient cancels the appointment | Removed from active schedule; retained in history |
| **Rescheduled** | Appointment moved to a new date/time | Original slot freed; new appointment created |
| **Deleted** | Appointment permanently removed | No record retained on the calendar |

**Auto-Complete Appointments (BETA toggle):** When enabled, appointments automatically complete when SOAP is submitted and charges post. When disabled, front desk manually marks complete.

> [!info] Known Product Gap (Source: Dr. C product training, 2026-04-10)
> ChiroHD has no dedicated **Care Plan module** to track recommended visit frequency, services, and payment schedules across a full treatment plan. Dr. C has flagged this for potential development next quarter if engineering capacity allows.

---

## Known Issues & Upcoming Changes

*(Source: Dr. C product training, 2026-04-10)*

| Item | Type | Details |
|------|------|---------|
| **Overbooking dialog wording** | Bug | When scheduling into an already-booked slot, the dialog says "blocked off time slot" — should say "already scheduled." Confirmed by Dr. C. |
| **AI Coaching / Data Dashboard** | In Development | Scheduling efficiency recommendations driven by behavioral data insights. Will surface patterns and suggest optimizations for practices. |
| **User/Calendar Separation** | In Design (Josh Benner) | Will restructure how calendar settings are organized and surfaced. Many current calendar settings screens will change significantly. Decouples user accounts from calendar configuration. |

---

## Related

- [[Calendar & Scheduling#Three Views|Patient Queue & Routing (this note, Three Views section)]] — Detailed Flow screen and Calling Module documentation
- [[SOAP Notes]] — What happens when a provider opens a note from the queue
- [[Office Configuration]] — Toggles that affect calendar behavior (Calling Module, SOAP Assignment, Auto-Complete)
- [[Appointment Settings]] — System-level appointment type configuration
