# Puzzle Reference — Caravan

## Access
- URL: https://app.puzzle.io
- Login: walt@thecaravan.ai
- Password: in 1Password Walt vault → item: **"Puzzle.io"**
- Role: Employee
- Entity: teebeedeeai, LLC

## API Status
- **No individual customer API available as of April 2026**
- Platform/distribution API exists but not yet open to individual accounts
- Mercury confirmed building individual API — ETA ~1 month from April 2026
- API access requested via support@puzzle.io on April 10, 2026
- All access currently via web login only

## Connected data sources
- Mercury (bank transactions sync automatically)
- Stripe (payment sync)
- Gusto (payroll sync)

## Chart of accounts (key items)
- Dual-basis setup (Accrual + Cash)
- Accrual is default for all reporting
- Deferred Revenue = deposits received for future trainings
- AR (Accounts Receivable) = second payments outstanding after training

## Accrual accounting rules
- Revenue recognized on **training date**, not payment date
- Deposit received → Deferred Revenue (liability)
- Training happens → move Deferred Revenue to Revenue
- Outstanding second payment → AR

## Reconciliation status (as of March 31, 2026)
- Mercury Checking ✅ Reconciled
- Mercury Savings ✅ Reconciled
- Reserve Fund ✅ Reconciled
- Mercury Credit Card: $61.74 gap (IO AUTOPAY timing issue — Mercury debited Apr 1, Puzzle dated Mar 31)

## Revenue to recognize (as of April 2026)
These trainings have occurred but revenue NOT yet recognized in Puzzle:
| Client | Training Date | Status |
|---|---|---|
| Culton Companies | Mar 3 | Second Invoice — unknown payment status |
| Moultrie | Mar 12 | FULLY PAID (EBSCO $4K Feb + $4K Apr) |
| SPM | Mar 19 | Deposit only ($4K) — owes second $4K |
| CrewOS | Mar 19 | Second Invoice — unknown payment status |
| Centennial | Mar 31 | Second Invoice — unknown payment status |
| Community Foundation | Apr 2 | Second Invoice — unknown payment status |
| OHD | Apr 2 | Deposit only ($4K) — owes second $4K |

## What to do in Puzzle (manual until API)
1. For each completed training: move deposit from Deferred Revenue → Revenue
2. For outstanding second payments: record as AR
3. Uncategorized items still pending responses from Austin/Robert on ~15 transactions

## Categorization rules already set up in Puzzle
- Anthropic, Vistage, LinkedIn, Mercury sub fee, OpenAI → auto-categorize
- Still needed: GitHub, DigitalOcean, Tailscale, Obsidian, Cal.com, Loom, Slack, Zoom, Livekit, Supabase, Lovable, Elevenlabs, Superhuman, Canva, 1Password, Contabo, Google Workspace, HubSpot, OpenPhone, eBay, Intl Transaction Fee
