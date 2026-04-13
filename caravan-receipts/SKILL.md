---
name: caravan-receipts
description: >
  Check expense receipt compliance for Caravan. Use when Brandon asks about receipts,
  expenses, who submitted receipts, what's missing, or wants to see Walt's inbox for
  expense emails. Triggers on: "receipts", "expenses", "who sent receipts", "missing
  receipts", "receipt compliance", "check Walt's inbox", "expense tracking".
---

# Caravan Expense Receipt Tracker

Checks receipt compliance using Mercury's built-in policy fields and scans Walt's inbox
for receipt emails from the team.

## How to run

SSH to Mac and run:
```
source ~/.zshrc
cd ~/.openclaw/workspace-brandon/caravan/finance/
python3 receipt_matcher.py        # last 30 days
python3 receipt_matcher.py 60     # last 60 days
```

## What it shows

1. **Missing Receipts** — transactions Mercury flags as non-compliant with no receipt on file
2. **Receipts on File** — transactions covered (Mercury receipt, attachment, or marked compliant)
3. **Receipt Emails in Walt's Inbox** — emails from team that may contain receipts not yet uploaded

## How the receipt workflow works

1. Team members (Brandon, Austin, Robert, Michael) email receipts to `walt@thecaravan.ai`
2. Walt scans inbox and uploads receipts to the matching Mercury transaction
3. Mercury marks the transaction as `compliantWithReceiptPolicy = true`
4. This script confirms everything is covered

## What to do with results

- **Missing receipts**: Reply to the team member asking for the receipt for that specific
  transaction (date + amount + vendor)
- **Inbox emails**: For each receipt email, upload the attachment to the matching
  Mercury transaction at app.mercury.com → Transactions → find transaction → Add receipt
- **No response after 3 days**: Escalate to Brandon

## Script location

`/Users/caravan/.openclaw/workspace-brandon/caravan/finance/receipt_matcher.py`
Also in: `CaravanAI/training` repo → `finance/receipt_matcher.py`
