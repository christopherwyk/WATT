# ⚡ Watt — AI Energy Intelligence Agent

An autonomous n8n agent that fetches live UK energy tariff data from the Octopus Energy API, processes it through multi-stage filtering and normalisation logic, and delivers a Claude-generated intelligence report directly to Discord — on a schedule, without manual intervention.

---

## What It Does

Watt runs automatically and performs the following in sequence:

1. **Fetches** all available Octopus Energy products via the public API (27 products)
2. **Splits** the product list into individual records for processing
3. **Filters** down to IMPORT tariffs only (15 relevant tariffs)
4. **Retrieves** unit rates for each tariff via individual HTTP requests
5. **Normalises** all rate data to the North West England region
6. **Aggregates** all 15 tariff datasets into a single structured payload
7. **Generates** a 1,000-word intelligence report using Claude AI (Anthropic)
8. **Delivers** the final report to a Discord channel via webhook

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

## Tech Stack

| Component | Tool |
|---|---|
| Automation platform | n8n |
| Energy data source | Octopus Energy REST API (public) |
| AI report generation | Anthropic Claude API |
| Delivery channel | Discord Webhook |
| Language (Code node) | JavaScript |

---

## Setup & Configuration

### Prerequisites

- n8n instance (local via npm/Docker, or n8n Cloud)
- Anthropic API key — [console.anthropic.com](https://console.anthropic.com)
- Discord server with a webhook URL — Server Settings → Integrations → Webhooks
- Octopus Energy API access (public, no key required for product listing)

### Credentials to configure in n8n

| Credential | Where to set it |
|---|---|
| `Anthropic API Key` | n8n Credentials → Anthropic API |
| `Discord Webhook URL` | n8n Credentials → Discord Webhook or HTTP Header |

> ⚠️ Never commit real API keys or webhook URLs to this repository. Configure all credentials inside n8n's built-in credential manager.

### Import the workflow

1. Clone this repository
2. Open your n8n instance
3. Go to **Workflows** → click **⋮** → **Import from file**
4. Select `watt-workflow.json`
5. Configure your credentials (see above)
6. Activate the workflow

---

## Example Output

The agent delivers a structured report to Discord covering:

- Current cheapest IMPORT tariff in the North West region
- Unit rate comparison across all 15 tariffs
- Standing charge analysis
- Recommended tariff based on typical household usage
- Market trend observations

---

## Project Context

Watt was built as a portfolio project to demonstrate autonomous AI agent design using n8n — specifically: multi-step API chaining, data filtering and transformation logic, LLM-powered report generation, and cross-platform delivery.

It forms part of a broader portfolio of AI automation projects available at [github.com/christopherwyk](https://github.com/christopherwyk).

---

## Roadmap

### Delivery
- [ ] Email delivery option — send the report to an inbox alongside Discord
- [ ] Multi-channel support — Slack, Telegram, or WhatsApp delivery

### Data Sources
- [ ] Agile Octopus real-time pricing (half-hourly rate tracking)
- [ ] Gas tariff support alongside electricity
- [ ] National Grid carbon intensity API — include grid carbon data in the report
- [ ] Ofgem price cap data integration — benchmark tariffs against the cap
- [ ] Historical rate tracking — store daily snapshots to Airtable or Google Sheets

### Intelligence & Evaluation
- [ ] Claude report evaluator — automated eval node that scores each generated report on accuracy, structure, and completeness (0–1.0 scoring) before delivery
- [ ] Prompt version control — store prompt templates as versioned configs so report style can be updated without touching the workflow
- [ ] Anomaly detection — flag unusual rate spikes or drops in the report automatically

### Coverage
- [ ] Multi-region support — extend normalisation beyond North West to all UK regions
- [ ] Tariff comparison over time — track whether a tariff has gotten cheaper or more expensive week-on-week

---


## Author

**Christopher Ho**
[github.com/christopherwyk](https://github.com/christopherwyk) · [christopherwyk@gmail.com](mailto:christopherwyk@gmail.com)
