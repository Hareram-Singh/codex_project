# Voice Sales Agent Project

This project initializes a **voice agent** that can:

- **Make outbound calls** to share sales information.
- **Receive inbound calls** from customers.
- Provide product/pricing details.
- Deliver notices (renewals, payment reminders, delivery updates, promotions).
- Route urgent calls to a human agent.

---

## 1) Project Goals

Build a production-ready voice assistant for sales and notices with:

- Natural conversation flow.
- CRM-aware responses.
- Call logging and transcript storage.
- Compliance support (consent, opt-out, DNC handling).

---

## 2) Suggested Tech Stack

- **Telephony:** Twilio / Vonage / Plivo
- **Voice AI:** OpenAI Realtime API or speech-to-text + LLM + text-to-speech pipeline
- **Backend:** Node.js (Express/Fastify) or Python (FastAPI)
- **Database:** PostgreSQL (customers, call logs, notice history)
- **Queue/Jobs:** Redis + BullMQ / Celery
- **CRM Integration:** Salesforce / HubSpot / Zoho API
- **Observability:** OpenTelemetry + Grafana/Datadog

---

## 3) Core Features

### Inbound Call Handling

- Greeting + language selection.
- Caller verification (phone, OTP, customer ID).
- Intent detection (pricing, plan details, invoice, delivery notice).
- FAQ answering and lead capture.
- Human handoff for edge cases.

### Outbound Campaign Calls

- Automated daily/weekly call campaigns.
- Personalized script using CRM attributes.
- Retry logic with time windows and local timezone awareness.
- Disposition tracking (answered, voicemail, callback requested, no answer).

### Notices & Alerts

- Payment due reminders.
- Policy/plan renewal notifications.
- Shipping and appointment notices.
- Escalation for failed deliveries or missed payments.

---

## 4) Minimal Data Model

Create these tables/collections first:

1. `customers`
   - `id`, `name`, `phone`, `email`, `language`, `timezone`, `consent_status`
2. `sales_offers`
   - `id`, `title`, `description`, `price`, `valid_until`
3. `call_sessions`
   - `id`, `customer_id`, `direction`, `start_time`, `end_time`, `outcome`, `recording_url`
4. `call_transcripts`
   - `id`, `call_session_id`, `speaker`, `timestamp`, `text`
5. `notices`
   - `id`, `customer_id`, `type`, `message`, `status`, `sent_at`

---

## 5) Environment Variables (`.env`)

```bash
# Telephony
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
TWILIO_PHONE_NUMBER=

# AI
OPENAI_API_KEY=
OPENAI_MODEL=gpt-4o-realtime-preview

# App
PORT=3000
APP_BASE_URL=https://your-domain.com

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/voice_agent

# Optional
REDIS_URL=redis://localhost:6379
CRM_API_KEY=
```

---

## 6) Suggested API Endpoints

- `POST /calls/outbound/start` → Start outbound sales/notice call
- `POST /calls/webhook/inbound` → Telephony inbound webhook
- `POST /calls/webhook/status` → Call status updates
- `POST /calls/webhook/transcript` → Transcript chunks/events
- `GET /customers/:id/history` → Customer interaction history
- `POST /notices/send` → Trigger notice campaign/manual notice

---

## 7) Conversation Prompt Baseline

Use a system prompt like:

- You are a polite sales and notices voice assistant.
- Confirm customer identity before sharing private information.
- Keep responses short (1–2 sentences for voice).
- Never invent pricing or dates; use CRM/API data only.
- Offer SMS/email summary after the call.
- If user says “stop”, mark opt-out and end gracefully.

---

## 8) Compliance Checklist (Important)

- Honor **Do Not Call** and opt-out requests.
- Follow local outbound call time restrictions.
- Announce recording if legally required.
- Store consent proof with timestamp.
- Mask sensitive data in logs/transcripts.

> This is implementation guidance, not legal advice. Validate with your legal/compliance team.

---

## 9) MVP Milestones

### Phase 1 (Week 1)

- Inbound call webhook + greeting flow
- Basic FAQ + human handoff
- Transcript storage

### Phase 2 (Week 2)

- Outbound campaign runner
- CRM personalization
- Call outcomes dashboard

### Phase 3 (Week 3)

- Notice templates + scheduling
- Retry/queueing + analytics
- QA and compliance hardening

---

## 10) Quick Start (Example)

1. Create backend service (`/src`) and `.env` file.
2. Configure telephony webhook URL to `/calls/webhook/inbound`.
3. Add LLM + TTS/STT integration.
4. Implement customer lookup and consent checks.
5. Test with sandbox numbers before production release.

---

## 11) Future Enhancements

- Multilingual voices with automatic language switching.
- Sentiment-based escalation to senior agents.
- Smart upsell/cross-sell recommendations.
- A/B testing for scripts and opening lines.

---

If you want, I can also generate a **starter Node.js or FastAPI code scaffold** for this README in the next step.
