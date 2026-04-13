# Notion API Reference — Caravan

## Authentication
- API key stored in 1Password Walt vault → item: **"Caravan - Notion API"**, field: `credential`
- Pull via: `op item get "Caravan - Notion API" --vault Walt --fields label=credential --reveal`
- Header: `Authorization: Bearer {key}`, `Notion-Version: 2022-06-28`
- Base URL: `https://api.notion.com/v1`

## Training database
- **Used by the engine** — syncs every 15 minutes automatically
- The engine reads from Notion and writes back status changes

## Training status workflow
```
Tentative → Open → First Invoice → Booked → Second Invoice → Done
```
- **Tentative / Open / Unbooked**: not in D1, ignored by engine
- **First Invoice**: training confirmed → engine creates Stripe deposit invoice
- **Booked**: deposit paid (Stripe webhook updates this automatically)
- **Second Invoice**: training complete, final invoice sent (engine cron does this automatically)
- **Done**: fully paid (Stripe webhook updates this automatically)
- **Abandoned**: cancelled → engine archives the D1 record

## Notion fields the engine reads
| Field | Type | Used for |
|---|---|---|
| Company | Title | Client name |
| Status | Status | Training workflow state |
| Training Date | Date | Revenue recognition, final invoice trigger |
| Type | Select | Public vs Corporate |
| Location | Select | Where training is held |
| Lead Trainer | People | Assigned trainer |
| Guide | Select | Training guide used |
| Stripe Payment Link | URL | Public training payment link |
| Contact Email | Email | Stripe invoice recipient for corporate |
| Proposal Amount | Number | Invoice amount (in dollars, stored as cents in D1) |

## Key engine behaviors driven by Notion
1. **New training at "First Invoice"** + Contact Email + Proposal Amount → Stripe deposit invoice auto-created
2. **Status moves back to Tentative** → D1 record archived (recoverable)
3. **Training deleted from Notion** → D1 record archived (recoverable)
4. **Notion sync**: every 15 minutes via Cloudflare Worker cron

## Writing back to Notion (engine does this automatically)
- Deposit paid → Status set to "Booked"
- Final invoice sent → Status set to "Second Invoice"
- Final payment received → Status set to "Done"

## API endpoints used
```
POST /databases/{id}/query    — fetch all trainings (paginated)
PATCH /pages/{id}             — update training status
```

## Knowledge vault
- Location on Mac: `/Users/caravan/Documents/knowledge-vault/`
- Git-backed, Obsidian-style markdown
- Contains: company info, EOS docs, contacts, training materials, processes
