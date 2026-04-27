# Jerry — Multi-Agent AI Assistant

A multi-agent AI assistant built in n8n that manages emails, calendar, and meeting transcripts through Telegram. One message, three specialized agents, real actions.

Built as part of the Outskill AI Generalist Fellowship (Cohort 7, Week 12).

---

## What It Does

I message Jerry on Telegram like I'd message a friend. It figures out what I need and handles it.

- "Read my last 3 emails" → Email Agent fetches from Gmail
- "Schedule a meeting tomorrow at 3pm" → Calendar Agent creates the event
- "Summarize my last meeting" → Meeting Agent pulls the Fireflies.ai transcript

## Architecture

```
Telegram Message
       │
       ▼
┌──────────────┐
│ Master Agent │  ← OpenAI GPT-4o + Simple Memory
└──┬─────┬─────┬┘
   │     │     │
   ▼     ▼     ▼
Email Calendar Meeting
Agent  Agent   Agent
```

**Master Agent** — Orchestrator powered by OpenAI. Reads my message, decides which sub-agent handles it, delegates.

**Email Agent** — 5 Gmail tools:
- Get Many Messages | Send | Reply | Create Draft | Delete

**Calendar Agent** — 5 Calendar tools + Date & Time:
- Get Events | Check Availability | Create | Delete | Update
- Date & Time tool ensures the AI always knows today's date

**Meeting Agent** — 2 HTTP Request tools (Fireflies.ai GraphQL API):
- List all transcripts | Fetch full transcript details (speakers, attendees, sentences)

Each sub-agent has its own LLM (OpenAI) and memory for independent reasoning.

## Customization

The original workbook uses n8n's built-in chat trigger. I swapped it for a **Telegram trigger** so I can message Jerry from my phone anywhere. Created a bot via BotFather, restricted access to my user ID, and added Edit Fields to map inputs.

## Tech Stack

| Component | Tool |
|---|---|
| Automation Platform | n8n (self-hosted on DigitalOcean VPS) |
| LLM | OpenAI GPT-4o |
| Messaging | Telegram Bot API |
| Email | Gmail API (OAuth2) |
| Calendar | Google Calendar API (OAuth2) |
| Meetings | Fireflies.ai GraphQL API |
| Memory | Simple Memory (Window Buffer) per agent |

## Status

All three agents tested and working end-to-end from Telegram.

---

**Prashanth Pattepu** | Pittsburgh, PA
