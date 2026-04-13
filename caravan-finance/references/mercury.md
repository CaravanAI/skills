# Mercury API Reference — Caravan

## Authentication
- Token stored in 1Password Walt vault → item: **"Mercury API"**, field: `credential`
- Pull via: `op item get "Mercury API" --vault Walt --fields label=credential --reveal`
- Header: `Authorization: Bearer {token}`

## Base URLs
- Transactions/Accounts: `https://backend.mercury.com/api/v1`
- Invoicing (AR): `https://api.mercury.com/api/v1/ar`

## Accounts
```
GET /accounts
```
Returns all Mercury accounts with current balances.
Fields: `id`, `name`, `accountNumber` (last 4 = `[-4:]`), `currentBalance`, `kind`

Caravan accounts:
- Mercury Checking ••8000
- Mercury Savings ••2432
- Mercury Savings (Reserve) ••2952

## Transactions
```
GET /account/{accountId}/transactions?start=YYYY-MM-DD&end=YYYY-MM-DD&limit=500&status=sent
```
Fields: `amount` (positive=inbound, negative=outbound), `postedAt`, `counterpartyName`, `externalMemo`, `bankDescription`, `status`

## Invoicing API (AR)
```
GET  /api/v1/ar/invoices          — list all invoices
POST /api/v1/ar/invoices          — create invoice
GET  /api/v1/ar/invoices/{id}     — get invoice
PUT  /api/v1/ar/invoices/{id}     — update invoice
DEL  /api/v1/ar/invoices/{id}     — cancel invoice
GET  /api/v1/ar/customers         — list customers
POST /api/v1/ar/customers         — create customer
```
**Note:** Invoicing API requires Mercury subscription plan AND invoicing scope on the API token.
Current token status: needs invoicing scope enabled. Update token in Mercury → Settings → API Keys.

## Finance scripts (on Mac)
Path: `/Users/caravan/.openclaw/workspace-brandon/caravan/finance/`
- `pull_mercury.py [YEAR] [MONTH]` — balances + transactions
- `monthly_close.py [YEAR] [MONTH]` — full close package
- `calculate_1099s.py [YEAR]` — contractor 1099 amounts

## Key business rules
- Reimbursements to contractors: always separate Mercury transactions with "Reimbursement" in memo — excluded from 1099
- Corporate training deposits typically $4,000 (50% of $8,000)
- Birmingham AI $10K loan (Mar 2026) = short-term debt, not revenue
- EBSCO Industries = Moultrie's parent company (payments from EBSCO = Moultrie)
