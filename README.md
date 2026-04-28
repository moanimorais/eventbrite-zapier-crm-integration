# eventbrite-zapier-crm-integration

Zapier integration pipeline connecting Eventbrite registrations to GoHighLevel via LeadConnector. 25 Zaps deployed across three webinar series, each mapped to a unique Event ID with date-specific tags routing contacts into the downstream GHL automation workflow.

# Eventbrite → Zapier → GoHighLevel Integration

Automated contact capture pipeline connecting Eventbrite registrations to GoHighLevel (All In One Marketing) via Zapier. Built for [Live Life With Zest](https://www.livelifewithzest.com/the-zest-collective), a health coaching brand based in New York.

25 Zaps deployed across three webinar series — Menopause, Burnout, and Weight Loss — each event mapped to a dedicated Zap.

---

## Overview

The core problem: every Eventbrite registration needs to create a contact in GoHighLevel with the correct data and tags, so the downstream automation workflow fires correctly for that specific event date.

The solution: a master Zap built once, then duplicated and updated per event — changing only the Event ID, date-specific tag, and event name. Each Zap listens to one Eventbrite event and pushes the registrant's data directly into GHL via LeadConnector.

---

## Stack

| Tool | Role |
|---|---|
| Eventbrite | Event registration platform |
| Zapier | Integration layer — listens to Eventbrite and pushes to GHL |
| LeadConnector | GHL's native Zapier connector |
| GoHighLevel (All In One Marketing) | CRM — receives contact data and triggers downstream workflow |

---

## Architecture
Eventbrite Event (unique Event ID per date)
|
v
Zapier Trigger
New Attendee Registered

Organization: Live Life with Zest
Event Status: All
Event: [specific Event ID]
|
v
Zapier Action
LeadConnector — Add/Update Contact
Last Name:     pulled from Eventbrite
Phone:         pulled from Eventbrite
Email:         pulled from Eventbrite
Source:        pulled from Eventbrite (Event Name)
Tags:          series tag + date-specific tag + status tag
|
v
Contact created/updated in GoHighLevel
Tags trigger downstream workflow


---

## How It Works

### The Master Zap

A single Zap was built as the template. It contains two steps:

**Step 1 — Trigger (Eventbrite):**
- App: Eventbrite
- Event: New Attendee Registered
- Organization: Live Life with Zest
- Event ID: unique per Eventbrite listing

**Step 2 — Action (LeadConnector):**

| GHL Field | Eventbrite Source |
|---|---|
| Last Name | Profile Last Name |
| Phone Number | Profile Cell Phone |
| Email | Profile Email |
| Source | Event Name Text |
| Tags | Static values per Zap |

**Tag structure applied per Zap:**

| Tag | Purpose |
|---|---|
| Series tag (e.g. `Burnout for Healthcare Professionals`) | Identifies the webinar series |
| Date tag (e.g. `webinar_sep_12`) | Routes contact to correct branch in GHL workflow |
| `status_new_lead` | Marks contact status for CRM pipeline |

### Scaling to 25 Zaps

The master Zap was duplicated for each new event date. Per duplication, three values are updated:

1. **Event ID** — updated to the new Eventbrite listing
2. **Date-specific tag** — updated to match the new date (e.g. `webinar_oct_10`)
3. **Zap name** — updated to reflect the series and date for clarity

No structural changes required. The field mapping remains identical across all 25 Zaps.

---

## Webinar Series

| Series | Zaps Deployed |
|---|---|
| Menopause | Multiple dates |
| Burnout | Multiple dates |
| Weight Loss | Multiple dates |

---

## Technical Notes

### What Eventbrite exposes to Zapier

Zapier pulls only fields that Eventbrite makes natively available: first name, last name, email, cell phone, and event name. Custom or external data cannot be passed through this layer.

### Zoom credentials — known limitation

An earlier version attempted to inject Zoom access details directly via Zapier using a custom field in GoHighLevel (`zoom_access_details`). Testing revealed that Zapier cannot reliably write data to GHL custom fields when that data does not originate from an Eventbrite native field.

**Resolution:** Zoom credential injection was moved entirely to the GoHighLevel workflow layer, where it functions correctly via the Update Contact Field action inside each conditional branch. See the companion repository for full details.

---

## Downstream Integration

This pipeline feeds directly into the Multi-Date Webinar Delivery Pipeline built in GoHighLevel. The tags applied by Zapier are the trigger mechanism for that workflow.

Companion repository: [multi-date-webinar-pipeline](https://github.com/moanimorais/multi-date-webinar-pipeline)

---

## Documentation

- [Master Operations Manual](./docs/Master%20Operations%20Manual.pdf) — Full system documentation covering infrastructure setup, Zapier configuration, GHL workflow structure, golden rules, and checklist for new webinar dates

---

## Status

Production — 25 Zaps deployed and running across three webinar series.
