# ⚡ Watt — AI Energy Intelligence Agent

An autonomous n8n agent that fetches live UK energy tariff data from 
the Octopus Energy API, processes it through multi-stage filtering and 
normalisation logic, and delivers a Claude-generated intelligence report 
directly to Discord — on a schedule, without manual intervention.

---

## What It Does

Watt runs automatically and performs the following in sequence:

1. **Fetches** all available Octopus Energy and Co-op Energy products 
   via the public API (27 products)
2. **Splits** the product list into individual records for processing
3. **Filters** down to IMPORT tariffs only (15 relevant tariffs)
4. **Retrieves** unit rates for each tariff via individual HTTP requests
5. **Normalises** all rate data to the North West England region (`_H`)
6. **Classifies** each tariff by type (standard, dynamic, EV, heat pump)
7. **Merges** live tariff data with Ofgem price cap benchmark (Big 6 SVT reference)
8. **Aggregates** all tariffs into a single structured payload
9. **Generates** a 1,000-word intelligence report using Claude AI
10. **Delivers** the final report to a Discord channel via webhook

---

## Workflow Architecture

---

## Workflow Architecture

```
Scheduled Trigger
      │
      ▼
Octopus Energy API ──► 27 products
      │
      ▼
Split Out ──► individual product records
      │
      ▼
Filter ──► 15 IMPORT tariffs
      │
      ▼
HTTP Request ──► unit rates per tariff
      │
      ▼
Code Node ──► normalise to NW region
      │
      ▼
Aggregate ──► bundle all 15 tariffs
      │
      ▼
Claude AI ──► 1,000-word tariff report
      │
      ▼
Discord ──► delivered to your channel
```
---

## Tariff Types Covered

| Type | Example | Who it suits |
|------|---------|--------------|
| Standard variable | Flexible Octopus, Co-op Flexible | Most households |
| Fixed rate | Co-op 10M Fixed, Cosy 12M Fixed | Stability seekers |
| Dynamic pricing | Agile Octopus | Flexible households |
| EV tariff | Octopus Go, Intelligent Octopus Go | EV owners |
| Heat pump | Cosy Octopus | Heat pump owners |
| Price cap benchmark | Ofgem SVT (Big 6 reference) | Comparison baseline |

---

## Data Sources

| Source | Method | Auth |
|--------|--------|------|
| Octopus Energy API | REST — `v1/products/` | None (public) |
| Ofgem price cap | Hardcoded quarterly | None |

> Ofgem cap rates are updated manually each quarter.  
> Next update: **27 May 2026** (for July–September 2026 cap)

---

## Tech Stack

| Component | Tool |
|-----------|------|
| Automation platform | n8n v2.14.2 (self-hosted, Windows) |
| Energy data | Octopus Energy REST API (public) |
| AI report generation | Anthropic Claude Sonnet (`claude-sonnet-4-20250514`) |
| Report delivery | Discord webhook |

---

## Setup

### Prerequisites
- n8n installed (native Windows or Docker)
- Anthropic API key
- Discord webhook URL

### Installation

1. Clone this repo
2. Import `Watt Energy Data Report.json` into n8n via  
   **Settings → Import workflow**
3. Add credentials:
   - Anthropic API key in the Claude node
   - Discord webhook URL in the final HTTP Request node
4. Activate the workflow

### Ofgem Cap Update Schedule

| Quarter | Period | Announcement |
|---------|--------|-------------|
| Q2 2026 | Apr–Jun | 25 Feb 2026 ✓ |
| Q3 2026 | Jul–Sep | 27 May 2026 |
| Q4 2026 | Oct–Dec | 26 Aug 2026 |
| Q1 2027 | Jan–Mar | 25 Nov 2026 |

---

## Changelog

### v2.0 — April 2026
- Added tariff type classification (dynamic, EV, heat pump, standard, fixed)
- Added Ofgem price cap benchmark as Big 6 SVT reference point
- Extended coverage to all 15 IMPORT tariffs including Agile, Go, 
  Intelligent Go and Cosy Octopus
- Improved Claude prompt with explicit data-driven analysis instructions
- Switched report delivery to Discord webhook

### v1.0 — Initial release
- Octopus Agile tariff tracking
- Claude AI report generation
- Discord delivery

---

## Roadmap

### Phase 1 — Core pipeline (complete)
- [x] Octopus Energy API — all 27 products
- [x] Filter to 15 IMPORT tariffs, North West region
- [x] Tariff classification (Agile, EV, heat pump, fixed, standard)
- [x] Ofgem price cap benchmark (Q2 2026)
- [x] Claude Sonnet 1000-word report
- [x] Discord webhook delivery
- [x] Weekly cron trigger

### Phase 2 — Data persistence (next)
- [ ] SQLite storage — weekly tariff snapshots with timestamps
- [ ] Historical trend context passed to Claude prompt
- [ ] Quarterly Ofgem benchmark updates

### Phase 3 — Expanded market coverage
- [ ] Big 6 SVT rates as structured data node
- [ ] Which? customer satisfaction scores in Claude context

### Phase 4 — Raspberry Pi 5 deployment
- [ ] Migrate n8n to Pi 5 via Docker alongside Home Assistant
- [ ] Persistent SQLite volume mount

### Phase 5 — Intelligence upgrades
- [ ] Personal usage inputs for exact cost calculation
- [ ] Agile half-hourly price forecast in report
- [ ] Price alert Discord ping below threshold
- [ ] Switching recommendation with annual saving calculation
- [ ] Dockerised full stack published to GitHub

---


## Author

**Christopher Ho**
[github.com/christopherwyk](https://github.com/christopherwyk) · [christopherwyk@gmail.com](mailto:christopherwyk@gmail.com)
