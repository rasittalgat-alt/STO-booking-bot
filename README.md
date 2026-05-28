# 🔧 СТО - Запись на Услуги (Telegram Bot)

An AI-powered Telegram bot for auto service centers (СТО) that handles client appointment booking, payment verification, and CRM integration — built entirely in [n8n](https://n8n.io).

## Features

- **Conversational AI booking** — clients describe what they need in natural language; the AI agent identifies the right service and books an appointment
- **Payment verification** — clients send a payment receipt photo; the bot validates it automatically
- **Google Calendar sync** — confirmed bookings are added to the service center's calendar instantly
- **Bitrix24 CRM** — a new deal is created in Bitrix24 for every confirmed booking
- **Manager notifications** — the manager receives an instant Telegram message when a booking is confirmed
- **Session memory** — the AI remembers context within a conversation (multi-turn dialogue)

## Architecture

```
Telegram Trigger
    │
    ├─► Payment Check ──► Receipt Verification ──► Confirmation
    │
    └─► AI Agent (Google Gemini / OpenAI)
            │   Tools:
            │   ├─ get_services     — lists available services & prices
            │   └─ book_appointment — creates the booking
            │
            └─► On booking confirmed:
                    ├─ Save booking record
                    ├─ Send payment link to client
                    ├─ Create event in Google Calendar
                    ├─ Notify manager via Telegram
                    └─ Create deal in Bitrix24
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Workflow automation | n8n |
| AI model | Google Gemini / OpenAI GPT |
| Messaging | Telegram Bot API |
| Calendar | Google Calendar |
| CRM | Bitrix24 |
| Payment | Custom payment link |

## Nodes (22 total)

| Node | Purpose |
|------|---------|
| Telegram Trigger | Entry point — receives all messages |
| Proverka oplaty | Checks if message is a payment receipt |
| Verifikatsiya cheka | Validates receipt image |
| AI Agent | Core booking logic via LLM |
| Google Gemini / OpenAI | LLM backends (configurable) |
| Pamyat sessii | Window buffer memory for conversation context |
| get_services (Tool) | Returns available services list |
| book_appointment (Tool) | Creates appointment record |
| Sohranenie broni | Persists booking data |
| Otpravka ssylki oplaty | Sends payment link to client |
| Sozdat v Calendar | Creates Google Calendar event |
| Notify Manager | Sends Telegram notification to manager |
| Bitrix24 - Sozdat sdelku | Creates deal in Bitrix24 CRM |

## Setup

1. **Import the workflow** — import `workflow.json` into your n8n instance
2. **Configure credentials:**
   - Telegram Bot token
   - Google Gemini API key (or OpenAI API key)
   - Google Calendar credentials
   - Bitrix24 webhook
3. **Update node parameters:**
   - Manager Telegram chat ID in `Notify Manager`
   - Payment link template in `Otpravka ssylki oplaty`
   - Service list in `get_services` tool
4. **Activate** the workflow

## How It Works

1. Client sends a message to the Telegram bot
2. The bot checks whether it's a payment receipt or a regular message
3. For regular messages → the AI Agent handles the conversation, understands intent, and books the appointment using its tools
4. Once booked → the workflow sends a payment link, creates a calendar event, notifies the manager, and logs the deal in Bitrix24
5. When the client sends a receipt → the bot verifies it and sends a confirmation

## Author

Built by [Talgat Rashit](https://github.com/rasittalgat-alt) as part of an AI automation portfolio.
