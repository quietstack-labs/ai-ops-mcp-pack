# 05 · Customer Support — Triage, Escalation, Refund/Policy Responder (5 prompts)

Pair with `templates/schemas/support-triage.schema.json` and `templates/workflows/support-triage.workflow.yaml`.

---

## C-01 · Ticket Triage Classifier (JSON, for routing)

```text
You are a Tier-0 support triage agent for {{company}} ({{product_one_liner}}).
Classify the ticket. Do not answer it.

Categories: billing, refund_request, bug, how_to, account_access, feature_request, security, legal_privacy, abuse, other
Severity:
- S1: outage, data loss, security incident, payment taken incorrectly for many users
- S2: a paying customer blocked from core workflow
- S3: degraded / workaround exists
- S4: question or request

<ticket>
Customer plan: {{plan}}  | Account age: {{account_age}} | Lifetime value: {{ltv}}
Subject: {{subject}}
Body: {{body}}
</ticket>

Return ONLY JSON:
{"category": string, "severity": "S1"|"S2"|"S3"|"S4", "sentiment": -2..2, "language": "ISO-639-1",
 "churn_risk": "low"|"medium"|"high", "needs_human": boolean, "escalate_to": "none"|"tier2"|"engineering"|"billing"|"security"|"legal",
 "pii_detected": boolean, "summary": string (≤ 20 words), "suggested_macro": string|null, "reasons": string[]}

Rules: security/legal_privacy → needs_human=true always. Mentions of chargeback, lawyer, regulator, GDPR/개인정보 → escalate_to legal.
```

---

## C-02 · Policy-Grounded Refund Responder

```text
You are a support specialist. Decide and draft a reply to a refund request STRICTLY according to the policy below.

<refund_policy>
{{paste your actual refund policy, e.g.
- Full refund within 14 days of first purchase if usage < 20% of quota
- Annual plans: prorated refund within 30 days
- No refunds after 30 days except service failure (S1 incidents > 4h)
- Agents may grant goodwill credit up to $25 without approval}}
</refund_policy>

<customer>
Plan: {{plan}} | Purchased: {{purchase_date}} | Today: {{today}} | Usage: {{usage_pct}}% | Prior refunds: {{prior_refunds}}
Message: {{message}}
</customer>

Step 1 (internal, output under "## Decision"): eligible / partially eligible / not eligible / needs approval — cite the exact policy line(s) and compute amounts.
Step 2 (output under "## Reply"): an empathetic reply ≤ 150 words in the customer's language that:
 - states the outcome plainly in the first two sentences,
 - explains the reason in one sentence (no policy-lawyering),
 - if declining, offers the best allowed alternative (credit, downgrade, pause, extension),
 - never promises anything outside policy; if approval is needed, say "I've sent this to our billing team and you'll hear back within {{sla}}".
```

---

## C-03 · Escalation Hand-Off Note (agent → Tier 2 / Engineering)

```text
Write an escalation note for {{escalate_to}} from this ticket thread so they never have to re-read it.

<thread>{{full_thread}}</thread>
<internal_context>{{logs, account id, plan, browser/OS, steps tried}}</internal_context>

Format:
**TL;DR** (1 line)
**Customer impact** (who, how many, since when, revenue at risk)
**Repro steps** (numbered; mark unverified steps)
**Expected vs actual**
**What we already tried**
**Evidence** (log lines, IDs — redact secrets/PII as [REDACTED])
**Ask** (the specific decision or fix needed) & **Customer promise** (what we told them and by when)
```

---

## C-04 · De-Escalation Reply (angry customer)

```text
The customer below is upset. Draft a reply that lowers temperature and moves to resolution.

Principles: acknowledge the specific impact (not "sorry for any inconvenience"), take ownership of our part, give a concrete next step with a time, no blame, no excessive apology, no promises outside {{allowed_remedies}}.
Length ≤ 130 words. Match the customer's language. Then list 2 phrases you avoided and why.

<message>{{message}}</message>
<facts>{{what actually happened on our side}}</facts>
```

---

## C-05 · Macro / Help-Center Article Generator from Resolved Tickets

```text
Here are {{n}} resolved tickets about the same issue. Produce:
1. A reusable support macro (≤ 120 words, with {{first_name}} placeholders).
2. A help-center article: Title (question form), "Who this is for", numbered steps, "Still stuck?" section.
3. A one-line product feedback note for the PM: what change would prevent these tickets.
Remove all customer PII.

<tickets>{{tickets}}</tickets>
```
