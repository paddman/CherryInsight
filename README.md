# CherryInsight

CherryInsight is a **report-only AI agent** project for generating structured reports from provided data.

The core agent is `CherryReportAgent`, designed to run with **Qwen 3.5 9B** and focus only on report generation, summarization, analysis, insight extraction, chart recommendation, and action recommendation.

## Pro Skill

### 🍒📝 CherryScribe — Pro Report Agent

**Version:** 1.0 Pro  
**Codename:** CherryScribe  
**Specialization:** End-to-End Report Generation Only  
**Tagline:** Translating Data into Decisions

CherryScribe is the professional report-generation skill for CherryInsight.

Skill files:

```text
skills/cherryscribe/SKILL.md
skills/cherryscribe/cherryscribe.skill.yaml
```

## Goal

Create a safe, focused AI agent that can:

- Ingest mixed user-provided data
- Validate data quality
- Build executive summaries
- Generate operation / incident / business / technical reports
- Summarize logs, metrics, tickets, notes, tables, JSON, CSV-like data, and mixed inputs
- Calculate supported metrics such as MoM, YoY, CAGR, variance, moving averages, and anomaly checks when data exists
- Extract key findings, risks, caveats, action items, and next steps
- Recommend chart types and chart specs
- Produce reports in Markdown, JSON, HTML, PDF config, or chart spec format

## Hard Scope

This project is **report only**.

It must not:

- Execute commands
- Modify infrastructure
- Send emails or messages
- Create, update, close, or approve tickets
- Place trades
- Change files or databases
- Perform destructive actions
- Do unrelated general tasks

## Default Model

```yaml
model: qwen3.5-9b
provider: local
```

If your runtime uses another model alias, adjust the model name in:

```text
agents/cherry-report-agent.yaml
```

## Repository Structure

```text
CherryInsight/
├── README.md
├── agents/
│   └── cherry-report-agent.yaml
├── docs/
│   └── report-workflow.md
├── prompts/
│   └── cherry-report-agent.system.md
├── schemas/
│   └── report-request.schema.json
├── skills/
│   └── cherryscribe/
│       ├── SKILL.md
│       └── cherryscribe.skill.yaml
└── examples/
    ├── report-request.example.json
    └── all-input-report-request.example.json
```

## Quick Usage Flow

1. Send source data to the agent.
2. Select report type and audience.
3. Agent validates available data.
4. Agent analyzes metrics, trends, anomalies, and caveats.
5. Agent generates a report with findings, risks, chart recommendations, and next steps.
6. Agent clearly marks assumptions and missing data.

## Recommended Report Types

- `executive_summary`
- `incident_report`
- `operation_report`
- `system_health_report`
- `financial_summary`
- `trading_journal_summary`
- `business_report`
- `customer_report`
- `technical_report`
- `custom_report`

## CherryScribe Commands

```text
/report quick [topic]
/report full [topic]
/report explain [chart/metric]
/report export [format]
/report critique
```

## Output Principles

- Clear structure
- Professional neutral tone
- No unsupported claims
- No invented numbers
- Separate facts from assumptions
- Highlight risks and gaps
- Include data quality notes
- Mask PII by default
- Use tables only when useful
- Keep language direct and practical
