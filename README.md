# AI Ops & MCP Prompt + Automation Mega-Pack · v1.0

💼 **Packaged on Gumroad:** [AI Ops & MCP Prompt + Automation Mega-Pack ($12)](https://playmaster72.gumroad.com/l/mxttvof) — 28 prompts + 12 templates in one download — one-click packaged download that supports continued development.


**28 copy-paste system prompts + 12 ready-to-use JSON/YAML/IDE templates (40 assets)** for the business tasks teams actually automate first: lead qualification, follow-up email, meeting → tickets, support triage & refunds, weekly KPI and finance analysis, and Claude Desktop / Cursor MCP setup.

Works with Claude, ChatGPT, Gemini, Cursor, Claude Code, n8n, Make and Zapier. No code required for the prompts; the workflows are tool-agnostic specs you can map onto any automation platform or a small script.

## Quick start (5 minutes)

1. Open a prompt file in `prompts/`, copy one fenced block, replace the `{{variables}}`, paste into your AI tool.
2. For JSON-returning prompts, validate output against the matching schema in `templates/schemas/` (most platforms support "structured output" / JSON Schema directly).
3. For MCP: copy `templates/ide/claude_desktop_config.json` into
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
   - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
   then delete the servers you don't need, fill in paths/tokens, restart Claude Desktop.
   For Cursor, copy `templates/ide/cursor-mcp.json` to `.cursor/mcp.json` in your project.
4. For workflows: read the YAML/JSON in `templates/workflows/` — each `step` maps 1:1 to a node in n8n/Make/Zapier or a function in your script. Every workflow ships with `dry_run: true`.

## What's inside

### Prompts (28)
| File | IDs | Use case |
|---|---|---|
| `prompts/01-sales-lead-qualification.md` | S-01 … S-05 | BANT+ICP lead scorer (JSON), batch CSV triage, account research brief, objection handler, discovery notes → CRM fields |
| `prompts/02-email-sequences-en.md` | E-01 … E-04 | Warm 4-touch follow-up, post-demo recap + mutual plan, re-engagement A/B, referral ask |
| `prompts/03-email-sequences-ko.md` | K-01 … K-04 | 한국어: 문의 고객 후속 3통, 미팅 요약 메일, 휴면 고객 재접촉, 한↔영 톤 다듬기 (정보통신망법 준수 가이드 포함) |
| `prompts/04-meeting-to-tickets.md` | M-01 … M-05 | Transcript → action items (strict JSON) → Linear / Jira CSV / Notion payloads, decision log + absentee email |
| `prompts/05-support-escalation-refunds.md` | C-01 … C-05 | Ticket triage classifier, policy-grounded refund responder, escalation hand-off, de-escalation, macro/KB generator |
| `prompts/06-kpi-finance-analyzer.md` | F-01 … F-05 | Weekly KPI brief, budget-vs-actual variance, cohort retention, cash runway scenarios, transaction anomaly scan |

### Templates (12)
| File | What it is |
|---|---|
| `templates/ide/claude_desktop_config.json` | Claude Desktop MCP config: filesystem, memory, fetch, GitHub, Linear, Notion |
| `templates/ide/cursor-mcp.json` | Cursor `.cursor/mcp.json`: filesystem, GitHub, Linear, read-only Postgres |
| `templates/ide/.cursorrules` | General engineering rules for Cursor (small diffs, tests, no invented APIs) |
| `templates/ide/cursorrules-typescript-api.mdc` | Scoped Cursor rule for TypeScript API services |
| `templates/ide/CLAUDE.md` | Claude Code project memory for an ops-automation repo (dry-run by default, PII redaction) |
| `templates/schemas/action-items.schema.json` | JSON Schema for M-01 |
| `templates/schemas/lead-score.schema.json` | JSON Schema for S-01 |
| `templates/schemas/support-triage.schema.json` | JSON Schema for C-01 |
| `templates/workflows/meeting-to-linear.workflow.yaml` | Transcript → redact → extract → Linear issues → Slack recap |
| `templates/workflows/support-triage.workflow.yaml` | Ticket → triage → route → policy-grounded draft (human sends) |
| `templates/workflows/weekly-kpi-report.workflow.yaml` | Monday cron → compute with code → LLM narrative → Slack + email |
| `templates/workflows/lead-qualification.workflow.json` | Consent gate → enrich → score → CRM → follow-up draft |

Plus `templates/sample-data/weekly_kpi_sample.csv` to test the KPI prompts immediately.

## Design principles baked into every prompt
- **Grounded:** "use only the provided material; write `unknown`/`null` instead of guessing."
- **Structured:** JSON outputs come with a schema, so you can validate and automate.
- **Safe by default:** workflows are dry-run first, humans approve customer-facing sends, inputs are treated as data (prompt-injection guard), PII is redacted.
- **Compliant outreach:** email prompts target opt-in / existing-relationship contacts and include unsubscribe lines (CAN-SPAM, GDPR, 정보통신망법).

## Notes
- MCP server package names were correct at release; check each server's README for the latest install command and required scopes. Start every token with the least privilege it needs.
- Numbers in finance/KPI analysis should be computed with code (see the workflow) — the prompts tell the model to do so when a code tool is available.

## Need it wired into your stack?

These prompts and templates are free (MIT). If you want them connected to your CRM, helpdesk, Linear/Jira or Slack as a working MCP server or n8n/Make flow, I run fixed-scope 2-week pilots:

- Live demo & details: https://quietstack-labs.github.io/mcp-automation-starter/
- Email: [alborotv@gmail.com](mailto:alborotv@gmail.com?subject=AI%20Ops%20Pack%20Pilot%20Inquiry)

Related open-source tools: [mcp-automation-starter](https://github.com/quietstack-labs/mcp-automation-starter) · [mcp-support-quote-starter](https://github.com/quietstack-labs/mcp-support-quote-starter) · [invoice-usdc-kit](https://github.com/quietstack-labs/invoice-usdc-kit) · [data-swiss-army](https://github.com/quietstack-labs/data-swiss-army)

## License
MIT — see `LICENSE`.
