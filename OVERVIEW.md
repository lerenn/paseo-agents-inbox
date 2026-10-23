Agents Inbox is where your agents ask you things. An agent that needs a decision, an approval or information only you have calls a tool, ends its turn, and the question shows up in an Inbox screen. You answer; the agent is woken with your answer.

## How it works

New agents get these MCP tools:

- `inbox_ask(question, options?, context?, kind?, blocking?, expires_in_minutes?, default_answer?, dedupe_key?)` asks you something and returns an item id. `kind: "approval"` offers Approve and Deny. Without options you answer in free text. Asking the same question again while it is pending returns the same item, so retries are safe.
- `inbox_status(item_id)`, `inbox_list(status?)` show the caller's own items.
- `inbox_withdraw(item_id, reason?)` takes back a pending question.

A blocking question (the default) tells the agent to end its turn. A non-blocking one lets it carry on and be told later. When you answer, the agent receives a message starting with "Agents Inbox:" carrying your choice and comment. An answer is recorded once: a second answer is refused and the first stands. If the message cannot be delivered, the answer stays recorded and delivery is retried with backoff (an archived or vanished agent is marked undeliverable instead); it is held while the agent has a permission request pending.

An item can expire. With a `default_answer` the default is recorded as the answer (marked as default) and the agent is woken; without one the agent is told it expired. If the asking agent is archived its pending questions are withdrawn. Agents cannot answer, and Inbox does not replace Paseo's permission prompts.

## Setup

By default every new agent gets the tools; create an agent with `PASEO_INBOX=off` to opt out, or set the scope to opt-in and use `PASEO_INBOX=on`. Agents created before Inbox was installed do not have the tools. The tools are served on `127.0.0.1:47825`; set `INBOX_PORT` on the daemon to change the port. Requires Paseo 0.11.2 or later.

## Screen

The Agents Inbox sidebar item shows an unread count (pending questions you have not opened). The screen has Pending, History and Audit tabs; expand a row to read the context and pick an option or write an answer. It works on a phone.

## Settings

Scope, tick interval, the gap between messages to agents, delivery attempts, a default expiry, retention, and the answer and expiry message templates (`{{question}}`, `{{id}}`, `{{answer}}`, `{{by}}`, `{{comment}}`).

## Data

Items and the audit log are stored in `~/.paseo/plugin-data/agents-inbox/state.json` on the daemon machine and survive restarts. Finished items and audit entries are removed after the retention period (30 days by default). Question text and context are stored as the agents wrote them, so agents should not put secrets in them. Inbox sends messages only to agents on the same daemon and makes no network calls.
