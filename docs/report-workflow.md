# CherryReportAgent Report Workflow

เอกสารนี้อธิบายว่า CherryReportAgent ทำรายงานจาก input แบบใดก็ได้อย่างไร

## Concept

CherryReportAgent เป็น agent แบบ **report only**

หน้าที่หลักคือรับข้อมูลจากผู้ใช้ แล้วแปลงเป็นรายงานที่อ่านง่าย มีหลักฐาน มีข้อสรุป และมี action items

Agent ไม่ทำ action อื่น เช่น execute command, deploy, update ticket, ส่งอีเมล หรือแก้ระบบ

## Input ที่รับได้

รับได้แบบ `all input` คือส่งมารวมกันได้หลายชนิดใน request เดียว

ตัวอย่าง input ที่รองรับ:

- Text notes
- Logs
- Metrics
- JSON
- Array
- CSV-like text
- Table
- Ticket text
- Email text
- Chat text
- File reference
- Image reference
- URL reference
- Mixed source bundle

## หลักการสำคัญ

รับ input ได้กว้าง แต่รายงานได้เฉพาะข้อมูลที่อ่านได้จาก input เท่านั้น

ถ้าส่งมาเป็นแค่ชื่อไฟล์, image id, หรือ URL โดยไม่มีเนื้อหา Agent จะไม่เดาข้อมูลข้างใน แต่จะใส่ไว้ใน `Missing Data` หรือ `Source Summary`

## Pipeline

```text
User Input
   ↓
Detect Input Type
   ↓
Split Sources
   ↓
Normalize Evidence
   ↓
Extract Facts / Metrics / Events / Risks
   ↓
Detect Missing Data
   ↓
Choose Report Type
   ↓
Generate Report
   ↓
Output Markdown / JSON
```

## Request Shape แบบง่าย

```json
{
  "title": "รายงานสรุปงานประจำวัน",
  "report_type": "operation_report",
  "period": "2026-07-05",
  "audience": "manager",
  "input_mode": "mixed",
  "output_format": "markdown",
  "input_data": "สรุปงานจาก NOC วันนี้..."
}
```

## Request Shape แบบรับทุกอย่าง

ใช้ `input_sources` เมื่อต้องรวมหลายแหล่งข้อมูล

```json
{
  "title": "รายงาน Incident + Operation",
  "report_type": "operation_report",
  "period": "2026-07-05",
  "audience": "manager",
  "input_mode": "mixed",
  "output_format": "markdown",
  "input_data": {
    "summary": "รวมข้อมูลหลายแหล่งสำหรับทำรายงาน"
  },
  "input_sources": [
    {
      "name": "NOC Notes",
      "source_type": "text",
      "content": "09:10 API latency สูง 09:35 restart node-02 แล้วปกติ"
    },
    {
      "name": "Metrics",
      "source_type": "metric",
      "content": {
        "node-02_cpu_peak": "95%",
        "impact_duration": "25 minutes"
      }
    },
    {
      "name": "Ticket",
      "source_type": "ticket",
      "content": {
        "ticket_id": "INC-2026-0705-001",
        "status": "resolved",
        "impact": "API slow for some customers"
      }
    }
  ]
}
```

## Report Types

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

## Output Structure

Default Markdown structure:

```markdown
# <Report Title>

## 1. Scope
## 2. Executive Summary
## 3. Key Findings
## 4. Risks / Issues
## 5. Metrics / Evidence
## 6. Action Items
## 7. Missing Data
## 8. Assumptions
```

## Behavior Rules

- ห้ามเดาข้อมูลที่ไม่มีใน input
- ห้ามสรุป root cause ถ้าหลักฐานไม่พอ
- ถ้าข้อมูลชนกัน ให้แสดง conflict
- ถ้า input มี noise ให้ดึงเฉพาะ evidence ที่ใช้ทำรายงาน
- ถ้าข้อมูลไม่พอ ให้บอกว่าขาดอะไร
- แยก `facts`, `assumptions`, `missing data` ให้ชัด

## Recommended Runtime Flow

Pseudo flow:

```text
1. validate request with schemas/report-request.schema.json
2. load agents/cherry-report-agent.yaml
3. load prompts/cherry-report-agent.system.md
4. send system prompt + request payload to qwen3.5-9b
5. return report output
```

## Minimal Runtime Payload

```json
{
  "system_prompt_file": "prompts/cherry-report-agent.system.md",
  "agent_config_file": "agents/cherry-report-agent.yaml",
  "model": "qwen3.5-9b",
  "request": {
    "title": "รายงานสรุปงาน",
    "report_type": "custom_report",
    "audience": "manager",
    "input_mode": "auto",
    "input_data": "ใส่ข้อมูลทั้งหมดตรงนี้"
  }
}
```
