# Stripe API Reference — Caravan

## Authentication
- Restricted key stored in 1Password Walt vault → item: **"Stripe API Credentials"**, field: `credential`
- Key starts with `rk_live_` (restricted, read + payment links write + invoicing write)
- Header: `Authorization: Bearer {key}`
- Base URL: `https://api.stripe.com/v1`

## Key permissions on restricted key
- Charges: Read
- Invoices: Read/Write
- Customers: Read/Write
- Payment Links: Read/Write
- Payouts: Read
- Refunds: Read
- Webhook Endpoints: needs Write added if modifying webhooks

## Caravan account
- Account ID: `acct_1SVDW12dNp3wAxWl`
- All live keys, no test mode in production

## Active payment links (public trainings)
```
plink_1TK83h2dNp3wAxWlatwXmB6b  — May 28 training
plink_1TJF6T2dNp3wAxWlpXgem1t3  — May 5 training
plink_1TC7N12dNp3wAxWlvQX9Bjhr  — Overton Fellowship Apr 17
plink_1TBxGG2dNp3wAxWlam8dPqeC  — Chicago Apr 20-21
plink_1T4n472dNp3wAxWlTGH1i9ZU  — Apr 28 training
```
All links collect: Phone (native), Company (custom field), Full Name (native, split into first/last in code)

## Webhook
- Endpoint: `https://caravan-engine.brandon-f00.workers.dev/api/stripe-webhook`
- ID: `we_1TEt3R2dNp3wAxWlItHV5bUw`
- Events: `checkout.session.completed`, `invoice.payment_succeeded`

## Key endpoints used

### Invoices (corporate training billing)
```
POST /invoices           — create invoice
POST /invoiceitems       — add line item
POST /invoices/{id}/finalize
POST /invoices/{id}/send
GET  /invoices?status=open
GET  /invoices?status=paid
```

### Customers
```
GET  /customers/search?query=email:'x@y.com'
POST /customers
```

### Payment links
```
GET  /payment_links
POST /payment_links/{id}   — update (custom_fields, phone_number_collection)
```

## Corporate training invoice structure
- Deposit: 50% of proposal amount, due in 7 days
- Final: 50%, created and sent automatically after training date via engine cron
- Both invoices include metadata: `training_id`, `invoice_type` (deposit/final_payment)

## Finance scripts (on Mac)
- `pull_stripe.py [YEAR] [MONTH]` — charges, invoices, payouts, refunds
- `create_invoice.py --client X --email Y --amount Z --training-date YYYY-MM-DD` — manual invoice creation

## HubSpot pipeline IDs (for reference)
- Pipeline: Public Training = `2181227201`
- Stage: Closed Won = `3488419515`
- Product: Single Seat - Group Training = `238717382357`
