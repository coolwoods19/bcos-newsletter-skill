# WhatsApp Newsletter Format Reference

## Formatting rules

WhatsApp uses its own lightweight markup — NOT Markdown, NOT HTML.

- **Bold**: Wrap text in asterisks → `*bold text*`
- **Italic**: Wrap text in underscores → `_italic text_`
- **Strikethrough**: Wrap text in tildes → `~strikethrough~`
- **Monospace**: Wrap text in backticks → `` `monospace` ``
- **Links**: Paste raw URLs — WhatsApp auto-links them. No markdown-style `[text](url)`.

## Structure for BCOS announcements

The WhatsApp version should be short, scannable, and emoji-driven. It is meant to be copy-pasted into a WhatsApp group chat. Keep the total length under ~1500 characters so it doesn't get truncated on small screens.

### Template

```
🐻 *Berkeley Club of Singapore*
📬 {{MONTH_RANGE}} {{YEAR}} Events

---

⭐ *{{FEATURED_TITLE}}*
{{FEATURED_DESCRIPTION_SHORT — max 1 sentence}}
📆 {{DATE}} · {{TIME}}
📍 {{VENUE}}
💰 {{COST — if applicable}}
🔗 {{RSVP_LINK}}

---

🎯 *{{EVENT_TITLE}}*
{{EVENT_DESCRIPTION_SHORT}}
📆 {{DATE}} · {{TIME}}
📍 {{VENUE}}
💰 {{COST}}
🔗 {{REGISTER_LINK}}

(Repeat for each confirmed event)

---

👀 *Coming Soon:* {{Comma-separated list of teaser event names}}

---

🔗 Not a member yet? Join here: https://international.berkeley.edu/club/singapore/get-involved-our-club/

📸 Follow us: instagram.com/sgucberkeley
💼 Connect: linkedin.com/company/berkeleyclubofsingapore/
```

### Key principles

1. **Lead with the featured event** — marked with ⭐
2. **One event per block** — separated by line breaks, not horizontal rules (WhatsApp doesn't render `---`, so use blank lines)
3. **Short descriptions** — 1 sentence max per event. WhatsApp is for quick info, not long reads.
4. **Raw links only** — no hyperlink text. Paste the full URL so it's tappable.
5. **Emojis for structure** — use 📆 for date, 📍 for venue, 💰 for cost, 🔗 for links consistently.
6. **No HTML entities** — everything is plain text UTF-8.
7. **Teaser events** — collapse into a single "Coming Soon" line with names only (no dates/venues since they're not confirmed).
8. **Footer** — membership link + social links. Keep it to 2-3 lines.

### Emoji mapping for event types

| Event Type | Emoji |
|---|---|
| Featured / Headline | ⭐ |
| Collab event | 🤝 |
| Signature event | 🎯 |
| Social / Networking | 🥂 |
| Sports / Active | 🏓 |
| Wellness / Health | 🧘 |
| Tech / AI | 🤖 |
| Professional / Career | 🎓 |
| Gala / Formal | 🎉 |

Choose the most fitting emoji per event based on its content.
