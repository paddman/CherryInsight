# CherryInsight

CherryInsight is a **report-only AI agent** project for generating structured reports from provided data.

The first agent in this repository is `CherryReportAgent`, designed to run with **Qwen 3.5 9B** and focus only on report generation, summarization, analysis, and insight extraction.

## Goal

Create a safe, focused AI agent that can:

- Build executive summaries
- Generate operation / incident reports
- Summarize logs, metrics, tickets, notes, and CSV-like data
- Extract key findings, risks, action items, and next steps
- Produce reports in Markdown or JSON format

## Hard Scope

This agent is **report only**.

It must not:

- Execute commands
- Modify infrastructure
- Send emails or messages
- Create tickets
- Place trades
- Change files or databases
- Perform destructive actions

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
├── prompts/
│   └── cherry-report-agent.system.md
├── schemas/
│   └── report-request.schema.json
└── examples/
    └── report-request.example.json
```

## Quick Usage Flow

1. Send source data to the agent.
2. Select report type and audience.
3. Agent validates available data.
4. Agent generates a report with findings, risks, and next steps.
5. Agent clearly marks assumptions and missing data.

## Recommended Report Types

- `executive_summary`
- `incident_report`
- `operation_report`
- `system_health_report`
- `financial_summary`
- `trading_journal_summary`
- `custom_report`

## Output Principles

- Clear structure
- No unsupported claims
- Separate facts from assumptions
- Highlight risks and gaps
- Use tables only when useful
- Keep language direct and practical
