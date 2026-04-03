---
name: bcos-newsletter
description: Generate Berkeley Club of Singapore (BCOS) newsletters from event pipeline data. Produces two outputs — (1) a polished HTML email newsletter and (2) a WhatsApp-friendly text summary. Use this skill whenever the user asks to create, draft, or generate a BCOS newsletter, event update, event announcement, WhatsApp blast, or email blast for Berkeley Club of Singapore. Also trigger when the user shares a Google Sheets link or event list and mentions BCOS, Berkeley Club, or newsletter. Even if the user just says "create the newsletter" or "send out the event update" — if BCOS context is present (from memory or conversation), use this skill.
---

# BCOS Newsletter Skill

Generate event newsletters for the Berkeley Club of Singapore from raw event data. Produces two deliverables every time:

1. **HTML email newsletter** — Gmail-compatible, table-based layout in UC Berkeley colors
2. **WhatsApp summary** — plain-text, emoji-structured, copy-pasteable for group chats

## Step 1: Fetch and parse event data

The user will share a Google Sheets link to the BCOS event pipeline. Fetch the sheet content using `web_fetch` on the URL provided.

Parse the spreadsheet table to extract events. Each row typically contains:
- **Column A** — Event name and description (often long-form with details embedded)
- **Column B** — Date & Time
- **Column C** — Event Type (`Collab` or `Signature`)
- **Column D** — Person In Charge
- **Column E** — Status (`Marketing` = confirmed and ready to promote, `Planning` = teaser only)

If the fetch doesn't return usable data (Google Sheets can be inconsistent), ask the user to paste the event details directly or upload a CSV export.

## Step 2: Classify events

Sort each event into one of three categories:

| Category | Criteria | How it appears in newsletter |
|---|---|---|
| **Featured** | The single most prominent confirmed event — typically a Signature event with a notable speaker, major partnership, or special venue. If ambiguous, ask the user which event to feature. | Dark blue hero card at top |
| **Confirmed** | Status = `Marketing`, has specific date + venue + registration link. Excludes the featured event. | Gold-bordered event cards |
| **Teaser** | Status = `Planning`, or lacks a firm date/venue/link | "On the Horizon" list items |

Within each category, sort events chronologically (earliest first).

**Extracting details from Column A**: The event description in Column A often contains embedded details like venue, cost, registration links, and deadlines. Parse these carefully:
- Look for patterns like `Venue :`, `Cost :`, `📍`, `📆`, `⏰`, `🎟️`, `RSVP:`, `register by`, `registration:`, URLs
- Extract registration/RSVP links (URLs starting with `http`)
- Extract cost/ticket price
- Extract venue address
- Extract registration deadlines

## Step 3: Generate the HTML email

Read the HTML template from `assets/template.html` in this skill's directory. This template contains the locked-in BCOS design with `{{placeholder}}` variables.

Do NOT modify the template's structure, colors, fonts, or layout. Only fill in the content placeholders.

### Assembly rules

1. **Header**: Set `{{MONTH_RANGE}}` and `{{YEAR}}` based on the date span of events being promoted.

2. **Intro message**: Write 1–2 warm sentences previewing what's inside. Tone: friendly, community-driven, alumni voice. Not corporate. Example: "It's shaping up to be a packed season for our community in Singapore. From cross-school socials to a landmark visit by our Haas Dean, here's everything on the calendar — mark your dates and join us!"

3. **Featured event**: Fill the dark blue hero card. Write a 2–3 sentence description that captures why this event matters. Include the gold "RSVP NOW" button.

4. **Confirmed event cards**: For each additional confirmed event, duplicate the event card block. Include:
   - Type label (e.g., "Collab · HSBA", "Collab · Multi-School Social")
   - Title
   - Short 1–2 sentence description
   - Date, time, cost, venue
   - Registration deadline if mentioned
   - "Register Now" button with link
   - For the last card, change bottom padding from `16px` to `28px`

5. **On the Horizon**: List teaser events with an appropriate emoji, title, approximate timeframe, and optional note (e.g., collaboration partner). Include a "Save the Date" block if any major future event has a specific date (like the Annual Gala).

6. **Fixed sections** (do NOT change):
   - Membership CTA banner (gold, links to sign-up page)
   - Footer with Co-Presidents (Chris Fong + Erica Ding with LinkedIn links)
   - Social links (Instagram, LinkedIn, Website)
   - Unsubscribe line

### Fixed links (always use these)

| Purpose | URL |
|---|---|
| Membership sign-up | https://international.berkeley.edu/club/singapore/get-involved-our-club/ |
| Instagram | https://www.instagram.com/sgucberkeley |
| LinkedIn | https://www.linkedin.com/company/berkeleyclubofsingapore/ |
| Website | https://international.berkeley.edu/club/singapore/ |
| Chris Fong LinkedIn | https://www.linkedin.com/in/cfong/ |
| Erica Ding LinkedIn | https://www.linkedin.com/in/ericading/ |

### HTML technical requirements

- All styles must be inline (no `<style>` blocks) for Gmail compatibility
- Table-based layout only — no `<div>` for structure
- Max width: 640px
- Colors: Berkeley Blue `#003262`, California Gold `#FDB515`
- Fonts: Georgia for body, Helvetica/Arial for labels and UI elements
- All links must have `target="_blank"`
- Use HTML entities for emojis in the HTML version (e.g., `&#128197;` for 📆)

## Step 4: Generate the WhatsApp summary

Read `references/whatsapp-format.md` for the full formatting guide.

Key rules:
- Use WhatsApp formatting: `*bold*`, `_italic_` — NOT Markdown or HTML
- Raw URLs only (no hyperlinks)
- Lead with featured event marked ⭐
- One event per block, separated by blank lines
- Max 1 sentence description per event
- Collapse teaser events into a single "Coming Soon" line
- End with membership link + social links
- Keep total under ~1500 characters

## Step 5: Output both files

1. Save the HTML newsletter to `/mnt/user-data/outputs/bcos-newsletter.html`
2. Save the WhatsApp summary to `/mnt/user-data/outputs/bcos-whatsapp.txt`
3. Present both files to the user using `present_files`
4. After presenting, add a brief note:
   - For the HTML: "Open this file in your browser, Ctrl+A to select all, Ctrl+C to copy, then paste into Gmail compose."
   - For the WhatsApp: "Copy the text and paste directly into your WhatsApp group chat."

## Edge cases

- **No featured event is obvious**: Ask the user which event to highlight as the hero.
- **Only 1 confirmed event**: Use it as the featured event. Skip the "More This Month" section.
- **No teaser events**: Skip the "On the Horizon" section entirely.
- **No confirmed events**: Don't generate. Tell the user there's nothing ready to promote yet.
- **Google Sheets won't load**: Ask the user to paste event details or upload a CSV.
- **Event has no registration link**: Include the event card but omit the CTA button. Add a note like "Details coming soon."
