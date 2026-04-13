---
name: caravan-monthly-close
description: >
  Run Caravan's monthly financial close. Use when Brandon asks for monthly financials,
  wants to close the books, needs a P&L, or wants to check close status. Pulls live
  from Mercury and Stripe. Puzzle is manual until API arrives (~May 2026).
  Triggers on: "run the close", "monthly financials", "close the books", "P&L",
  "balance sheet", "cash flow", "monthly close", "financial package".
---

# Caravan Monthly Financial Close

Runs the end-to-end monthly close from live sources.

## How to run

SSH to Mac and run:
```
source ~/.zshrc
cd ~/.openclaw/workspace-brandon/caravan/finance/
python3 monthly_close.py 2026 3    # for March 2026
python3 monthly_close.py           # current month
```

Or use the shell runner:
```
./run_close.sh 2026 3
```

## What it produces

1. **Cash position** — all Mercury account balances
2. **Revenue activity** — Stripe payments, payouts, refunds, open invoices
3. **Open invoices (AR)** — who still owes money
4. **Puzzle checklist** — manual steps required in Puzzle
5. **Status** — PROVISIONAL until Puzzle is verified

## Accrual accounting rules

- Revenue recognized on **training date**, not payment date
- Deposit received → Deferred Revenue in Puzzle
- Training happens → move to Training Revenue in Puzzle
- Outstanding second payments → AR in Puzzle

## Puzzle (manual until API)

Puzzle API expected ~May 2026. Until then, manually in Puzzle after running the script:
1. Rename "Transaction Revenue" → "Training Revenue" (one-time)
2. For each completed training: recognize revenue on training date
3. Record outstanding second payments as AR

## Revenue to recognize (as of April 2026)

| Client | Training Date | Action in Puzzle |
|---|---|---|
| Culton Companies | Mar 3 | Recognize $8K, $4K AR |
| Moultrie | Mar 12 | Recognize $8K (fully paid via EBSCO) |
| SPM | Mar 19 | Recognize $8K, $4K AR |
| Centennial | Mar 31 | Recognize $8K, $4K AR |
| Community Foundation | Apr 2 | Recognize $8K, $4K AR |
| OHD | Apr 2 | Recognize $8K (fully paid) |

## Script location

`/Users/caravan/.openclaw/workspace-brandon/caravan/finance/monthly_close.py`
