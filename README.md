# Weather Reporting Bot
Here is the live link : https://kenlykena.app.n8n.cloud/webhook/8a86d384-6de6-4c14-8bd2-ab7911f6f19a/chat
n8n chat workflow that looks up the weather for a city and answers with a short description plus a funny suggestion.

Synced from the n8n workflows **Weather Reporting Bot** (`vd2g0Bd4NaMPZyNi`) and **Child Workflow** (`y2IPCdKqCLWwtrFQ`)

## What it does

1. A chat trigger receives the user message.
2. An AI Agent (OpenAI `gpt-4.1-mini`) decides when to look up weather.
3. Window memory keeps the last 10 turns of context.
4. The **Weather** tool calls **Child Workflow**, which:
   - Reads the city from `query`
   - Fetches current conditions from OpenWeatherMap
   - Asks OpenAI `gpt-4.1` to rewrite the API payload in plain English
   - Returns that text as `Response`
5. The parent agent adds a funny line about what to do in that weather.

## Import into n8n

1. In n8n, open **Workflows** → **Import from file**.
2. Import `child-workflow.json` first.
3. Import `weather-reporting-bot.json`.
4. On the parent **Weather** tool node, point `workflowId` at the imported Child Workflow (n8n assigns a new ID on import).
5. Attach credentials:
   - **OpenAI** on `OpenAl` (parent) and `Message a model` (child)
   - **OpenWeatherMap** on `OpenWeatherMap` (child)
6. Publish / activate both workflows.

API keys are not stored in this repository. Reconnect credentials after import.

## Files

| File | Workflow |
| --- | --- |
| `weather-reporting-bot.json` | Chat parent (AI Agent + Weather tool) |
| `child-workflow.json` | OpenWeatherMap lookup used as a tool |

## Nodes

### Weather Reporting Bot

| Node | Type |
| --- | --- |
| When chat message received | Chat Trigger |
| AI Agent | LangChain Agent |
| OpenAl | OpenAI Chat Model |
| Simple Memory | Window Buffer Memory |
| Weather | Call n8n Workflow Tool |

### Child Workflow

| Node | Type |
| --- | --- |
| When Executed by Another Workflow | Execute Workflow Trigger |
| Edit Fields | Set |
| OpenWeatherMap | OpenWeatherMap |
| Message a model | OpenAI |
| Edit Fields1 | Set |
