# 06 · Weekly KPI & Financial CSV Analyzer (5 prompts)

Try them on `templates/sample-data/weekly_kpi_sample.csv`. Pair with `templates/workflows/weekly-kpi-report.workflow.yaml`.

> Tip: with tools that can run code (Claude analysis tool, ChatGPT Advanced Data Analysis, Cursor), add "Compute every number with code; show the code." LLM mental arithmetic on large CSVs is unreliable.

---

## F-01 · Weekly KPI Brief (CSV → exec summary)

```text
You are a data-literate COO. Analyze the weekly KPI CSV below.

Context: business model {{model}} (e.g. B2B SaaS, e-commerce), currency {{currency}}, targets: {{targets_json}}.

<csv>{{csv}}</csv>

Compute (show values, not formulas):
- Latest week vs previous week and vs 4-week average for every numeric column (abs and %).
- Which metrics are off-target and by how much.
- Derived: conversion rates between funnel columns, CAC = spend/new_customers, ARPU = revenue/active_customers where columns exist.

Output:
## Headline (1 sentence a CEO would read)
## Scorecard (table: Metric | This wk | Δ WoW | Δ vs 4wk avg | Target | Status 🟢🟡🔴)
## 3 things that moved and the most likely driver (label hypotheses as hypotheses)
## 2 actions for next week (owner role + metric it should move)
## Data issues (missing weeks, outliers > 3σ, suspicious zeros)
Never fabricate columns that aren't in the CSV.
```

---

## F-02 · P&L Variance Analysis (budget vs actual)

```text
Act as an FP&A analyst. Given monthly budget and actual P&L lines, produce a variance analysis.

<budget_csv>{{budget}}</budget_csv>
<actual_csv>{{actual}}</actual_csv>

1. Table: Line item | Budget | Actual | Variance | Variance % | Fav/Unfav — sorted by |variance| desc.
2. Top 5 variances explained: volume vs price vs timing vs one-off (state which is evidenced vs assumed).
3. Full-year re-forecast if the trend of the last {{n}} months continues (state the method).
4. 3 questions for department owners.
Revenue up = favorable; expense up = unfavorable.
```

---

## F-03 · Cohort Retention & Churn from Raw Subscriptions

```text
From the subscription export below (columns: customer_id, start_date, end_date|null, plan, mrr), build:
- Monthly signup cohorts and logo retention % at months 1, 2, 3, 6, 12 (table).
- Net revenue retention by cohort where mrr history allows.
- Gross churn % and MRR churn for the last 3 months.
- The plan with the worst 3-month retention and a hypothesis why.
Treat null end_date as active. State any rows excluded and why.

<csv>{{subscriptions_csv}}</csv>
```

---

## F-04 · Cash Runway & Scenario Planner

```text
You are a startup CFO. Inputs:
- Cash on hand: {{cash}} on {{date}}
- Last 6 months of revenue and expenses (CSV below)
- Planned changes: {{hires, price changes, big one-off costs}}

<csv>{{monthly_csv}}</csv>

Produce three 12-month scenarios (Base / Downside: revenue growth −50% of base / Upside: +50%),
each with: monthly net burn, ending cash per month, runway month where cash < 0, and the single lever with the biggest runway impact.
Output a table per scenario + a 5-line summary. Show assumptions explicitly.
```

---

## F-05 · Anomaly & Fraud-Signal Scan for Transactions

```text
Scan the transaction export for anomalies. Columns: {{columns}}.

Flag rows that show: duplicate amount+counterparty within 48h, round-number payments > {{threshold}}, weekend/after-hours outliers vs the account's norm, new counterparties with large first payments, refunds > original amount.
Return a Markdown table: row_id | rule triggered | why it's unusual (≤ 15 words) | risk low/med/high.
Then a summary count by rule. This is a screening aid, not an accusation — say so in one line at the top.

<csv>{{transactions_csv}}</csv>
```
