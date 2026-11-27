# 04 · Meetings → Action Items → Tickets (5 prompts)

Pair with `templates/schemas/action-items.schema.json` and `templates/workflows/meeting-to-linear.workflow.yaml`.

---

## M-01 · Transcript → Action Items (strict JSON)

```text
You are a meticulous chief-of-staff. Extract action items from the meeting transcript.

Meeting: {{meeting_title}} on {{date}}. Attendees: {{attendees_with_roles}}.
<transcript>
{{transcript}}
</transcript>

An action item exists ONLY if someone committed to do something, or a task was explicitly assigned.
Ideas, opinions and "we should maybe" are NOT action items — put them in "parking_lot".

Return ONLY JSON:
{
  "summary": string,                      // ≤ 3 sentences
  "decisions": [{"decision": string, "made_by": string|null, "quote": string}],
  "action_items": [{
    "title": string,                      // imperative, ≤ 10 words: "Draft Q3 pricing page copy"
    "description": string,                // what "done" looks like
    "owner": string|null,                 // must be an attendee name or null
    "due": string|null,                   // ISO date if stated or clearly implied ("by Friday" → resolve from {{date}}), else null
    "priority": "urgent"|"high"|"medium"|"low",
    "source_quote": string,               // verbatim ≤ 25 words proving the commitment
    "confidence": number                  // 0–1
  }],
  "open_questions": string[],
  "parking_lot": string[]
}
Never invent owners or dates. If confidence < 0.6, still include the item but say why in description.
```

---

## M-02 · Action Items → Linear Issues (GraphQL-ready payloads)

```text
Convert these action items into Linear `issueCreate` inputs.

Team key: {{linear_team_key}}   Team ID: {{linear_team_id}}
Owner → Linear user ID map: {{json_user_map}}
Priority map: urgent=1, high=2, medium=3, low=4
Label to add on every issue: {{label_id_meeting}}

<action_items_json>{{paste M-01 output}}</action_items_json>

Return a JSON array of objects with exactly: title, description (Markdown; end with "> Source: {{meeting_title}} {{date}} — \"<source_quote>\""), teamId, assigneeId (null if unmapped), priority, dueDate (YYYY-MM-DD or null), labelIds.
Skip items with confidence < 0.5 and list their titles under a top-level key "skipped" (wrap output as {"issues": [...], "skipped": [...]}).
```

---

## M-03 · Action Items → Jira (bulk CSV import)

```text
Convert the action items JSON into a Jira bulk-import CSV with header:
Summary,Description,Issue Type,Priority,Assignee,Due Date,Labels

- Issue Type: "Task" (use "Bug" only if the item fixes a defect)
- Priority map: urgent→Highest, high→High, medium→Medium, low→Low
- Assignee: map names using {{jira_username_map}}; blank if unknown
- Due Date format: dd/MMM/yy (e.g. 05/Dec/26)
- Labels: "meeting-{{yyyymmdd}}"
- RFC 4180 quoting for any field containing commas, quotes or newlines.
Output ONLY the CSV.

<action_items_json>{{json}}</action_items_json>
```

---

## M-04 · Action Items → Notion database rows

```text
Map each action item to a Notion `pages.create` body for database {{notion_database_id}} whose properties are:
- "Name" (title), "Owner" (people — use {{notion_user_id_map}}), "Due" (date), "Priority" (select: Urgent/High/Medium/Low),
  "Status" (status: "Not started"), "Meeting" (rich_text).
Put the description and source quote in the page body as a paragraph and a quote block.
Return a JSON array of request bodies only.

<action_items_json>{{json}}</action_items_json>
```

---

## M-05 · Decision Log + Follow-Up Email (for people who missed the meeting)

```text
From the transcript, write:
1. A decision log table: # | Decision | Rationale (their words) | Owner | Revisit date (if any)
2. A ≤150-word follow-up email to {{absent_people}} covering: what was decided, what they personally need to do (by name), and the next meeting date {{next_meeting}}.
Use only what's in the transcript. Flag disagreements that were NOT resolved under "Still open".

<transcript>{{transcript}}</transcript>
```
