---
name: caravan-receipts
description: >
  Process expense receipts for Caravan. Use when Brandon asks about receipts,
  expenses, who submitted receipts, what's missing, or wants to review pending
  expenses. Also run proactively when checking Walt's inbox. Triggers on:
  "receipts", "expenses", "check expenses", "process receipts", "what's in
  Walt's inbox", "missing receipts", "expense review".
---

# Caravan Expense Receipt Processing

Walt reads receipt emails, interprets each expense using judgment, and records
them appropriately. No rigid vendor rules — use context from who sent it, what
the receipt says, and what Caravan does.

## Workflow

### Step 1 — Check Walt's inbox for receipt emails

Search Gmail for recent receipt submissions:
- From: brandon@thecaravan.ai, austin@thecaravan.ai, robert@thecaravan.ai, michael@thecaravan.ai
- Keywords: receipt, invoice, expense, meal, purchase, order
- Period: last 30 days

Read each email fully. Extract: vendor, amount, date, submitter, purpose (if stated).

### Step 2 — Interpret each expense

Use judgment. Context you have:
- **Who submitted it** — Brandon (ops/finance), Austin (CEO/visionary), Robert (brand/marketing), Michael (sales)
- **What Caravan does** — AI training company, B2B, client-facing work in Birmingham and nationally
- **What the receipt looks like** — restaurant = meals, software = subscriptions, Amazon/office = supplies

Common expense categories:
| Type | Category in Puzzle |
|---|---|
| Restaurant, catering | Meals (Office) or Meals (Other) |
| Client entertainment | Meals (Other) — note the client |
| Software subscriptions | Software - Operating Expense |
| Office supplies, printing | Computers & Hardware or Supplies |
| Marketing materials | Other Sales & Marketing |
| Professional memberships | Dues & Subscriptions |
| Travel | Travel |

If unclear, use the submitter's role as a hint. Michael's expenses are likely sales/marketing.
Robert's are likely brand/marketing. Austin's are likely ops or general business.

**Do not ask Brandon to categorize obvious expenses.** Use judgment. Only flag
genuinely ambiguous ones (e.g., a $2,500 vendor payment with no context).

### Step 3 — Check Mercury transactions for receipt compliance

Run the compliance check:
```
source ~/.zshrc
cd ~/.openclaw/workspace-brandon/caravan/finance/
python3 receipt_matcher.py
```

Cross-reference emailed receipts with transactions needing coverage.

### Step 4 — Record in Puzzle (manual until API)

For each receipt processed:
1. Note the category, amount, date, submitter, and any client/purpose
2. If uploading to Mercury: go to the transaction in Mercury and attach the receipt
3. If going directly to Puzzle: log it as an expense entry with the receipt attached

### Step 5 — Flag anything unusual

Bring to Brandon's attention if:
- Amount is over $500 with no clear business purpose
- Expense doesn't fit Caravan's typical spend patterns
- Submitter note says something that needs a decision (e.g., "not sure if this is expensable")
- Receipt is missing key info (no amount, no vendor, no date)

## Current receipt emails in Walt's inbox (as of April 13, 2026)

- Michael Johnson — "Business Card Order" (Apr 13)
- Brandon Stewart — "Urban Cookhouse receipt" (Apr 6) — likely client meal
- Brandon — "Puzzle receipt #2231-5090" (Apr 4) — software subscription
- Robert Hill — "Client meals" (Apr 3) — meals, client-facing
- Michael Johnson — "Gamma receipt #2113-6312" (Apr 3) — software subscription

## Expense policy reference

- Receipts required for all card transactions
- Reimbursements to contractors: separate Mercury ACH with "Reimbursement" in memo
- Reimbursements never appear on contractor 1099s
- Full expense policy: https://docs.google.com/document/d/1SP0CGAzuFxnEtJ-FZZHwS2bnMJZxT81ecp5jhSMsrow/edit
