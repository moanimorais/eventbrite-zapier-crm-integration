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
