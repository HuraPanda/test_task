# AI Gateway MVP

Anonymous prompt gateway: React + Vite client, Node + Express server, direct PostgreSQL access through `pg`, and one OpenAI-compatible provider (OpenRouter by default).

## Features

- anonymous `HttpOnly` cookie session, no login
- prompt sent to server and to the configured AI provider
- successful prompt/answer pairs saved per session in PostgreSQL
- latest history cached in browser `localStorage`
- one small server file and one React component; no ORM or service layers

## Quick start

### Install dependencies

```bash
pnpm install
```

### Start the project

```bash
docker compose up -d db
cp .env.example .env
# fill AI_API_KEY and AI_MODEL in .env before sending prompts
pnpm start
```

The server serves the production client at:

```text
http://localhost:3000
```

For local development, run the server and Vite in separate terminals:

```bash
pnpm dev:server
pnpm dev:client
```

## API and structure

- `POST /api/session` creates or confirms the cookie session.
- `GET /api/history` returns its latest 50 records.
- `POST /api/requests` accepts `{ "prompt": "..." }` and returns the saved answer.

```text
server.mjs       Express, pg, sessions, AI call and SQL
client/          Vite entry, one React component and CSS
compose.yaml     Local PostgreSQL
tasks/           SDD plan and implementation checklist
```

## Environment

See `.env.example`: `DATABASE_URL`, `AI_BASE_URL`, `AI_API_KEY`, `AI_MODEL`, `PORT`, `APP_ORIGIN`, and optional timeout/cookie settings. The API key is server-only and is never bundled into the client.

The MVP deliberately excludes authentication, streaming, pagination, rate limiting, retries, multiple providers, and conversation context. A real provider call must be checked with a valid key; local build checks do not prove provider access.

## Возможные следующие функции

Сервис можно развивать постепенно, сохраняя простой основной сценарий:

- потоковый ответ токен за токеном через SSE;
- несколько диалогов с названиями, удалением и поиском;
- передача выбранных сообщений в контекст продолжения диалога;
- выбор модели и AI-провайдера с серверной конфигурацией;
- шаблоны промптов и сохранённые системные инструкции;
- Markdown-рендеринг с безопасной санитизацией;
- экспорт истории в JSON, Markdown или CSV;
- регистрация и синхронизация истории между устройствами;
- лимиты запросов, квоты и отображение оставшегося бюджета;
- rate limiting, CAPTCHA и мониторинг злоупотреблений;
- метрики задержки, стоимости и количества токенов;
- фоновые задачи для длинных запросов и уведомления о готовности;
- загрузка файлов и работа с документами через поддерживаемые модели;
- роли, командные рабочие пространства и общий доступ к диалогам;
- автоматические оценки качества ответов и обратная связь пользователя;
- резервное копирование, очистка старой истории и административная панель.

## Scripts

- `pnpm build` — build the Vite client
- `pnpm start` — start Express after the client is built
- `pnpm dev` — run Vite and the server together

## License

ISC
