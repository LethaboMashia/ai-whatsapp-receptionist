# WhatsApp AI Receptionist — n8n + Claude Automation

An AI-powered front-desk receptionist for small medical and service businesses, built on n8n workflow automation and the Anthropic Claude API. It answers WhatsApp enquiries instantly, holds context across a conversation, and guides the customer toward a booked appointment — without a human needing to be online.

## Why I built this

Small clinics and service businesses lose leads every day simply because nobody replies to a WhatsApp enquiry fast enough. "Speed to lead" — how quickly a business responds to a new enquiry — is one of the strongest predictors of whether that enquiry converts into a booking. This system closes that gap by responding in seconds, 24/7, with a consistent, warm, on-brand tone, and it's built to be reusable across different businesses and channels, not a one-off script.

## Architecture
WhatsApp message received
│
▼
WhatsApp Trigger (n8n)
│
▼
AI Agent (LangChain agent node)
│
├── Anthropic Chat Model (Claude Sonnet 5) — generates the reply
│
└── Simple Memory (buffer window, keyed by sender’s phone number)
— gives the agent conversation history per customer
│
▼
Send Message (WhatsApp Business API)
│
▼
Reply delivered to customer

**Components:**
- **Trigger** — n8n's native WhatsApp Trigger node, listening for incoming messages via the WhatsApp Business API
- **Brain** — An AI Agent node powered by Claude Sonnet 5 (Anthropic), driven by a custom system prompt that defines the business persona, tone, and conversation goals
- **Memory** — A buffer-window memory node keyed by the sender's phone number, so the AI remembers earlier messages in the same conversation rather than treating every message as a fresh start with no context
- **Output** — The agent's generated response is sent back to the same WhatsApp number via the WhatsApp Business API send-message node

## What the AI is instructed to do

The system prompt positions the AI as a clinic's front-desk assistant that:
- Opens every new conversation with a consistent, warm, branded greeting (defined word-for-word in the prompt, so the business's voice stays identical across every conversation)
- Collects the information a real receptionist would need — patient name, reason for the visit, and a preferred day/time
- Handles both new and returning patient enquiries, recognising returning conversations through the memory layer
- Is explicitly directed to move every conversation toward a booked appointment, not just answer isolated questions — the agent has a goal, not just a script

## Two builds in this repo

- **`WhatsApp_AI_Receptionist.json`** — built for a single clinic ("MashiaMedi Clinic"), a focused single-channel (WhatsApp-only) demo proving the core pattern end-to-end
- **`Speed_to_lead_Medical.json`** — a more advanced, generalised version of the same architecture. Its system prompt is written for a multi-channel front desk (WhatsApp, website, email, Instagram, Facebook referenced in the prompt logic) under a "speed-to-lead" framing, built to show how the same underlying pattern extends beyond one single channel or one single business

Both share the same core architecture (trigger → agent → memory → send); the difference is in the system prompt design and how generalisable the persona and logic are.

## Tech stack

- **n8n** — visual workflow automation and orchestration platform
- **Claude API (Anthropic)** — the LLM powering the conversational agent (Claude Sonnet 5)
- **WhatsApp Business API** — the message channel, via n8n's native WhatsApp integration
- **LangChain agent framework** — accessed through n8n's LangChain nodes, providing structured agent reasoning plus the memory layer

## Status

Built and tested as a working demo system — over 200 test conversations run through the WhatsApp Receptionist workflow to validate conversational flow, memory persistence across messages, and reply quality and tone consistency. This is a demo built to prove the automation pattern works end-to-end; it has not yet been deployed into a live, paying client environment.

## Credentials

API keys and WhatsApp Business credentials are **not included** in these exported workflow files — n8n strips credential values automatically on export. To run this yourself, you will need your own Anthropic API key and your own WhatsApp Business API setup (phone number ID and access token).

## About me

I'm Lethabo Mashia, a BSc IT (Software Engineering) student building practical AI automation systems alongside my studies. I build these end-to-end — workflow design, prompt engineering, and integration — to prove I can ship working systems, not just describe them.

---
GitHub: [github.com/LethaboMashia](https://github.com/LethaboMashia)
