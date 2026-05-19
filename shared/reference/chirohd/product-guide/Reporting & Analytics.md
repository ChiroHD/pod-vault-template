---
type: reference
date: 2026-04-02
author: "[[Jeff Williams]]"
source: claude
tags:
  - type/reference
  - source/claude
  - topic/product
  - feature/reporting/dashboards
---

# Reporting & Analytics

> ChiroHD provides multiple reporting interfaces at both the system level and location level. The reporting system is undergoing a significant redesign, with several BETA dashboards alongside legacy report generators.

---

## Access

### System Level (Org Home)

The dashboard widgets on the Org Home provide cross-location metrics:
- **Metrics** tab — Total Income by location (bar chart)
- **Records** tab — Record-level data views
- **Trends** tab — Trend-over-time visualizations
- **Insurance BETA** tab — Insurance-specific metrics

Controls: Date range, Case Type filter, CSV export, Include Inactive Locations toggle.

### Location Level (Reporting Menu)

Accessed via the **Reporting** dropdown in the location top nav:

| Menu Item | Description |
|-----------|-------------|
| **Reporting Dashboards** | Visual dashboards with multiple tabs (new) |
| **All Reports** | Traditional report generator with category/format selection |
| **All Reports (BETA)** | Redesigned report generator |
| **Simple Reporting** | Quick single-metric reports |
| **Recurring Payments (BETA)** | Track and manage recurring payment plans |
| **Patient List** | Full patient directory with filters and export |
| **Advanced Patient Search** | Multi-criteria patient search with complex filtering |
| **Lead Tracking** | Lead pipeline and conversion tracking |

---

## Reporting Dashboards

The primary visual reporting interface (requires "New Reporting Dashboards" toggle ON in Office Configuration).

### Dashboard Tabs

| Tab | Category | Status |
|-----|----------|--------|
| **Metrics** | Core business metrics (income, visits, collections) | GA |
| **Records** | Record-level data exploration | GA |
| **Trends** | Time-series trend analysis | GA |
| **Insurance BETA** | Insurance-specific dashboards (co-pay/co-insurance breakdown) | BETA |
| **Appointments BETA** | Appointment volume and scheduling analytics | BETA |
| **Financial BETA** | Financial performance dashboards | BETA |
| **Referral Tree BETA** | Referral source analysis and patient acquisition tracking | BETA |
| **Benchmark BETA** | Performance benchmarking across locations | BETA |

### Dashboard Controls

- **Date range** selector (from/to date pickers)
- **Case Type** filter (All, Cash, Insurance, PI, etc.)
- **Metric selector** (Total Income, Visit Count, Collections, etc.)
- **Aggregation** (Sum, Average, Count)
- **Visualization** (Trend Over Time, Bar Chart, etc.)
- **CSV export** button
- **Show Widgets** toggle

---

## All Reports

The traditional report generator. Select a category and format to generate reports.

**Report Categories:**
- Cash
- Insurance
- Collections
- Production
- Patient
- Scheduling
- Provider
- Financial Summary
- Custom

**Report Formats:**
- PDF
- CSV
- On-screen

**Common Reports:**

| Report | Category | What It Shows |
|--------|----------|---------------|
| Daily Income Summary | Cash | Total collections by payment type for a date range |
| Provider Production | Production | Revenue generated per provider |
| Aging Report | Insurance | Outstanding insurance receivables by aging bucket |
| Patient Visit Summary | Patient | Visit counts per patient over a date range |
| Appointment Report | Scheduling | Appointment volume, no-shows, cancellations |
| GreenSheet Report | Insurance | Claims submitted, status, amounts |
| Transaction Detail | Financial | Line-item transaction history |

---

## Simple Reporting

Quick, single-metric reports with minimal configuration:
- Select a metric
- Select a date range
- Generate

Useful for quick lookups without the complexity of the full report generator.

---

## Advanced Patient Search

Multi-criteria patient search and filtering tool. Available via Reporting → Advanced Patient Search.

**Filter criteria include:**
- Name, email, phone
- Status (Active, Inactive, Terminated)
- Tag (patient tags like VIP, Cash Patient, Insurance Patient)
- Last visit date range
- Provider
- Appointment type
- Case type
- Insurance payer
- Referral source
- Location (for multi-location searches)
- Custom fields

**Actions on results:**
- Export to CSV
- Send mass notification
- Apply tags
- View individual patient profiles

---

## Patient List

The full patient directory for the location:

**Columns:**
- First Name, Last Name
- Email
- Cell Phone, Home Phone, Work Phone
- Status (Active, Inactive, Terminated)

**Controls:**
- Order by: Last Name, First Name, Status
- Filter by status: Active, Inactive, Terminated, All
- CSV export
- Pagination (for large patient lists)

---

## Lead Tracking

Pipeline view for tracking patient acquisition:

**Pipeline Stages (configurable):**
1. New Lead Received
2. Call 1, Day Of
3. Call 2, Day 2
4. Call 3, Day 5
5. Email
6. Last Call - Free 30 Min Massage
7. Scheduled

**Per-lead data:**
- Name, phone, email
- Source (referral source)
- Stage
- Date entered
- Notes
- Actions (call, email, schedule, advance stage)

---

## Recurring Payments (BETA)

Track and manage patients on recurring payment plans:
- Monthly membership charges
- Care plan installment payments
- Subscription billing

**Requires:** Membership Clinic toggle enabled in Office Configuration.

---

## Related

- [[Office Configuration]] — Toggle: New Reporting Dashboards, Enable Production Report
- [[System Settings & Configuration]] — System-level dashboard widgets on Org Home
- [[Insurance Home & Claims]] — Insurance-specific reporting (AR aging, claim status)
- [[Patient Records]] — Patient list and search functionality
