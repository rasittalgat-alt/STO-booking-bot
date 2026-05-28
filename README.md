# 🔧 Auto Service Booking Bot (Telegram)

A Telegram bot for auto service centers with an AI agent built on [n8n](https://n8n.io). Clients describe their issue in natural language — the bot books an appointment, verifies payment, and syncs everything with Google Calendar and Bitrix24 CRM.

## Demo

▶️ **[Watch video demo](https://drive.google.com/file/d/1Jrx67hOfYH8-V-cTocf5V6rjBZ_gitRm/view?usp=sharing)**

## Features

- **Conversational booking** — clients describe their problem in free text; the AI agent identifies the right service and creates an appointment
- **Payment verification** — clients send a photo of the receipt; the bot validates it automatically
- **Google Calendar sync** — confirmed bookings appear in the calendar instantly
- **Bitrix24 CRM** — a deal is created automatically for every confirmed booking
- **Manager notifications** — instant Telegram message when a booking is confirmed
- **Session memory** — the bot remembers conversation context within a session

## Architecture

```
Telegram Trigger
    │
    ├─► Payment Check ──► Receipt Verification ──► Confirmation to client
    │
    └─► AI Agent (Google Gemini / OpenAI)
            │   Tools:
            │   ├─ get_services      — list of services and prices
            │   └─ book_appointment  — create booking
            │
            └─► On booking confirmed:
                    ├─ Save booking record
                    ├─ Send payment link to client
                    ├─ Create Google Calendar event
                    ├─ Notify manager via Telegram
                    └─ Create deal in Bitrix24
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Automation | n8n |
| AI model | Google Gemini / OpenAI GPT |
| Messaging | Telegram Bot API |
| Calendar | Google Calendar |
| CRM | Bitrix24 |

## Setup

1. **Import the workflow** — upload `workflow.json` to your n8n instance
2. **Configure credentials:**
   - Telegram Bot token
   - Google Gemini or OpenAI API key
   - Google Calendar credentials
   - Bitrix24 webhook
3. **Update node parameters:**
   - Manager chat ID in `Notify Manager` node
   - Payment link template in `Otpravka ssylki oplaty`
   - Service list in `get_services` tool
4. **Activate** the workflow

## How It Works

1. Client sends a message to the Telegram bot
2. Bot checks — is it a payment receipt or a regular message?
3. Regular message → AI agent holds a conversation, understands the need, creates a booking
4. After booking → sends payment link, creates calendar event, notifies manager, logs deal in Bitrix24
5. Client sends a receipt → bot verifies and sends confirmation

## Author

[Talgat Rashit](https://github.com/rasittalgat-alt)

---

# 🔧 СТО — Запись на Услуги (Telegram-бот)

Telegram-бот для автосервиса с AI-агентом на базе [n8n](https://n8n.io). Принимает заявки от клиентов, записывает на услуги, проверяет оплату и синхронизирует данные с Google Calendar и Bitrix24.

## Демо

▶️ **[Смотреть видео-демо](https://drive.google.com/file/d/1Jrx67hOfYH8-V-cTocf5V6rjBZ_gitRm/view?usp=sharing)**

## Возможности

- **Запись через чат** — клиент описывает проблему своими словами, AI-агент определяет нужную услугу и создаёт запись
- **Проверка оплаты** — клиент отправляет фото чека, бот верифицирует его автоматически
- **Google Calendar** — подтверждённые записи сразу появляются в календаре
- **Bitrix24 CRM** — по каждой записи автоматически создаётся сделка
- **Уведомление менеджера** — мгновенное сообщение в Telegram при подтверждении записи
- **Память сессии** — бот помнит контекст в рамках одного разговора

## Архитектура

```
Telegram Trigger
    │
    ├─► Проверка оплаты ──► Верификация чека ──► Подтверждение клиенту
    │
    └─► AI-агент (Google Gemini / OpenAI)
            │   Инструменты:
            │   ├─ get_services      — список услуг и цен
            │   └─ book_appointment  — создание записи
            │
            └─► После подтверждения записи:
                    ├─ Сохранение брони
                    ├─ Отправка ссылки на оплату клиенту
                    ├─ Создание события в Google Calendar
                    ├─ Уведомление менеджера в Telegram
                    └─ Создание сделки в Bitrix24
```

## Стек

| Компонент | Технология |
|-----------|-----------|
| Автоматизация | n8n |
| AI-модель | Google Gemini / OpenAI GPT |
| Мессенджер | Telegram Bot API |
| Календарь | Google Calendar |
| CRM | Bitrix24 |

## Установка и настройка

1. **Импортируй воркфлоу** — загрузи `workflow.json` в свой инстанс n8n
2. **Настрой учётные данные:**
   - Токен Telegram-бота
   - API-ключ Google Gemini или OpenAI
   - Учётные данные Google Calendar
   - Вебхук Bitrix24
3. **Обнови параметры узлов:**
   - Chat ID менеджера в узле `Notify Manager`
   - Шаблон ссылки на оплату в узле `Otpravka ssylki oplaty`
   - Список услуг в инструменте `get_services`
4. **Активируй** воркфлоу

## Как работает

1. Клиент пишет сообщение в Telegram-бот
2. Бот определяет — это чек об оплате или обычное сообщение
3. Обычное сообщение → AI-агент ведёт диалог, выясняет потребность и создаёт запись
4. После записи → отправляет ссылку на оплату, создаёт событие в календаре, уведомляет менеджера, логирует сделку в Bitrix24
5. Клиент присылает чек → бот проверяет и отправляет подтверждение

## Автор

[Talgat Rashit](https://github.com/rasittalgat-alt)
