# 02 · Email Sequences — English (4 prompts)

> **Compliance first.** Send only to people who opted in, asked to be contacted, or have an existing business relationship with you. Include your real identity, a postal address and a working unsubscribe line where required (CAN-SPAM, GDPR/PECR, CASL). These prompts are for *warm* and *permission-based* outreach.

---

## E-01 · Warm Follow-Up Sequence (4 touches after an inbound request / event / demo sign-up)

```text
You are an email copywriter for B2B SaaS. Write a 4-email follow-up sequence for a prospect who {{trigger_event}}
(e.g. "downloaded our pricing guide", "booked then missed a demo", "talked to us at {{event_name}}").

Inputs:
- Sender: {{sender_name}}, {{sender_role}} at {{company}}
- Prospect: {{first_name}}, {{role}} at {{prospect_company}}
- What they showed interest in: {{interest}}
- One relevant proof point (true): {{proof_point}}
- CTA: {{cta}} (e.g. 15-min call link, reply with a number, trial link)

Constraints for every email:
- Subject ≤ 6 words, lowercase-friendly, no clickbait, no "Re:" unless it is a real reply.
- Body ≤ 110 words, plain text, one CTA, no attachments, no images.
- Email 1 (day 0): reference the trigger + one useful resource.
- Email 2 (day 3): one insight relevant to {{role}} + soft CTA.
- Email 3 (day 7): short case/proof point + direct CTA.
- Email 4 (day 14): polite close-the-loop ("should I stop following up?").
- End each with: "{{sender_name}} · {{company}} · {{postal_address}} — reply 'stop' and I won't email again."

Output as Markdown: ### Email N — Day X / **Subject:** / body.
```

---

## E-02 · Post-Demo Recap + Mutual Action Plan

```text
Write a post-demo recap email from {{sender_name}} to {{prospect_names}} at {{prospect_company}}.

<demo_notes>{{notes}}</demo_notes>

Structure:
1. One-line thank-you that names something specific they said.
2. "What we heard" — 3 bullets of their goals in THEIR words.
3. "What we showed" — 3 bullets mapping features → goals.
4. "Mutual plan" — a table: Step | Owner | Target date (from today {{today}}), ending at go-live.
5. One question to confirm the plan.
≤ 200 words. No superlatives. No invented commitments.
```

---

## E-03 · Re-Engagement (closed-lost or gone-quiet, existing relationship)

```text
Write 2 alternative re-engagement emails (A/B) to {{first_name}}, who evaluated {{product}} {{months_ago}} months ago and chose {{outcome}}
(e.g. "to wait", "a competitor", "to build in-house").

Something genuinely new since then: {{what_changed}}

Rules: ≤ 80 words each, no guilt-tripping, no fake urgency, one yes/no question as CTA.
Variant A leads with what changed. Variant B leads with a question about their current approach.
Include an opt-out line.
```

---

## E-04 · Referral / Intro Ask (happy customer)

```text
Draft a short note asking {{customer_name}} (a happy customer — evidence: {{evidence_of_happiness}}) for one introduction.

Make it easy:
- Explain in one sentence who would benefit (profile: {{ideal_referral_profile}}).
- Offer a forwardable 3-sentence blurb they can paste as-is (write it).
- Make "no" easy and guilt-free.
≤ 120 words total including the blurb.
```
