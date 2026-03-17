# 🌍 Automated Country Risk Analyst

> AI-powered country risk intelligence system for investment banking — monitors African markets daily using Groq LLaMA, NewsAPI and n8n workflow automation.

---

## Overview

This system automatically ingests live news for 5 African countries every morning, analyses it using a large language model, and delivers structured investment banking intelligence directly to your inbox — completely free to run.

**Countries monitored:** Ethiopia · Uganda · Rwanda · Zambia · Gabon

---

## What it does every day

- Fetches 20 latest news articles per country via NewsAPI
- Sends articles to Groq LLaMA 3.3 AI for analysis
- Generates structured risk scores across 6 dimensions
- Writes an executive briefing with IB implications
- Flags deal opportunities (M&A, IPO, Project Finance)
- Raises critical alerts for high-risk events
- Delivers a formatted HTML email report to your inbox
- Saves all results to Supabase database for historical tracking

---

## Risk dimensions scored

| Dimension | Description |
|---|---|
| Political stability | Government stability, elections, civil unrest |
| Geopolitical risk | Regional conflicts, international relations |
| Regulatory risk | Policy changes, compliance environment |
| FX / Currency risk | Exchange rate volatility, capital controls |
| Sanctions exposure | International sanctions and restrictions |
| Corruption index | Transparency and governance quality |

---

## Tech stack

| Layer | Tool | Cost |
|---|---|---|
| Automation / Scheduler | n8n (self-hosted) | Free |
| News ingestion | NewsAPI | Free tier |
| AI analysis | Groq LLaMA 3.3 70B | Free tier |
| Database | Supabase | Free tier |
| Email delivery | SMTP / Gmail | Free |
| Language | JavaScript (n8n Code nodes) | — |

**Total monthly cost: $0**

---

## How it works

```
Daily Trigger (06:00)
      ↓
Country List (5 countries)
      ↓
Fetch News (NewsAPI — 20 articles per country)
      ↓
Prepare Headlines (build AI prompt)
      ↓
Groq LLaMA AI Analysis (risk scoring + briefing)
      ↓
Parse Response (JSON extraction + error handling)
      ↓
Build Email HTML (formatted report)
      ↓
Send Email (Gmail SMTP)
      ↓
Save to Supabase (historical database)
      ↓
Critical Alert? → Send urgent email if triggered
```

---

## Sample email output

Each country produces a daily email containing:

- **Risk score** (0–100) with direction indicator (improving / stable / deteriorating)
- **Executive briefing** — 3-sentence IB-focused summary
- **Risk dimension bars** — visual breakdown of all 6 scores
- **Regulatory updates** — recent policy changes with deal impact
- **Deal opportunities** — M&A, IPO, infrastructure signals with urgency rating
- **Alerts** — critical and high severity flags

---

## Setup guide

### Prerequisites
- Node.js installed (`https://nodejs.org`)
- n8n running locally (`npx n8n`)
- Free API keys (all no credit card required):
  - Groq: `https://console.groq.com`
  - NewsAPI: `https://newsapi.org/register`
  - Supabase: `https://supabase.com`

### Installation

**1. Clone this repo**
```bash
git clone https://github.com/Kagiso228/country-risk-analyst.git
```

**2. Import the workflow into n8n**
- Open `http://localhost:5678`
- Click **+ New Workflow** → **⋮ menu** → **Import from file**
- Upload `middle_east_risk_workflow_gemini.json`

**3. Create Supabase table**
```sql
CREATE TABLE country_risk_daily (
  id                 BIGSERIAL PRIMARY KEY,
  country            TEXT NOT NULL,
  country_code       TEXT,
  date               DATE NOT NULL,
  overall_risk_score INTEGER,
  risk_direction     TEXT,
  executive_briefing TEXT,
  analysis_json      JSONB,
  article_count      INTEGER,
  created_at         TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(country, date)
);
```

**4. Add your API keys in n8n**

| Node | Field | Value |
|---|---|---|
| Fetch News | apiKey query param | Your NewsAPI key |
| Gemini AI Analysis | URL `?key=` param | Your Groq API key |
| Send Email | SMTP credential | Your Gmail + App Password |
| Save to Supabase | Supabase credential | Your project URL + anon key |

**5. Activate the workflow**
- Toggle **Inactive → Active** in n8n
- The workflow runs automatically at 06:00 every day

---

## Customisation

### Add more countries
Open the **Country List** node and add entries:
```javascript
{ name: 'Kenya', query: 'Kenya economy investment finance Nairobi', code: 'KE' },
{ name: 'Nigeria', query: 'Nigeria economy investment finance Lagos', code: 'NG' },
```

### Change trigger time
Edit the cron expression in the **Daily Trigger** node:
```
0 6 * * *     = 06:00 every day
0 7 * * 1-5   = 07:00 weekdays only
0 6,18 * * *  = 06:00 and 18:00 daily
```

---

## Skills demonstrated

- Workflow automation with n8n
- LLM / AI integration (Groq LLaMA 3.3)
- REST API integration (NewsAPI, Groq, Supabase)
- JSON data pipeline engineering with error handling
- Financial risk analysis and IB domain knowledge
- JavaScript (n8n Code nodes)
- Database design (Supabase / PostgreSQL)

---

## Author

**Kagiso Mosehla**  
BSc Hons Computer Science · University of Pretoria  
Former Junior Data Scientist · Absa Group Limited  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/kagiso-mosehla-a23071272/)
[![GitHub](https://img.shields.io/badge/GitHub-Kagiso228-black)](https://github.com/Kagiso228)

---

*Built as part of a personal project in AI-powered financial intelligence automation.*
