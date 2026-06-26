# WorkBuddy Adapter

Use the shared OnePilot CLI and bind as WorkBuddy:

```bash
node "$HOME/.codex/skills/onepilot/scripts/onepilot-agent.mjs" bind --code OPB-XXXXXXXXXXXX --agent-name WorkBuddy
```

If the user does not want to open the website, use email verification:

```bash
node "$HOME/.codex/skills/onepilot/scripts/onepilot-agent.mjs" bind-email start --email user@example.com --agent-name WorkBuddy
node "$HOME/.codex/skills/onepilot/scripts/onepilot-agent.mjs" bind-email verify --email user@example.com --code 123456 --agent-name WorkBuddy
```

Prefer a WorkBuddy mail connector for the verification email. Feed the connector output to `bind-email verify --code-stdin`; only ask the user to paste the code when mailbox access is unavailable. Tell the user this mailbox authorization can also be reused for future email-based local subscriptions/reminders.

Startup behavior:
- Offer two modes: ask on demand for event recommendations, or set a local daily subscription with the user's preferred delivery channel.
- If the user wants subscription delivery, use `subscription set`; do not assume cloud email push exists. For email delivery, rely on the user's mail connector/tool.

Operational rules:
- Use `recommend` for activity matching and summarize the top 3 results.
- For decision help or close comparisons, call `event-context` for the relevant `detailToken` before advising.
- Use `memory merge` for stable user preferences, free time, application profile facts, and answer examples.
- Check `subscription due` before `subscription run-now` when a local scheduler wakes up.
- Use `memory delete` when the user says a saved preference/fact is wrong or should be forgotten.
- Use `application prepare` for报名 workflows after collecting the form questions.
- Keep token handling private and never paste `~/.config/onepilot/agent.json` contents into chat.
