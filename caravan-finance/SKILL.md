---
name: caravan-finance
description: >
  Caravan finance operations. Use this skill for anything related to Caravan's money, training
  revenue, invoicing, or financial close. Triggers on: "new training", "create invoice",
  "lock in [client]", "run the close", "monthly financials", "who owes us", "open invoices",
  "1099s", "contractor payments", "revenue this month", "what's our cash", "close the books",
  "accrual", "recognize revenue", "training payment", "deposit received".
---

# Caravan Finance

Finance and revenue operations for Caravan (teebeedeeai, LLC).

## The Mac is where everything runs

All scripts live on the Mac Mini via Tailscale SSH:

```
ssh caravan@100.74.167.73
cd ~/.openclaw/workspace-brandon/caravan/finance/
```

Always `source ~/.zshrc` before running scripts (loads 1Password service account token).
All credentials are pulled from 1Password vault "Walt" at runtime — never ask Brandon for keys.

## Data sources

| Source  | Role                              | Access                    |
|---------|-----------------------------------|---------------------------|
| Stripe  | Invoicing, payments, webhooks     | API key in 1Password      |
| Mercury | Bank accounts, ACH payments       | API token in 1Password    |
| Puzzle  | Accounting system of record       | Web login (API pending)   |
| D1      | Training records, attendees       | Cloudflare Worker engine  |
| Notion  | Master training list (syncs to D1)| API key in 1Password      |

## Accrual accounting rules

- Revenue is recognized on the **training date**, not when cash is received
- Deposit received → book as **Deferred Revenue** (liability) in Puzzle
- Training date arrives → move from Deferred Revenue to **Revenue**
- Reimbursements are NEVER income — always separate Mercury transactions with "Reimbursement" in memo
- Puzzle is the accounting source of truth. Mercury and Stripe are supporting evidence.

## Training payment structure

- Corporate trainings: $8,000 standard
  - 50% deposit invoice due immediately on signing
  - 50% final payment due 7 days before training date
- Public trainings: $1,000/seat via Stripe payment link
  - Full payment at registration
  - Revenue recognized on training date

## Workflows

### Create a new corporate training invoice

When Brandon says "lock in [client]", "new training for [client]", or similar:

1. Confirm: client name, contact email, total amount, training date, training name
2. SSH to Mac and run:
   ```
   source ~/.zshrc
   cd ~/.openclaw/workspace-brandon/caravan/finance/
   python3 create_invoice.py \
     --client "[name]" \
     --email "[email]" \
     --amount [amount] \
     --training-date "[YYYY-MM-DD]" \
     --training-name "[name]"
   ```
3. Script creates Stripe customer + 2 invoices (deposit + final)
4. Return both invoice URLs to Brandon — these go to the client

### Run the monthly close

When Brandon asks for monthly financials or to run the close:

1. SSH to Mac and run: `source ~/.zshrc && cd ~/.openclaw/workspace-brandon/caravan/finance/ && python3 monthly_close.py [YEAR] [MONTH]`
2. Review output: cash position, Stripe revenue, open invoices
3. Flag any open invoices or overdue payments
4. Remind Brandon to verify Puzzle manually (API access pending)
5. Label close PROVISIONAL until Puzzle is confirmed

### Check who owes money

When Brandon asks "who owes us", "open invoices", "AR", "what's outstanding", or "collections":

1. SSH to Mac and run: `source ~/.zshrc && cd ~/.openclaw/workspace-brandon/caravan/finance/ && python3 ar_tracker.py`
2. Script pulls open invoices from both Stripe AND Mercury automatically
3. Shows overdue (sorted by days overdue) and pending separately with totals
4. Flag anything overdue to Brandon with suggested follow-up

### Year-end 1099s

When Brandon asks about contractor 1099s (run in January):

1. Run `calculate_1099s.py [YEAR]`
2. All Mercury outgoing payments grouped by contractor
3. Anything with "reimbursement" in memo is excluded from 1099 total
4. Output: each contractor's taxable income, flag anyone above $600 threshold

## Current open items (as of April 2026)

- Community Foundation: deposit received, final payment pending
- SPM: second payment overdue
- 15 uncategorized transactions in Puzzle pending response from Austin/Robert
- D1 migration (accrual fields) pending Cloudflare auth — ask Austin to run `wrangler login` on Mac

## Revenue recognition checklist (run on 1st of each month)

- [ ] Pull training list from D1 — which trainings happened last month?
- [ ] For each completed training: mark `revenue_recognized = 1`, set `revenue_recognition_date`
- [ ] Move corresponding deferred revenue to revenue in Puzzle
- [ ] Verify deposit + final payment received or flag outstanding
