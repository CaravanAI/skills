---
name: caravan-ar
description: >
  Caravan accounts receivable tracker. Use when Brandon asks who owes money, what's
  outstanding, what's overdue, AR status, collections, unpaid invoices, or "what needs
  follow-up." Pulls live from both Stripe and Mercury. Triggers on: "who owes us",
  "what's outstanding", "open invoices", "AR", "overdue", "collections", "past due",
  "what needs follow-up", "unpaid".
---

# Caravan AR Tracker

Pulls live open/unpaid invoices from Stripe AND Mercury and shows what's outstanding.

## How to run

SSH to Mac and run:
```
source ~/.zshrc
cd ~/.openclaw/workspace-brandon/caravan/finance/
python3 ar_tracker.py
```

## What it shows

- **OVERDUE** — invoices past due date, sorted by days overdue (most urgent first)
- **PENDING** — invoices sent but not yet due
- **Totals** — total outstanding and total overdue

## Data sources

- **Stripe** — all open invoices (corporate training final payments)
- **Mercury** — all unpaid AR invoices (legacy and ongoing)

## Known exclusions

- INV-9 (Community Foundation deposit) — paid by check, not yet marked in Mercury

## Follow-up actions

When overdue invoices are found, suggest:
1. For Mercury invoices: check if payment was received but not recorded
2. For Stripe invoices: resend the invoice if it's been more than 2 weeks
3. Flag anything 30+ days overdue to Brandon for direct outreach

## Script location

`/Users/caravan/.openclaw/workspace-brandon/caravan/finance/ar_tracker.py`
Also in: `CaravanAI/training` repo → `finance/ar_tracker.py`
