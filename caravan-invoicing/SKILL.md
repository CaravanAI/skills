---
name: caravan-invoicing
description: >
  Create Stripe invoices for Caravan training clients. Use when Brandon says a new
  training is confirmed, wants to send an invoice, or needs to lock in a client.
  Triggers on: "new training", "create invoice", "lock in [client]", "send invoice",
  "invoice [company]", "we booked [client]", "confirmed training".
---

# Caravan Invoicing

Creates Stripe invoices for corporate training clients. All invoices use ACH bank
transfer only (no credit card). Two invoices per engagement: deposit (50%) and final
payment (50% after training).

## Standard training structure

- **Total fee**: $8,000 (standard corporate training)
- **Deposit**: $4,000 — due immediately on confirmation
- **Final payment**: $4,000 — sent automatically after training date via engine cron
- **Payment method**: ACH bank transfer only
- **Footer**: "Payment by ACH bank transfer. Questions? Email brandon@thecaravan.ai"

## What I need to create an invoice

1. Client company name
2. Contact email
3. Total training fee (default $8,000)
4. Training date (YYYY-MM-DD)
5. Training name (optional — defaults to "AI Foundations Workshop")

## How to run manually

SSH to Mac and run:
```
source ~/.zshrc
cd ~/.openclaw/workspace-brandon/caravan/finance/
python3 create_invoice.py \
  --client "Acme Corp" \
  --email "contact@acme.com" \
  --amount 8000 \
  --training-date "2026-06-15" \
  --training-name "AI Foundations Workshop"
```

## Automated flow (via Notion)

The engine handles this automatically when a training moves to "First Invoice" in Notion:
1. Add training to Notion with status "First Invoice"
2. Fill in "Contact Email" and "Proposal Amount" fields
3. Engine creates Stripe deposit invoice on next 15-min sync
4. Review and send from Stripe dashboard

## Script location

`/Users/caravan/.openclaw/workspace-brandon/caravan/finance/create_invoice.py`

## Stripe account details

- Platform account: `acct_1SVDW12dNp3wAxWl`
- Webhook: `caravan-engine.brandon-f00.workers.dev/api/stripe-webhook`
- Events: `checkout.session.completed`, `invoice.payment_succeeded`
