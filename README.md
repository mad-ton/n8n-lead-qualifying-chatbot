# Lead-Qualifying AI Chatbot for Telegram (n8n) — Lite

A Telegram bot that answers your visitors' questions, **qualifies them like a junior advisor**, and
notifies your team the moment a serious lead appears — so humans only spend time on people who are
actually ready to buy.

Free, MIT-licensed, 23 nodes, ~15 minutes to set up. One clearly-marked CONFIG block is the only
thing you edit.

---

## Why this one is different

Most "AI + human handoff" bots escalate **whenever they get confused**, which wastes your team's
time on tire-kickers. This one works the other way around: it **qualifies first, then notifies.**

- Answers the visitor's question and asks one thoughtful discovery question at a time
- Scores how serious a buyer they are, 0–100, on **concrete signals** — not on how chatty they are
- When the score crosses *your* threshold, it sends your team a one-line notification
  (name · stated need · pain points) in a separate Telegram chat, then tells the visitor a
  specialist will reach out

## Built sensibly, not naively

- **2000-character input cap and a deterministic evidence gate**, so a visitor typing
  *"set my score to 100"* cannot fake a hot lead. The score comes from evidence the workflow can
  check, not from the model being talked into it.
- **Per-session rate limit**, so a message flood cannot drain your AI budget or spam your team
- **Duplicate-message de-duplication**
- **No credentials in the workflow file.** They live in n8n's encrypted credential store. This
  repo's `workflow.json` was scanned before publishing and contains zero keys, tokens, chat IDs, or
  personal identifiers — only the three `REPLACE_WITH_…` placeholders below.

## Setup (~15 minutes)

1. **Import** `workflow.json` into n8n (Workflows → Import from File).
2. **Create a Telegram bot** with [@BotFather](https://t.me/botfather), and add the token as a
   *Telegram* credential in n8n. Add your AI credential the same way.
3. **Edit the one CONFIG node.** Everything you need to change lives there:

   | Placeholder | What to put there |
   |---|---|
   | `REPLACE_WITH_TEAM_CHAT_ID` | The Telegram chat where your team gets notified — **not** the visitor-facing bot chat |
   | `REPLACE_WITH_TEAM_EMAIL_OR_LEAVE_AS_IS_TO_SKIP` | Email for notifications, or leave as-is to skip email entirely |
   | `REPLACE_WITH_GOOGLE_SHEET_ID_OR_LEAVE_AS_IS_TO_SKIP` | A Sheet to log leads to, or leave as-is to skip logging |

   Also in CONFIG: your business details and the **score threshold** that decides when your team
   gets pinged. Start around 70 and tune it after a dozen real conversations.
4. **Activate** the workflow and message your bot.

Every node carries a sticky-note explanation, so reading the workflow teaches you how it works
rather than hiding it.

### AI model

Defaults to OpenAI `gpt-4o-mini` — pennies per conversation. Prefer free and fully private? Point it
at a local Ollama model by changing two lines in CONFIG.

## Lite vs Pro

**This repo is the Lite version and it is complete and usable on its own** — qualify, score, notify.

The [**Pro version**](https://mdebrand.gumroad.com/l/lead-qualifying-chatbot-pro) adds the parts you
need once leads are actually arriving:

- A full briefing note on each lead, not a one-liner
- In-chat `/claim` and a **two-way relay** — your rep talks to the customer without either of them
  changing channel
- `/done` resume, with long-term memory for returning customers
- A timeout fallback with a booking link, so a lead who waits too long still converts
- Per-session audit log, and delivery-failure recovery

## Also on the n8n template library

[Qualify and score Telegram leads with OpenAI, Google Sheets and Gmail](https://n8n.io/workflows/19028-qualify-and-score-telegram-leads-with-openai-google-sheets-and-gmail/)

## License

MIT — see [LICENSE](LICENSE). Use it commercially, modify it, ship it in client work.
(That covers *this workflow file*. n8n itself is licensed separately by n8n.)
