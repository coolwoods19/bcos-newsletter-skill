# BCOS Newsletter Skill for Claude Code

A [Claude Code](https://claude.ai/claude-code) skill that generates Berkeley Club of Singapore (BCOS) newsletters from event pipeline data.

## What it does

Given a Google Sheets event pipeline, it produces two ready-to-send outputs:

1. **HTML email newsletter** — Gmail-compatible, table-based layout in UC Berkeley colors (Berkeley Blue + California Gold)
2. **WhatsApp summary** — plain-text, emoji-structured, copy-pasteable for group chats

## Installation

```bash
curl -L https://github.com/coolwoods19/bcos-newsletter-skill/archive/refs/heads/main.zip -o /tmp/bcos.zip \
  && unzip -o /tmp/bcos.zip -d /tmp/ \
  && mkdir -p ~/.claude/skills/bcos-newsletter \
  && cp -r /tmp/bcos-newsletter-skill-main/* ~/.claude/skills/bcos-newsletter/
```

Then restart Claude Code. The skill will appear as `/bcos-newsletter`.

## Usage

1. Open Claude Code in any project
2. Type `/bcos-newsletter`
3. Share your Google Sheets event pipeline link (or paste event data directly)
4. Claude generates both files and saves them to your project folder

## Event pipeline format

**[View example Google Sheet](https://docs.google.com/spreadsheets/d/1xOAJ7F949EGqE7_kaLQNC29q46rapQYvsSBWa9Y161A/edit?usp=sharing)** — make a copy and use it as your template.

The skill expects a Google Sheet with these columns:

| Column | Content |
|--------|---------|
| A | Event name + description (may include venue, cost, links) |
| B | Date & Time |
| C | Registration link |
| D | Event Type (`Signature` or `Collab`) |
| E | Person In Charge |
| F | Status (`Marketing` = confirmed, `Planning` = teaser only) |

## Output

- **Featured event** — most prominent confirmed event, shown as a dark blue hero card
- **Confirmed events** — `Marketing` status, shown as gold-bordered cards with registration links
- **On the Horizon** — `Planning` status, listed as teasers without dates/links
- **Save the Date** — any major future event with a specific date (e.g. Annual Gala)

## Adapting for your club

To use this for a different alumni club, update the following in `SKILL.md`:

- Club name and branding colors
- Co-Presidents names and LinkedIn links
- Fixed links (membership, Instagram, LinkedIn, website)
- HTML template in `assets/template.html`

## Files

```
bcos-newsletter/
├── SKILL.md                        # Skill instructions for Claude
├── assets/
│   └── template.html               # HTML email template with placeholders
├── references/
│   └── whatsapp-format.md          # WhatsApp formatting guide
└── evals/
    └── evals.json                  # Test cases
```

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI
- Google Sheet must be publicly accessible (or paste data manually)
